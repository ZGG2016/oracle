# ADD_MONTHS

**add_months(date, integer)**

将日期 date 加 integer 月。

月份由 NLS_CALENDAR 会话参数（默认值是GREGORIAN）定义。

```sql
SQL> select add_months(TO_DATE('19991020','YYYYMMDD'), 2) from dual;

ADD_MONTHS(TO_DATE('19991020','YYYYMMDD'),2)
--------------------------------------------
1999/12/20


SQL> select add_months(TO_DATE('19991020','YYYYMMDD'), -2) from dual;

ADD_MONTHS(TO_DATE('19991020','YYYYMMDD'),-2)
---------------------------------------------
1999/8/20
```

date 参数可以是 datetime 值，或是能隐式地转换成 DATE 的值。

```sql
SQL> select add_months(TIMESTAMP'1999-10-01 10:00:00', 1) from dual;

ADD_MONTHS(TIMESTAMP'1999-10-0110:00:00',1)
-------------------------------------------
1999/11/1 10:00:00

SQL> select add_months('19991020', 2) from dual;

ADD_MONTHS('19991020',2)
------------------------
1999/12/20
```

integer 参数可以是整型，或是能隐式地转换成整型的值。

无论 date 的数据类型是什么，返回值的类型总是 DATE

如果 date 是一个月的最后一天，或者结果月份的天数比 date 的 day 部分少，那么结果是结果月份的最后一天。否则，结果就是 date 的 day 部分。

```sql
SQL> select add_months('19990930', 2) from dual;

ADD_MONTHS('19990930',2)
------------------------
1999/11/30


SQL> select add_months('20230430', 1) from dual;

ADD_MONTHS('20230430',1)
------------------------
2023/5/31
```

