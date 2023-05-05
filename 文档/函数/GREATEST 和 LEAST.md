# GREATEST 和 LEAST

[TOC]

## GREATEST

**GREATEST(expr,expr...)**

返回一个或多个表达式组成的列表中的最大值。

第一个参数决定了返回类型。

如果第一个 expr 是数字，在比较前，隐式地将剩余参数转换为数字。

如果第一个 expr 不是数字，在比较前，隐式地将第一个 expr 之后的每个 expr 转换成第一个 expr 数据类型。 

```sql
SQL> select greatest(1,'3.925','4.1') from dual;

GREATEST(1,'3.925','4.1')
-------------------------
                      4.1

SQL> select greatest(to_date('2023-04-11','YYYY-MM-DD'),to_date('2023-05-01','YYYY-MM-DD')) as GREATEST from dual;

GREATEST
-----------
2023/5/1

SQL> select greatest('a','b','c') from dual;

GREATEST('A','B','C')
---------------------
c

SQL> select greatest('a','ab','ac') from dual;

GREATEST('A','AB','AC')
-----------------------
ac
```

## LEAST

**GREATEST(expr,expr...)**

返回一个或多个表达式组成的列表中的最小值。

```sql
SQL> select least(1,'3.925','4.1') from dual;

LEAST(1,'3.925','4.1')
----------------------
                     1

SQL> select least(to_date('2023-04-11','YYYY-MM-DD'),to_date('2023-05-01','YYYY-MM-DD')) as LEAST from dual;

LEAST
-----------
2023/4/11 

SQL> select least('a','b','c') from dual;

LEAST('A','B','C')
------------------
a

SQL> select least('a','ab','ac') from dual;

LEAST('A','AB','AC')
--------------------
a                    
```