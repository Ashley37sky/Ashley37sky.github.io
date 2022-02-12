---
title: Linux命令
date: 2021-03-01 19:49:06
tags: Linux命令
categories: Linux
---

# Linux命令

## 命令格式 

**cmd [-options] [arguments] **

+ Linux**区分大小**
+ 个别命令不遵循此格式
+ 有多个选项时，可以写在一起   eg ls -la /etc

<!--more-->

+ **简化选项**前使用**一个**`减号-`。**完整选项**前使用两个`减号--`   eg. -a == --all
+ **通配符**
  - *：匹配任何字符和任何数目的字符
  - ?：匹配单一数目的任何字符
  - [ ]：匹配[ ]之内的任意一个字符
  - [! ]：匹配除了[! ]之外的任意一个字符，!表示非的意思

## 常见

+ `pwd`（print working directory）：查看用户的当前目录
+ `cd` ： 切换目录
+ `.`表示当前目录
+ `..` 表示当前目录的上一级目录（父目录）
+ `-`表示用 cd 命令切换目录**前**所在的目录
+ `~` 表示**用户主目录**的绝对路径名
+ `clear`:清屏

<!--more-->

+ **绝对路径：**以斜线（/）开头 
+ **相对路径 ：**不以斜线（/）开头

## 常用快捷键

+ `ctrl+l` clear screen
+ `ctrl+r`  查看历史命令
+ `ctrl+q` quit

## 文件/目录处理命令

### ls

+ -a (all)： 显示隐藏文件

+ -d (directory) 显示目录本身

+ -h (human) 显示单位

+ -i (id)：i节点，理解为身份证

+ -l (long): 显示属性

  ![image-20210301200427123](C:\Users\Ashley\AppData\Roaming\Typora\typora-user-images\image-20210301200427123.png)

  1. u-user 所有者（一个用户）
  2. g-group 所属组（一个组）
  3. o-other 其他人

  eg.

  | -    | -二进制文件       d目录         l软连接文件 |
  | ---- | ------------------------------------------- |
  | rw-  | u                                           |
  | r--  | g                                           |
  | r--  | o                                           |

  + r：读  
  + w：写
  + x：执行

### 目录处理命令

+ `mkdir`: 创建新目录
  + -p 递归创建
  + 可同时创建多个目录，空格隔开

- `rmdir`：删除空目录 （很很少用）
- `cp [origin] [new]`：复制文件
  - -r：复制目录
  - -p：保留文件属性 （eg.修改时间）
  - 复制的时候可以改名
    - cp -r /tmp/test /root/test1
    - cp -r /rmp/test /root (不改名) 
- `mv [origin] [new]`：移动文件或目录、改名(同一目录下移动并改成新名字)
- `rm`：删除文件
  - -r 删除目录
  - -f 强制删除（不会询问是否删除）
  - 常用 rm -rf

### 文件处理命令

+ `touch [文件名]`： 创建空文件、
+ `cat`: 显示文件内容
  + -n: 显示行号
+ `tac`：反向显示文件内容
+ `more`:分页显示文件内容
  + 空格、f  --> 翻页
  + enter  -->换行
  + q、Q  -->退出
+ `less`: more升级版
  + pgup：向上翻页
  + /[keyword]: 搜索
    + n：下一个关键词所在位置
+ `head`：显示文件前几行
  + -n 指定行数，默认10
+ `tail`：显示文件后几行
  + -n 
  + -f：动态显示，实时更新（日志）
+ `find`：查找文件

### 链接命令

- `ln [原文件] [目标文件]`：建立链接文件

  - -s：创建软连接

  |       软链接（同win快捷方式，常用）        | 硬链接（很少用）              |
  | :----------------------------------------: | ----------------------------- |
  | 显示lrwxrwxrwx，实际文件操作权限才有决定权 | 除文件名都一样                |
  |       很小，只是符号链接，有箭头指向       | i节点同源文件，两文件同时更新 |
  |                 可以是目录                 | 不能是目录                    |
  |         删除原文件后不能访问软链接         | 删除原文件后能访问硬链接      |
  |                 可以跨分区                 | 不能跨分区 (/boot  /home   /) |

### 其他

- `echo`：把内容重定向到指定的文件中 ，有则打开，无则创建
- `管道命令 |` ：将前面的结果给后面的命令，例如：`ls -la | wc`，将ls的结果加给wc命令来统计字数

+ `重定向 > 是覆盖模式，>> 是追加模式`，例如：`echo "Java3y,zhen de hen xihuan ni" > qingshu.txt`把左边的输出放到右边的文件里去

## 权限管理命令

### 更改权限-chmod 

chmod：（change mode）root、所有者可以执行

两种方式

+ `chmod [{ugoa} {+-=} {rwx} [文件或目录]]`

+ `chmod [421] [文件或目录]` (更常用)

  ps. r---4, w--2, x--1

  ​	  rwxrw-r--(764)

  + `-R` 递归修改 （把目录及其子目录全西安都修改）
  + 一次更改多个权限，用逗号隔开 `g+w,o-r`

 补充： 创建普通用户，组

+ `useradd __`          `passwd  __`
+ `groupadd`

**重要：file和directory的rwx权限**

|      | file                    | directory                                       |
| ---- | ----------------------- | ----------------------------------------------- |
| r    | cat/more/head/tail/less | ls （列出目录内容）                             |
| w    | vim （可修改文件）      | touch/mkdir/rmdir/rm （可在目录中创建删除文件） |
| x    | script command          | cd （可以进入目录，所以一般rx同时出现）         |

+ 文件 w权限 = 修改文件
+ 目录 w权限 = 删除创建文件
+ 对文件有权限，但是进不去文件所在目录也不行

### 其他权限命令

+ `chown [用户] [文件或目录]`: change ownership 改变所有者，只有root能执行

+ `umask`  显示，设置文件的缺省（默认）权限

  + `-S` : 以rwx显示新建文件的default权限
  + 不加S

  <img src="C:\Users\Ashley\AppData\Roaming\Typora\typora-user-images\image-20210311143914415.png" alt="image-20210311143914415" style="zoom: 25%;" />

## 文件搜索命令

### find

`find [搜索范围] [匹配条件]`

+ `find [搜索范围] -name [条件]`

  + `-iname`
  + `*` 匹配多个  `?` 匹配一个字符

+ `find [搜索范围] -size +204800` 查找大于100MB的文件

  + `+ - =`

  + size 是数据块大小  1个数据块-512字节-0.5k

    （100M = 102400K =204800数据块）

+ `find [搜索范围] -user [用户名]`  查找以用户名为所属者的文件

  + `-group`    根据所属组查找

+ `find [搜索范围] -cmin -5`  查找5min内被修改过属性的文件和目录

  + `amin`   access 访问时间
  + `cmin` change 文件属性
  + `mmin` modify 文件内容

+ `-type` 根据文件类型查找

  + f：文件
  + d：目录
  + l：软连接文件

+ 多个条件

  + `-a` and

    eg. `find /etc -size +163940 -a size -204800`

  + `-o` or

+ `-exec/-ok [命令] {} \` 对搜索结果执行操作

  + eg. `find /etc -name init -exec ls -l {} \`
  + `-ok` 会询问是否确定操作

### 其他搜索命令

+ `locat [文件]`再文件资料库中查找文件
  + `-i` 不区分大小写
  + `updatedb` 更新资料库
  + 查找比find快，但是新建文件后 如果资料库未更新 则搜不到文件
  + /tmp 的文件搜不到
+ `which [命令]` 搜索 命令 所在 目录 和 别名
+ `whereis [命令]` 搜索 命令 所在 目录 和 帮助文档路径
+ `grep -iv [指定字串] [文件]` 在文件中搜寻字串匹配的行并输出
  + `-i` 不区分大小写
  + `-v` 排除指定字符串
  + `grep -v ^# [file]` 去掉以#开头的行（注释行）

## 压缩/解压

- `gzip filename`
- `bzip2 filename`
- `tar -czvf filename`

- `gzip -d filename.gz`
- `bzip2 -d filename.bz2`
- `tar -xzvf filename.tar.gz`