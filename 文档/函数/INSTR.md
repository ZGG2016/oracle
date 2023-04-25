# INSTR

[TOC]

**instr(string, substring, position, occurrence)**

在 string 中搜索 substring，如果搜索到了，返回 substring 的第一个字符在 string 中的位置，如果没有搜索到，返回0。

- position: 非0整数，开始搜索的位置。正数表示从前往后搜索，负数表示从后往前搜索。

- occurrence: 正整数，表示取 substring 在 string 中第 occurrence 次出现的子串。

参数 position 和 substring_length 必须是 NUMBER 数据类型，或能隐式转换成 NUMBER 的任意数据类型，且必须能被解析成整型。

参数 position 和 substring_length 的默认值都是1

```sql
SQL> select instr('CORPORATE FLOOR', 'OR', 3, 2) from dual;
Warning: connection was lost and re-established

INSTR('CORPORATEFLOOR','OR',3,2)
--------------------------------
                              14

SQL> select instr('CORPORATE FLOOR', 'OR', -3, 2) from dual;

INSTR('CORPORATEFLOOR','OR',-3,2)
---------------------------------
                                2                              
```