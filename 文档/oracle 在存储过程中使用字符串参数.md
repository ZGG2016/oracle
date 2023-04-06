# oracle 在存储过程中使用字符串参数


```sql
# task_name 的内容是一个字符串
CREATE OR REPLACE NONEDITIONABLE PROCEDURE test
(task_id IN VARCHAR2,task_name IN VARCHAR2)
is
insert_sql varchar2(2000);
begin
    insert_sql := 'insert into testtb 
                       select '||task_id||' as task_id,
                              '''||task_name||''' as task_name,
                       ....';
    execute immediate insert_sql;
    commit;
end test;
```

如果设置错误，会报错: 

```sh
SQL> EXEC test('6921372193','tasktest',...);
....

ORA-00904: "tasktest": 标识符无效
ORA-06512: 在 "SYSTEM.test", line 29
ORA-06512: 在 line 1
```