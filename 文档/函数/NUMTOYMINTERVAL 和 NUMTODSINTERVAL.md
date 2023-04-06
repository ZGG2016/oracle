# NUMTOYMINTERVAL 和 NUMTODSINTERVAL

[TOC]

## NUMTOYMINTERVAL

**NUMTOYMINTERVAL(n, 'interval_unit')**

将数值 `n` 转成 `INTERVAL YEAR TO MONTH ` 时间间隔字面量。

参数 `n` 可以是 `NUMBER` 类型，或能隐式转换成 `NUMBER` 值的表达式。

参数 `interval_unit` 可以是 `CHAR`、`VARCHAR2`、`NCHAR` 或 `NVARCHAR2` 类型，其值只能是 `YEAR` 和 `MONTH`，大小写不敏感。

参数 `interval_unit` 指定了参数 `n` 的单位。

```sql
SQL> select sysdate from dual;

SYSDATE
-----------
2022/4/26 1

SQL> select sysdate + numtoyminterval(1, 'YEAR')  from dual;

SYSDATE+NUMTOYMINTERVAL(1,'YEAR')
---------------------------------
2023/4/26 11:59:26

SQL> select sysdate + numtoyminterval(-1, 'YEAR')  from dual;

SYSDATE+NUMTOYMINTERVAL(-1,'YEAR')
----------------------------------
2021/4/26 11:59:36

SQL> select sysdate + numtoyminterval(1, 'month')  from dual;

SYSDATE+NUMTOYMINTERVAL(1,'MONTH')
----------------------------------
2022/5/26 12:01:38

SQL> select sysdate + numtoyminterval(1+1, 'month')  from dual;

SYSDATE+NUMTOYMINTERVAL(1+1,'MONTH')
------------------------------------
2022/6/26 12:02:38
```

官网原文：[https://docs.oracle.com/cd/E11882_01/server.112/e41084/functions118.htm#SQLRF00683](https://docs.oracle.com/cd/E11882_01/server.112/e41084/functions118.htm#SQLRF00683)

## NUMTODSINTERVAL

**NUMTODSINTERVAL(n, 'interval_unit')**

将数值 `n` 转成 `INTERVAL DAY TO SECOND` 时间间隔字面量。

参数 `n` 可以是 `NUMBER` 类型，或能隐式转换成 `NUMBER` 值的表达式。

参数 `interval_unit` 可以是 `CHAR`、`VARCHAR2`、`NCHAR` 或 `NVARCHAR2` 类型，其值只能是 `DAY`、`HOUR`、`MINUTE` 和 `SECOND`，大小写不敏感。

参数 `interval_unit` 指定了参数 `n` 的单位。

```sql
SQL> select sysdate + numtodsinterval(1, 'DAY')  from dual;

SYSDATE+NUMTODSINTERVAL(1,'DAY')
--------------------------------
2022/4/27 14:25:53

SQL> select sysdate + numtodsinterval(-1, 'HOUR')  from dual;

SYSDATE+NUMTODSINTERVAL(-1,'HOUR')
----------------------------------
2022/4/26 13:26:19
```


官网原文：[https://docs.oracle.com/cd/E11882_01/server.112/e41084/functions117.htm#SQLRF00682](https://docs.oracle.com/cd/E11882_01/server.112/e41084/functions117.htm#SQLRF00682)