# EXTRACT

**extract(year|month|day|hour|minute|second|timezone_hour|timezone_minute|timezone_region|timezone_abbr) from expr)**

从一个 datetime 或 interval 表达式中，抽取并返回指定的 datetime 字段的值。

```SQL
SQL> SELECT EXTRACT(YEAR FROM DATE '1998-03-07') from dual;

EXTRACT(YEARFROMDATE'1998-03-07')
---------------------------------
                             1998

SQL> SELECT EXTRACT(YEAR FROM INTERVAL '5-9' YEAR TO MONTH) "EXTRACT" from dual;

   EXTRACT
----------
         5

SQL> SELECT EXTRACT(MONTH FROM INTERVAL '5-9' YEAR TO MONTH) "EXTRACT" from dual;

   EXTRACT
----------
         9 
         
SQL> SELECT EXTRACT(MONTH FROM TIMESTAMP '1998-03-07 12:13:14') "EXTRACT" from dual;

   EXTRACT
----------
         3

SQL> SELECT EXTRACT(MINUTE FROM TIMESTAMP '1998-03-07 12:13:14') "EXTRACT" from dual;

   EXTRACT
----------
        13

SQL> SELECT EXTRACT(TIMEZONE_HOUR FROM SYSTIMESTAMP) "EXTRACT" from dual;

   EXTRACT
----------
         8                                             
```