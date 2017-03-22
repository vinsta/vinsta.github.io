---
layout: post
title:  "Python中glob模块的用法"
categories: python
excerpt: 简单介绍python中glob的用法。
tags:  python glob 
---

* content
{:toc}

glob主要用于查找符合要求的文件名，主要提供了两个方法：glob()和iglob()。
这两个方法以一个字符串为输入参数，返回匹配的文件名。
输入参数可以支持“*"、"?"和"[]"三个通配符
- ”*“表示0个或者多个字符
- ”?"表示1个字符
- "[]"匹配范围内的字符，比如[0-9]匹配0-9这10个数字

## 首先需要导入glob模块

```
import glob
```

## glob()返回所有匹配的文件名列表
下面是在IDLE中的测试结果:

```
>>>glob.glob("*")
>>>['DLLs', 'Doc', 'include', 'Lib', 'libs', 'LICENSE.txt', 'NEWS.txt', 'python.exe', 'python3.dll', 'python36.dll', 'pythonw.exe', 'Scripts', 'tcl', 'Tools', 'vcruntime140.dll']
>>>
>>>glob.glob("*.dll")
>>>['python3.dll', 'python36.dll', 'vcruntime140.dll']
>>>
```

## iglob()返回一个表达式，它是一个可遍历的对象
在IDLE中的测试结果:

```
>>> glob.iglob("*")
<generator object _iglob at 0x00000169A14353B8>
>>>
>>> [i for i in glob.iglob("*")]
['DLLs', 'Doc', 'include', 'Lib', 'libs', 'LICENSE.txt', 'NEWS.txt', 'python.exe', 'python3.dll', 'python36.dll', 'pythonw.exe', 'Scripts', 'tcl', 'Tools', 'vcruntime140.dll']
>>>
>>> [i for i in glob.iglob("*.dll")]
['python3.dll', 'python36.dll', 'vcruntime140.dll']
>>> 

