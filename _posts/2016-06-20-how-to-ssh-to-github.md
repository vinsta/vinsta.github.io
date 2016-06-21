---
layout: post
title:  "如何使用SSH连接Github"
categories: jekyll
tags:   SSH Github tool
---

* content
{:toc}
简单介绍如果使用SSH方式连接Github，来clone仓库或者下载、上传修改。


## 检查SSH Key是否存在
使用`ls -al ~/.ssh`命令查看SSH key，通常key成对使用，其中公钥的名字为`id_rsa.pub`， 私钥的名字为`id_rsa`。
<pre><code class="markdown">
$ ls -al ~/.ssh
total 20
drwx------ 2 jing jing 4096  6月20 11:14 ./
drwxr-xr-x 5 jing jing 4096  6月20 14:38 ../
-rw------- 1 jing jing 3326  6月20 11:14 id_rsa
-rw-r--r-- 1 jing jing  740  6月20 11:14 id_rsa.pub
-rw-r--r-- 1 jing jing  884  6月20 11:08 known_hosts
</code></pre>

如果key pair不存在，继续第2步，否则可以跳过第2步，直接看第3步。

## 生成SSH Key
使用下面的命令来生成key pair，使用邮件地址做标签用于识别。命令执行过程中会要求输入保存key的路径和文件名，一般默认即可。
<pre><code class="markdown">
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
</code></pre>

下面提示是否需要设置密码。如果设置密码，每次跟Github交互都需要输入密码。
如果不需要设置密码，直接留空回车即可。
<pre><code class="markdown">
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
</code></pre>

## 将SSH公钥添加到Github账户
- 拷贝公钥id\_isa.pub的内容。
- 进入Github账户设置页面`Setting->SSH and GPG keys->New SSH key`。
- 粘贴公钥内容。
- 点击`Add SSH key`按钮添加。

## 大功告成，现在可以使用git命令来操作仓库了。

