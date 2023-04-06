# 使用 cx_Oracle 将数据批量导入 oracle


此页面包含详细内容：[https://cx-oracle.readthedocs.io/en/latest/user_guide/batch_statement.html](https://cx-oracle.readthedocs.io/en/latest/user_guide/batch_statement.html)

遇到的问题：

1. `列在此处不允许`

2. `ORA-01841: (完整) 年份值必须介于 -4713 和 +9999 之间, 且不为 0`

3. `ORA-01008: 并非所有变量都已绑定`

解决：

在导入日期类型的字段时，转格式的时候错误。比如：

```python
 sql = '''INSERT INTO TEST
                    values (:1,:2,to_date(:3,'YYYY-MM-DD HH24:MI:SS'))'''
```                    	

上面的语句是正确的，如果写成 `"YYYY-MM-DD HH24:MI:SS"` 就会报错。

如果在处理数据中，给日期类型的数据添加了单引号，会报错。

如果列出的字段数量不对应，会报错。