# ROUND

## ROUND(DATE)

**round(date, fmt)**

将 date 近似到 fmt 指定的单元。

如果忽略了 fmt 参数，就近似到最近一天。

```sql
SQL> select round(to_date('2023-05-04','YYYY-MM-DD'), 'year') from dual;

ROUND(TO_DATE('2023-05-04','YYYY-MM-DD'),'YEAR')
------------------------------------------------
2023/1/1

SQL> select round(to_date('2023-05-04','YYYY-MM-DD'), 'MONTH') from dual;

ROUND(TO_DATE('2023-05-04','YYYY-MM-DD'),'MONTH')
-------------------------------------------------
2023/5/1

SQL> select round(to_date('2023-05-24','YYYY-MM-DD'), 'MONTH') from dual;

ROUND(TO_DATE('2023-05-24','YYYY-MM-DD'),'MONTH')
-------------------------------------------------
2023/6/1

SQL> select round(to_date('2023-05-24','YYYY-MM-DD'), 'DD') from dual;

ROUND(TO_DATE('2023-05-24','YYYY-MM-DD'),'DD')
----------------------------------------------
2023/5/24

SQL> select round(to_date('2023-05-04','YYYY-MM-DD'), 'DD') from dual;

ROUND(TO_DATE('2023-05-04','YYYY-MM-DD'),'DD')
----------------------------------------------
2023/5/4

SQL> select round(to_date('2023-05-23','YYYY-MM-DD'), 'DAY') from dual;

ROUND(TO_DATE('2023-05-23','YYYY-MM-DD'),'DAY')
-----------------------------------------------
2023/5/21

SQL> select round(to_date('2023-05-24','YYYY-MM-DD'), 'DAY') from dual;

ROUND(TO_DATE('2023-05-24','YYYY-MM-DD'),'DAY')
-----------------------------------------------
2023/5/21

SQL> select round(to_date('2023-05-26','YYYY-MM-DD'), 'DAY') from dual;

ROUND(TO_DATE('2023-05-26','YYYY-MM-DD'),'DAY')
-----------------------------------------------
2023/5/28
```

## ROUND(NUMBER)

**round(n, integer)**

将 n 四舍五入到小数点右侧第 integer 个位置。

如果忽略了 integer 参数，那么将 n 近似到 0 位置。如果是负数，近似到小数点左侧。 

```sql
SQL> select round(163.163, 1) from dual;

ROUND(163.163,1)
----------------
           163.2

SQL> select round(163.163, 2) from dual;

ROUND(163.163,2)
----------------
          163.16

SQL> select round(163.163, 0) from dual;

ROUND(163.163,0)
----------------
             163

SQL> select round(163.163) from dual;

ROUND(163.163)
--------------
           163 

SQL> select round(163.163,-1) from dual;

ROUND(163.163,-1)
-----------------
              160

SQL> select round(163.163,-2) from dual;

ROUND(163.163,-2)
-----------------
              200

SQL> select round(163.163,-3) from dual;

ROUND(163.163,-3)
-----------------
                0                    
```

- n 是0，总是返回0
- n 是负数，返回 -round(-n, integer)
- n 是正数，返回 `floor(n*power(10,integer)+0.5) * power(10,-integer)`

```sql
SQL> select round(0) from dual;

  ROUND(0)
----------
         0

SQL> select round(-163.163, 1) from dual;

ROUND(-163.163,1)
-----------------
           -163.2
```