# REGEXP_SUBSTR

[TOC]

**regexp_substr(source_char, pattern, position, occurrence, match_param, subexpr)**

扩展了 substr 函数的功能，能够使用正则表达式搜索字符串。

- source_char: 用来搜索字符串的源字符表达式 

- pattern: 正则表达式。如果类型和 source_char 类型不一致，会将其转成 source_char 类型。

- position: 开始搜索 source_char 的字符位置，默认是1，意味着是从 source_char 的第一个字符开始搜索。

- occurrence: 取搜索到的第 occurrence 次出现的子串。默认是1，意味着搜索 pattern 第一次出现的子串。

- match_param: 改变函数默认的匹配行为

	- `i`: 大小写不敏感的匹配

	- `c`: 大小写敏感的匹配

	- `n`: 允许点号`.`. 它是一个匹配任意字符的字符。如果省略此参数，则点号与换行符不匹配。

	- `m`: 将源字符串视为多行. Oracle 将 `^` 和 `$` 分别解释为源字符串中任意行的开始和结束，而不仅仅是整个源字符串的开始或结束。如果省略此参数，则 Oracle 将源字符串视为单行。

	- `x`: 忽略空格。默认空格匹配自身。

	如果忽略这个参数：

	- 由 NLS_SORT 参数决定了是否大小写敏感
	- 点号与换行符不匹配
	- 将源字符串视为单行

- subexpr: 对于具有子表达式的 pattern, 它是从0到9的非负整数，表示返回 pattern 中的哪个子表达式。

```sql
-- 以逗号开头和结尾，中间是非逗号的字符
SQL> select regexp_substr('500 Oracle Parkway, Redwood Shores, CA', ',[^,]+,') as "REGEXP_SUBSTR" from dual;

REGEXP_SUBSTR
-----------------
, Redwood Shores,


SQL> select regexp_substr('1234567890','(123)(4(56)(78))', 1, 1, 'i', 1) as "REGEXP_SUBSTR" from dual;

REGEXP_SUBSTR
-------------
123

SQL> select regexp_substr('1234567890','(123)(4(56)(78))', 1, 1, 'i', 2) as "REGEXP_SUBSTR" from dual;

REGEXP_SUBSTR
-------------
45678

SQL> select regexp_substr('1234567890','(123)(4(56)(78))', 1, 1, 'i', 3) as "REGEXP_SUBSTR" from dual;

REGEXP_SUBSTR
-------------
56

SQL> select regexp_substr('1234567890','(123)(4(56)(78))', 1, 1, 'i', 4) as "REGEXP_SUBSTR" from dual;

REGEXP_SUBSTR
-------------
78

-- occurrence: 取搜索到的第 occurrence 次出现的子串。默认是1，意味着搜索 pattern 第一次出现的子串。
SQL> select regexp_substr('abefabq','ab[a-z]', 2, 2, 'i') as "REGEXP_SUBSTR" from dual;

REGEXP_SUBSTR
-------------


SQL> select regexp_substr('abefabq','ab[a-z]', 2, 1, 'i') as "REGEXP_SUBSTR" from dual;

REGEXP_SUBSTR
-------------
abq

-- `c`: 大小写敏感的匹配
SQL> select regexp_substr('abefabq','AB[a-z]', 1, 1, 'c') as "REGEXP_SUBSTR" from dual;

REGEXP_SUBSTR
-------------

SQL> select regexp_substr('abefabq','AB[a-z]', 1, 1, 'i') as "REGEXP_SUBSTR" from dual;

REGEXP_SUBSTR
-------------
abe
```