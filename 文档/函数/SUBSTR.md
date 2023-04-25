# SUBSTR

[TOC]

**substr(char, position, substring_length)**

返回 char 的一部分，从 position 位置的字符开始，长 substring_length 个字符。 【一列转多列】

按照字符计算长度。

```sql
SQL> select substr('abcdef', 1, 2) from dual;

SUBSTR('ABCDEF',1,2)
--------------------
ab
```

- 如果 position 为 0，就将其当作 1

	```sql
	SQL> select substr('abcdef', 0, 2) from dual;

	SUBSTR('ABCDEF',0,2)
	--------------------
	ab
	```

- 如果 position 为正数，那么从 char 的第一个字符开始计算

	```sql
	SQL> select substr('abcdef', 2, 4) from dual;

	SUBSTR('ABCDEF',2,4)
	--------------------
	bcde
	```

- 如果 position 为负数，那么从 char 的最后一个字符倒着计算

	```sql
	SQL> select substr('abcdef', -2, 2) from dual;

	SUBSTR('ABCDEF',-2,2)
	---------------------
	ef

	SQL> select substr('abcdef', -2, 4) from dual;

	SUBSTR('ABCDEF',-2,4)
	---------------------
	ef

	SQL> select substr('abcdef', -4, 2) from dual;

	SUBSTR('ABCDEF',-4,2)
	---------------------
	cd
	```

- 如果忽略 substring_length 参数，那么就返回直到结尾的所有字符。如果 substring_length 小于1，那么返回 null

	```sql
	SQL> select substr('abcdef', 2) from dual;

	SUBSTR('ABCDEF',2)
	------------------
	bcdef

	SQL> select substr('abcdef', 2, 0) from dual;

	SUBSTR('ABCDEF',2,0)
	--------------------

	```

参数 position 和 substring_length 必须是 NUMBER 数据类型，或能隐式转换成 NUMBER 的任意数据类型，且必须能被解析成整型。

```sql
SQL> select substr('abcdef', 1.1, 2) from dual;

SUBSTR('ABCDEF',1.1,2)
----------------------
ab

SQL> select substr('abcdef', 1.1, 2.6) from dual;

SUBSTR('ABCDEF',1.1,2.6)
------------------------
ab
```