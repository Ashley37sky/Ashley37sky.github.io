---
title: linux
date: 2020-11-15 17:01:36
tags: linux
categories: linux
---

# **Linux命令**

## 常见

+ `pwd`（print working directory）：查看用户的当前目录
+ `cd` ： 切换目录
+ `.`表示当前目录
+ `..` 表示当前目录的上一级目录（父目录）
+ `-`表示用 cd 命令切换目录**前**所在的目录
+ `~` 表示**用户主目录**的绝对路径名

<!--more-->

+ **绝对路径：**以斜线（/）开头 
+ **相对路径 ：**不以斜线（/）开头

## 常用快捷键

+ `ctrl+l` clear screen
+ `ctrl+r`  查看历史命令
+ `ctrl+q` quit

## 文件

- `rmdir`：要求目录为空

- `touch`：生成一个空文件或更改文件的时间
- `cp`：复制文件或目录
- `mv`：移动文件或目录、文件或目录改名
- `rm`：删除文件或目录

- `ln`：建立链接文件
- `find`：查找文件
- `file/stat`：查看文件类型或文件属性信息
- `cat：`查看文本文件内容
- `more：`可以分页看
- `less：`不仅可以分页，还可以方便地搜索，回翻等操作
- `tail -10`： 查看文件的尾部的10行

- `head -20`：查看文件的头部20行
- `echo`：把内容重定向到指定的文件中 ，有则打开，无则创建
- `管道命令 |` ：将前面的结果给后面的命令，例如：`ls -la | wc`，将ls的结果加油wc命令来统计字数

+ `重定向 > 是覆盖模式，>> 是追加模式`，例如：`echo "Java3y,zhen de hen xihuan ni" > qingshu.txt`把左边的输出放到右边的文件里去

## 压缩/解压

- `gzip filename`
- `bzip2 filename`
- `tar -czvf filename`



- `gzip -d filename.gz`
- `bzip2 -d filename.bz2`
- `tar -xzvf filename.tar.gz`

# linux基础

## 1.系统组成

1. **linux内核**（linus 团队管理）

2. **shell**：用户与内核交互的接口 

   Shell可以执行：

   - **内部命令**
   - **应用程序**
   - **shell脚本**

3. **文件系统**：ext3、ext4等。windows 有 fat32 、ntfs

4. **第三方应用软件**

## 2.Linux基本目录结构

![img](https://pic2.zhimg.com/v2-1f6cdbc3e0765ae8484624eaa2a08ab9_b.jpg)

+ **bin 存放二进制可执行文件(ls,cat,mkdir等)**+
+ boot 存放用于系统引导时使用的各种文件+ 
+ dev 用于存放设备文件
+ **etc 存放系统配置文件**
 + home 存放所有用户文件的根目录
 + lib 存放跟文件系统中的程序运行所需要的共享库及内核模块
+ mnt 系统管理员安装临时文件系统的安装点
+ **opt 额外安装的可选应用程序包所放置的位置**
+ proc 虚拟文件系统，存放当前内存的映射
+ **root 超级用户目录**
+ sbin 存放二进制可执行文件，只有root才能访问
+ tmp 用于存放各种临时文件
+ usr 用于存放系统应用程序，比较重要的目录/usr/local 本地管理员软件安装目录
+ var 用于存放运行时需要改变数据的文件

## 命令 cmd [options] [arguments]

+ Linux是**区分大小**写的
+ **单字符**选项前使用**一个**`减号-`。**单词选项**前使用两个`减号--`
+ **通配符**
  - *：匹配任何字符和任何数目的字符
  - ?：匹配单一数目的任何字符
  - [ ]：匹配[ ]之内的任意一个字符
  - [! ]：匹配除了[! ]之外的任意一个字符，!表示非的意思