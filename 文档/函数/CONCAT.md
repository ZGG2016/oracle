# CONCAT

[TOC]

**concat(char1,char2)**

char1 和 char2 连接后，返回 char1 【多列转一列】

这个函数等价连接操作符 `||`


```sql
select concat( concat(last_name, '''s job catgory is '), job_id ) as "job"
    from hr.employees
where employee_id = 152;

job
-----------------------------------
Hall's job catgory is SA_REP
```

```sql
select last_name || '''s job catgory is ' || job_id as "job"
    from hr.employees
where employee_id = 152;
```

In concatenations of two different data types, Oracle Database returns the data type that results in a lossless conversion. 

```sql
-- SALARY	NUMBER
select concat( concat(last_name, '''s salary is '), salary ) "salary"
    from hr.employees
where employee_id = 152;

salary
-----------------------------------
Hall's salary is 9000
```
```sql
-- HIRE_DATE	DATE
select concat( concat(last_name, '''s hire date is '), HIRE_DATE ) "HIRE_DATE"
    from hr.employees
where employee_id = 152;

HIRE_DATE
-----------------------------------
Hall's hire date is 20-AUG-05
```