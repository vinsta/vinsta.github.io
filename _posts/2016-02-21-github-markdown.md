---
layout: post
title:  "Markdown基本语法"
categories: Tools
excerpt: Markdown简单使用手册
tags: tools
---

* content
{:toc}

## 标题设置
有两种方式设置标题：
- 在文字下方添加“=”或者“-”，分别表示一级标题和二级标题。
- 在文字开头加上“#”，通过“#”数量表示几级标题（支持1~6级标题，1级标题字体最大）

## 注释
- 在文字开头添加“>”表示注释  
实际效果：  
> Note：  

对应语法：  
```
> Note:
```
- 带层次的注释  
实际效果：  
> Note1:  
>   
>> Note2:  
>   
> Note1:

对应语法：  
```
> Note1:
> 
>> Note2:
> 
> Note1:
```
- 当“>”和文字之间添加五个空格时，格式会有变化  
实际效果：  
>     Note:  
>       

对应语法：  
```
><whitespace><withspace><whitespace><whitespace><whitespace>Note:
><whitespace><withspace><whitespace><whitespace><whitespace>
```

## 斜体
文字前后各添加1个`*`或者`_`：

*Note*

## 粗体
文字前后各添加2个`*`或者`_`：

**Note**

## 无序列表
在文字前面添加`*`、`+`、`-`实现无序列表。符号和文字之间需要添加空格。
- 符号和文字之间需要添加空格
- 一个文档中不要使用多种无序列表的符号

## 有序列表
以数字开头，后面加上点号和空格：
1. 列表一
2. 列表二
3. 列表三

## 链接（Links）
- 内联方式：
This is an [example link](http://example.com/).
对应语法：
```
This is an [example link](http://example.com/).
```

- 简单方式：  
<http://www.github.com>

<vinsta@github.com>

对应语法：  
`<http://www.github.com>`  
`<vinsta@gmail.com>`

## 图片（Images）
图片的处理方式和链接的处理方式，非常的类似。

![alt text](/path/to/img.jpg "Title")  
`![alt text](/path/to/img.jpg "Title")`  

## 代码
实现方式有两种：
- 简单文字出现一个代码框。使用\`code\`。（“\`”不是单引号而是左上角的ESC下面“~”中的“\`”）
`hello world!`
- 第二种：大片文字需要实现代码框，使用一对“```”。

## 分隔线
- 连续三个以空格分隔的“*”或者“-”号
- 连续三个无空格分隔的“*”（连续三个无分隔的“-”代表标题风格）

## 中划线
在要划线的文字前后分别加上两个“~”

~~划线内容~~

## 下划线

使用行内html来实现

<u>划线内容</u>
`<u>划线内容</u>`


