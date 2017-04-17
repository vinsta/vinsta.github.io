---
layout: post
title:  "python中的几种文件对话框"
categories: python
tags: python dialog
excerpt: 简单介绍Python中几种文件对话框。
---

* content
{:toc}

Python的Tkinter模块提供了下面几种常用的文件对话框，位于python安装目录的`lib/tkinter/filedialog.py`。

## FileDialog
基本的文件选择对话框，界面类似于文件管理器，左边是文件夹列表，右边是当前目录下的文件列表。
使用`go()`方法可以返回选定文件的路径。
```python
filename = tkinter.FileDialog(root).go()
```

## LoadFileDialog
从FileDialog派生，外观跟FileDialog相同，区别在于它在退出之前会校验选择的文件是否存在。
```python
filename = tkinter.LoadFileDialog(root).go()
```

## SaveFileDialog
从FileDialog派生，外观跟FileDialog相同，区别在于
- 判断是否存在同名目录。
- 如果指定的文件已存在，会弹出一个小对话框提示是否覆盖已有文件。
- 会判断指定的上一级目录是否存在。
```python
filename = tkinter.SaveFileDialog(root).go()
```

## askopenfilename
filedialog模块提供的一个方法，可以打开一个跟操作系统原生对话框类似的文件选择对话框。它返回选定文件的路径。
```python
filename = tkinter.filedialog.askopenfilename()
```

## asksaveasfilename
与askopenfilename类似，区别是如果指定的文件存在，会提示是否覆盖。
```python
filename = tkinter.filedialog.asksaveasfilename()
```

## askopenfilenames
与askopenfilename类似，但是可以允许选择多个文件，返回一个文件名列表。

## askopenfile
与askopenfilename类似，但是返回的是打开的文件，而不是文件名。
```python
filehandler = tkinter.filedialog.askopenfile()
```

## askopenfiles
与askopenfile类似，但是返回的是打开文件的列表。

## asksaveasfile
与asksaveasfilename类似，但是返回的是打开的指定的文件

## askdirectory
打开一个选择目录的对话框，返回选定的目录。
