# LAST_DAY 和 NEXT_DAY

## LAST_DAY 

**last_day(date)**

取 date 所在月的最后一天，返回值类型总是 DATE

月的最后一天是由会话参数 NLS_CALENDAR 定义的

```sql
SQL> select sysdate, last_day(sysdate) as "last_day", last_day(sysdate) - sysdate as "days left" from dual;

SYSDATE     last_day     days left
----------- ----------- ----------
2023/5/4 10 2023/5/31 1         27
```


## NEXT_DAY

**next_day(date, char)**

返回 date 之后的第一个名 char 的日期，返回值类型总是 DATE

char 必须是你的会话的日期语句中的一周的周几，要么是全名称要么是简写。

```sql
SQL> select sysdate from dual;

SYSDATE
-----------
2023/5/4 10

SQL> select NAME,VALUE from v$parameter where name='nls_date_language';

NAME                           VALUE
------------------------------ ------------------------------
nls_date_language              SIMPLIFIED CHINESE

SQL> select next_day(sysdate,'星期二') AS "next_day" from dual;

next_day
-----------
2023/5/9 10

SQL> alter session set nls_date_language='american';

Session altered

SQL> select NAME,VALUE from v$parameter where name='nls_date_language';

NAME                           VALUE
------------------------------ ------------------------------
nls_date_language              american

SQL> select next_day(sysdate,'TUESDAY') AS "next_day" from dual;

next_day
-----------
2023/5/9 10

SQL> select next_day(sysdate,'TUE') AS "next_day" from dual;

next_day
-----------
2023/5/9 10
```

```sql
-- 5.7 星期日
SQL> select next_day(sysdate,1) AS "next_day" from dual;

next_day
-----------
2023/5/7 10

SQL> select next_day(sysdate,2) AS "next_day" from dual;

next_day
-----------
2023/5/8 10

-- 5.7 星期六
SQL> select next_day(sysdate,7) AS "next_day" from dual;

next_day
-----------
2023/5/6 10

SQL> select next_day(sysdate,8) AS "next_day" from dual;
select next_day(sysdate,8) AS "next_day" from dual

ORA-01846: 周中的日无效
```
