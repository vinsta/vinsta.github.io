---
layout: post
title:  "python中的正则表达式"
categories: python
tags: python regex
excerpt: 简单介绍Python中正则表达式的用法。
---

* content
{:toc}

Python的正则表达式模块位于python安装目录的`lib/re.py`。表达式可以包含特殊和普通字符。大多数普通字符只代表它们自己。

## 特殊字符
- "."：匹配除了新行（\n）之外的所有字符
- "^"：匹配字符串的开始
- "$"：匹配字符串的结束，或者字符串结束之前的新行（\n）
- "*"：以贪婪模式（尽可能多）匹配前面的表达式0次或任意多次，其非贪婪版本为"*?"
- "+"：以贪婪模式（尽可能多）匹配前面的表达式1次或任意多次，其非贪婪版本为"+?"
- "?"：以贪婪模式（尽可能多）匹配前面的表达式0次或1次，其非贪婪版本为"??"
- {m,n}：匹配m至n次，其非贪婪版本为{m,n}?
- "\\"：转义特殊字符，或者表示一个特殊序列
  - \number
  - \A：从字符串起始处匹配
  - \Z：从字符串结束处匹配
  - \b：匹配单词之前或者之后的空白字符
  - \B：匹配不是单词之前或者之后的空白字符
  - \d：匹配一个Unicode数字，或者带re.ASCII标记的[0-9]
  - \D：匹配一个Unicode非数字，或者带re.ASCII标记的[^0-9]
  - \s：匹配Unicode空白，或带re.ASCII标记的[\t\n\r\f\v]
  - \S：匹配Unicode非空白，或带re.ASCII标记的[^\t\n\r\f\v]
  - \w：匹配一个Unicode单词，或带re.ASCII标记的[a-zA-Z0-9_]
  - \W：匹配一个Unicode非单词，或带re.ASCII标记的[^a-zA-Z0-9_]
  - \\\：匹配"\\"字符
- []：表示一个字符集，若字符集以"^"开始，则表示字符集的补集
- "|"：A|B，匹配A或者B
- (...)：匹配括号中的表达式
- (?aiLmsux)：为表达式设置flag（A，I，L，M，S，U，X）
  - A：ASCII，使\w，\W，\b，\B，\d，\D匹配对应的ASCII字符
  - I：IGNORECASE，进行不区分大小写的匹配
  - L：LOCALE，使\w，\W，\b，\B与当前语言无关
  - M：MULTILINE
    - "^"匹配行或者字符串的起始位置
    - "$"匹配行或者字符串的结束位置
  - S：DOTALL，"."匹配任意字符，包括新行
  - U：UNICODE，用于兼容性，字符串模式下忽略，字节模式下禁用
  - X：忽略空格和注释
- (?P<name>...)：匹配的子串可以通过name访问
- (?P=name)：匹配之前命名的子串
- (?#...)：注释，可忽略
- (?=...)："..."匹配下一个
- (?!...)："..."不匹配下一个
- (?<=...)：前面不是"..."
- (?(id/name)yes|no)：如果"id/name"匹配，则匹配"yes"，否则匹配"no"（no是可选参数）

## 方法
### match(pattern, string flags = 0)
从字符串的起始处匹配表达式，返回一个匹配的对象或者None。可以调用对象的group()或者groups()方法来获取匹配的子串。

### fullmatch(pattern, string, flags = 0)
对于整个字符串匹配表达式，返回一个匹配的对象或者None。

### search(pattern, string, flags = 0)
根据给出的表达式搜索字符串，返回一个个匹配的对象或者None。

### sub(pattern, repl, string, count = 0, flags = 0)
从起始处开始替换字符串中匹配表达式的位置，返回替换完成的字符串。

### subn(pattern, repl, string, count = 0, flags = 0)
与sub类似，区别是返回替换完成的字符串和替换的次数。

### split(pattern, string, maxsplit = 0, flags = 0)
在匹配表达式的位置分割字符串，返回包含子串的列表。

### findall(pattern, string, flags = 0)
返回匹配表达式的所有子串列表。

### finditer(pattern, string, flags = 0)
对于所有匹配表达式的子串，返回一个迭代成器

### compile(pattern, flags = 0)
构造一个表达式对象，便于以后重复使用

## 实例
```python
>>> import re
>>> s = "hello123world321!!!"
>>> p = r'[a-zA-Z]+'
>>> m = re.match(p,s)
>>> m.group()
'hello'
>>> m.groups()
()
>>> m.group(0)
'hello'
>>> m.group(1)
Traceback (most recent call last):
  File "<pyshell#57>", line 1, in <module>
    m.group(1)
IndexError: no such group
>>> m = re.findall(p,s)
>>> m
['hello', 'world']
>>> m = re.finditer(p,s)
>>> m
<callable_iterator object at 0x0000000002F8F780>
>>> n = next(m)
>>> n
<_sre.SRE_Match object; span=(0, 5), match='hello'>
>>> n.group()
'hello'
>>> n.groups()
()
>>> n.group(0)
'hello'
>>> n.group(1)
Traceback (most recent call last):
  File "<pyshell#70>", line 1, in <module>
    n.group(1)
IndexError: no such group
>>> next(m).group()
'world'
>>> next(m).group()
Traceback (most recent call last):
  File "<pyshell#72>", line 1, in <module>
    next(m).group()
StopIteration
>>> 
>>> p = r'[a-zA-Z]*\d*'
>>> m = re.match(p,s)
>>> m.group()
'hello123'
>>> m.group(0)
'hello123'
>>> m.group(1)
Traceback (most recent call last):
  File "<pyshell#79>", line 1, in <module>
    m.group(1)
IndexError: no such group
>>> m.groups()
()
>>> m = re.findall(p,s)
>>> m
['hello123', 'world321', '', '', '', '']
>>> 
>>> p = r'([a-zA-Z]*)(\d*)'
>>> m = re.match(p,s)
>>> m.group()
'hello123'
>>> m.groups()
('hello', '123')
>>> m.group(0)
'hello123'
>>> m.group(1)
'hello'
>>> m.group(2)
'123'
>>> m.group(3)
Traceback (most recent call last):
  File "<pyshell#93>", line 1, in <module>
    m.group(3)
IndexError: no such group
>>> m = re.findall(p,s)
>>> m
[('hello', '123'), ('world', '321'), ('', ''), ('', ''), ('', ''), ('', '')]
>>> 
```
