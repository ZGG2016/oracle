# TRUNC

## TRUNC(date, fmt)

returns date with the time portion of the day truncated to the unit specified by the format model `fmt`.

如果不指定 `fmt` 参数，默认使用 'DD'.

```sql
SQL> select sysdate from dual;

SYSDATE
-----------
2023/4/14 1

-- 取当年的第一天
SQL> select trunc(sysdate,'year') from dual;

TRUNC(SYSDATE,'YEAR')
---------------------
2023/1/1

-- 取当年的第一周的周一
SQL> select trunc(sysdate,'iy') from dual;

TRUNC(SYSDATE,'IY')
-------------------
2023/1/2

-- 取当前季度的第一天
SQL> select trunc(sysdate,'q') from dual;

TRUNC(SYSDATE,'Q')
------------------
2023/4/1

-- 取当前月的第一天
SQL> select trunc(sysdate,'month') from dual;

TRUNC(SYSDATE,'MONTH')
----------------------
2023/4/1

-- 取当天
SQL> select trunc(sysdate,'dd') from dual;

TRUNC(SYSDATE,'DD')
-------------------
2023/4/14

-- 取当周的周日
SQL> select trunc(sysdate,'d') from dual;

TRUNC(SYSDATE,'D')
------------------
2023/4/9

-- 取当前的小时
SQL> select trunc(sysdate,'hh') from dual;

TRUNC(SYSDATE,'HH')
-------------------
2023/4/14 10:00:00

-- 取当前的分钟
SQL> select trunc(sysdate,'MI') from dual;

TRUNC(SYSDATE,'MI')
-------------------
2023/4/14 10:19:00

-- 取当周的周日
SQL> select trunc(sysdate,'ww') from dual;

TRUNC(SYSDATE,'WW')
-------------------
2023/4/9

-- 取当周的周一
SQL> select trunc(sysdate,'iw') from dual;

TRUNC(SYSDATE,'IW')
-------------------
2023/4/10

SQL> select trunc(sysdate,'w') from dual;

TRUNC(SYSDATE,'W')
------------------
2023/4/8

SQL> select trunc(to_date(20221111,'yyyy-mm-dd'),'year') from dual;

TRUNC(TO_DATE(20221111,'YYYY-MM-DD'),'YEAR')
--------------------------------------------
2022/1/1
```

## TRUNC(n1,n2)

returns `n1` truncated to `n2` decimal places.

```sql
SQL> select trunc(15.79, 1) from dual;

TRUNC(15.79,1)
--------------
          15.7         

-- 如果忽略 n2 参数，就将 n1 截断到0位置。
SQL> select trunc(15.79) from dual;

TRUNC(15.79)
------------
          15

-- 如果 n2 是负数，截断小数点左侧的 n2 个长度
SQL> select trunc(15.79,-1) from dual;

TRUNC(15.79,-1)
---------------
             10

SQL> select trunc(1512.79,-2) from dual;

TRUNC(1512.79,-2)
-----------------
             1500

-- 函数可以接收任意的数值数据类型或任意非数值数据。
-- 非数值数据会被隐式转换为数值数据类型   
SQL> select trunc(1512.79,'1') from dual;

TRUNC(1512.79,'1')
------------------
            1512.7

SQL> select trunc(1512.79,'a') from dual;
select trunc(1512.79,'a') from dual

ORA-01722: 无效数字       
```