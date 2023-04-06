# LISTAGG

[TOC]

**listagg([all|ditinct] measure_expr, 'delimiter' listagg_overflow_clause) within group (order_by_clause) over(query_partition_clause)**

其中 listagg_overflow_clause:

- on overflow error
- on overflow truncate 'truncation-indicator' [with|without] count

工作流程是: 先分组（没有groupby，就所有数据都在一个组），然后在每个组内按照 order by 子句指定的顺序排序，最后再将列的值连接起来。

## 1 不同使用场景

1. As a single-set aggregate function, LISTAGG operates on all rows and returns a single output row.

	表中所有行输出一行值

	```sql
	select listagg(last_name, '; ')
	           within group (order by hire_date, last_name) "Emp_list",  -- 把所有数据当成一组，在这个组内执行order by
	       min(hire_date) "Earliest"
	from hr.employees
	where department_id = 30;

	Emp_list                                                     Earliest
	------------------------------------------------------------ ---------
	Raphaely; Khoo; Tobias; Baida; Himuro; Colmenares            07-DEC-02
	```

	执行计划如下
	```
	 Plan Hash Value  : 1249432893 

	-----------------------------------------------------------------------------------------------------
	| Id  | Operation                              | Name              | Rows | Bytes | Cost | Time     |
	-----------------------------------------------------------------------------------------------------
	|   0 | SELECT STATEMENT                       |                   |    1 |    19 |    2 | 00:00:01 |
	|   1 |   SORT GROUP BY                        |                   |    1 |    19 |      |          |
	|   2 |    TABLE ACCESS BY INDEX ROWID BATCHED | EMPLOYEES         |    6 |   114 |    2 | 00:00:01 |
	| * 3 |     INDEX RANGE SCAN                   | EMP_DEPARTMENT_IX |    6 |       |    1 | 00:00:01 |
	-----------------------------------------------------------------------------------------------------

	Predicate Information (identified by operation id):
	------------------------------------------
	* 3 - access("DEPARTMENT_ID"=30)
	```
2. As a group-set aggregate, the function operates on and returns an output row for each group defined by the GROUP BY clause.

	每个分组输出一行值
	
	```sql
	select department_id "Dept.", 
		   listagg(last_name,'; ')
			  within group(order by hire_date, last_name) "Employees" -- 在每个group by产生的分组内，执行order by
	from hr.employees
	group by department_id
	order by department_id;

	Dept. Employees
	------ ------------------------------------------------------------
	    10 Whalen
	    20 Hartstein; Fay
	    30 Raphaely; Khoo; Tobias; Baida; Himuro; Colmenares
	    40 Mavris
	    50 Kaufling; Ladwig; Rajs; Sarchand; Bell; Mallin; Weiss; Davie
	       s; Marlow; Bull; Everett; Fripp; Chung; Nayer; Dilly; Bissot
	       ; Vollman; Stiles; Atkinson; Taylor; Seo; Fleaur; Matos; Pat
	       el; Walsh; Feeney; Dellinger; McCain; Vargas; Gates; Rogers;
	        Mikkilineni; Landry; Cabrio; Jones; Olson; OConnell; Sulliv
	       an; Mourgos; Gee; Perkins; Grant; Geoni; Philtanker; Markle
	    60 Austin; Hunold; Pataballa; Lorentz; Ernst
	    70 Baer
	. . .
	```

	```
	 Plan Hash Value  : 2107619104 

	--------------------------------------------------------------------------
	| Id | Operation            | Name      | Rows | Bytes | Cost | Time     |
	--------------------------------------------------------------------------
	|  0 | SELECT STATEMENT     |           |  100 | 38700 |    4 | 00:00:01 |
	|  1 |   SORT GROUP BY      |           |  100 | 38700 |    4 | 00:00:01 |
	|  2 |    TABLE ACCESS FULL | EMPLOYEES |  100 | 38700 |    3 | 00:00:01 |
	--------------------------------------------------------------------------

	Notes
	-----
	- Dynamic sampling used for this statement ( level = 2 )
	```

3. As an analytic function, LISTAGG partitions the query result set into groups based on one or more expression in the `query_partition_clause`.

	把表数据按照 over 子句划分到不同组内（开窗），再组内排序、连接列的值。

	```sql
	select department_id "Dept", hire_date "Date", last_name "Name",
	    listagg(last_name, '; ')
	        within group(order by hire_date, last_name) 
	        over(partition by department_id) "Emp_list"
	from hr.employees
	where hire_date < '01-SEP-2003'
    order by "Dept", "Date", "Name";

    Dept  Date      Name            Emp_list
	----- --------- --------------- ---------------------------------------------
	   30 07-DEC-02 Raphaely        Raphaely; Khoo
	   30 18-MAY-03 Khoo            Raphaely; Khoo
	   40 07-JUN-02 Mavris          Mavris
	   50 01-MAY-03 Kaufling        Kaufling; Ladwig
	   50 14-JUL-03 Ladwig          Kaufling; Ladwig
	   70 07-JUN-02 Baer            Baer
	   90 13-JAN-01 De Haan         De Haan; King
	   90 17-JUN-03 King            De Haan; King
	  100 16-AUG-02 Faviet          Faviet; Greenberg
	  100 17-AUG-02 Greenberg       Faviet; Greenberg
	  110 07-JUN-02 Gietz           Gietz; Higgins
	  110 07-JUN-02 Higgins         Gietz; Higgins
	```

	```
	 Plan Hash Value  : 1919783947 

	--------------------------------------------------------------------------
	| Id | Operation            | Name      | Rows | Bytes | Cost | Time     |
	--------------------------------------------------------------------------
	|  0 | SELECT STATEMENT     |           |  107 |  2033 |    3 | 00:00:01 |
	|  1 |   WINDOW SORT        |           |  107 |  2033 |    3 | 00:00:01 |
	|  2 |    TABLE ACCESS FULL | EMPLOYEES |  107 |  2033 |    3 | 00:00:01 |
	--------------------------------------------------------------------------
	```

## 2 关键字

1. `ALL` : 可选的，默认。
2. `DISTINCT`: 移除组内的重复值

	```sql
	select listagg(distinct MANAGER_ID, '; ')
	           within group (order by MANAGER_ID) "MANAGERS"
	from hr.employees
	where department_id = 30;

	MANAGERS
	100; 114
	----------------------------------------------------------------

	select listagg(MANAGER_ID, '; ')
	          within group (order by MANAGER_ID) "MANAGERS"
	from hr.employees
	where department_id = 30;

	MANAGERS
	100; 114; 114; 114; 114; 114
	```

3. `measure_expr`: 度量列，可以是任意表达式。忽略列中的 Null 值。

	```sql
	select listagg(distinct MANAGER_ID+1, '; ')
	           within group (order by MANAGER_ID) "MANAGERS"
	from hr.employees
	where department_id = 30;

	MANAGERS
	101; 115
	```

4. `delimiter`: 用来划分分组里列的值。可选地，默认是 Null。

5. `order_by_clause`: 决定连接值的顺序。如果指定了这个子句，也必须指定 `WITHIN GROUP` 子句，反之亦然。

## 3 listagg_overflow_clause子句

返回数据类型的最大长度取决于 `MAX_STRING_SIZE` 属性。

- `MAX_STRING_SIZE = EXTENDED`: 最大长度是 32767 (VARCHAR2\RAW)
- `MAX_STRING_SIZE = STANDARD`: 最大长度是 4000(VARCHAR2), 2000(RAW)

当返回值超出了数据类型的最大长度时，执行 `listagg_overflow_clause` 子句。

- `ON OVERFLOW ERROR`: 返回 ORA-01489 错误。默认情况。

- `ON OVERFLOW TRUNCATE`: 返回截断的列表。

	- `truncation_indicator` 指定追加到截断列表的字符串，默认是 `...`
	- 在 `truncation_indicator` 后指定了 `WITH COUNT`，就会追加截断值的数量，用括号括起来。
	- 如果指定了 `WITHOUT COUNT`，就省略截断值的数量。
	- 如果不指定 `WITH COUNT` 或 `WITHOUT COUNT`，那么就默认 `WITH COUNT`

```sql
SELECT department_id "Dept.",
       LISTAGG(last_name, '; ' ON OVERFLOW TRUNCATE '...')
               WITHIN GROUP (ORDER BY hire_date) "Employees"
  FROM employees
  GROUP BY department_id
  ORDER BY department_id;

Dept. Employees
------ ------------------------------------------------------------
    10 Whalen
    20 Hartstein; Fay
    30 Raphaely; Khoo; Tobias; Baida; Himuro; Colmenares
    40 Mavris
    50 Kaufling; Ladwig; Rajs; Sarchand; Bell; Mallin; Weiss; Davie
       s; Marlow; Bull; Everett; Fripp; Chung; Nayer; Dilly; Bissot
       ; Vollman; Stiles; Atkinson; Taylor; Seo; Fleaur; ... (23)       -- 这里
    70 Baer
. . .
```