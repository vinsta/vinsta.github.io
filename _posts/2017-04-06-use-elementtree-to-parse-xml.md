---
layout: post
title:  "在python中使用ElementTree解析xml文件"
categories: python
tags: python xml elementtree
excerpt: 简单介绍在Python中使用ElementTree模块解析xml文件的方法。
---

* content
{:toc}

ElementTree是python自带的处理xml格式文件的模块，位于`lib\xml\etree\ElementTree.py`。这个模块有两个基本概念：
- ElementTree：表示整个树型结构的xml文档，对xml文件的读写操作通常使用此对象。
- Element：表示树型结构中的一个节点，包含了层次结构的数据，通过列表和字典来描述。对xml节点或者子节点的操作通常使用此对象。它包含下列属性：
  - tag：代表element名字的字符串
  - attrib：保存element所有属性的字典
  - text：包含element文本内容的字符串
  - tail：保护element结束符之后文本的可选字符串
  - 此外，还有一个sequence结构用于保存它的子element
`<tag attrib>text<child/>...</tag>tail`
Element的长度是它包含的所有子element的个数。

## 向element中插入一个子element
```python
insert(index, subelement)
```

## 删除一个子element
```python
remove(subelement)
```

## 查找element
- `find(path, namespace = None)`：path可以是element名字（tag），或者XPath。
- `findnext(path, default = None, namespace = None)：default是未找到element时的返回值。
- `findall(path, namespace = None)：返回包含所有匹配的element的列表。
- `iterfind(path, namespace = None)：返回包含所以匹配的element的迭代器。

## 清空
此方法删除所有的子element，attrib，设置text和tail为None
```python
clear()
```

## get(key, default = None)
根据key获取对应的属性，返回包含属性值的字符串。

## set(key, value)
设置element的属性值

## keys()
返回保护所有属性名称的列表。

## items()
返回包含所有（属性名称，属性值）的列表。

## iter(tag = None)
查询element和所有的子element，返回包含匹配的element的迭代器。

## 其它
- cursor.executemany(sql, seq_of_parameters)：对 seq_of_parameters 中的所有参数或映射执行一个 SQL 命令。
- cursor.executescript(sql_script)：一旦接收到脚本，会执行多个 SQL 语句。它首先执行 COMMIT 语句，然后执行作为参数传入的 SQL 脚本。所有的 SQL 语句应该用分号（;）分隔。
- connection.total_changes()：返回自数据库连接打开以来被修改、插入或删除的数据库总行数。
