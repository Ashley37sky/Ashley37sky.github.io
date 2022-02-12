---
title: linux系统/虚拟机网络连接
date: 2020-3-1 19:47:36
tags: [Linux, 虚拟机]
categories: Linux
---

# linux系统/虚拟机网络连接

## 系统组成

1. **linux内核**（linus 团队管理）

2. **shell**：用户与内核交互的接口 

   Shell可以执行：

   - **内部命令**
   - **应用程序**
   - **shell脚本**

3. **文件系统**：ext3、ext4等。windows 有 fat32 、ntfs

4. **第三方应用软件**

## 系统分区

1. 分区类型

   + 主分区：最多四个
   + 扩展分区
     + 最多一个
     + 主分区+扩展分区 <= 4
     + 只能包含逻辑分区，不能写入数据
   + 逻辑分区：
     + 可正常格式化，并写入数据
     + 逻辑分区永远从5开始

   <img src="C:\Users\Ashley\AppData\Roaming\Typora\typora-user-images\image-20210301185714514.png" alt="image-20210301185714514" style="zoom: 50%;" />

   1，2，3，4：主分区       5，6 逻辑分区

   

   <img src="C:\Users\Ashley\AppData\Roaming\Typora\typora-user-images\image-20210301185923398.png" alt="image-20210301185923398" style="zoom:50%;" />

   1，2：主分区   5，6，7，8：逻辑分区

   

2. 格式化的目的：写入系统文件

   + 分成block
   + 建立inode列表

3. 硬件设备文件名：Linux所有硬件都是文件，在`dev`目录下

   |     硬件      | 设备文件名   |
   | :-----------: | ------------ |
   | SCSI/SATA/USB | /dev/sd[a-d] |
   |    打印机     | /dev/lp[0-2] |
   |     鼠标      | /dev/mouse   |

   eg. sda1: 第一块硬盘接口的第一个block

4. windows: 分区-格式化-分配盘符

   Linux：    分区-格式化-建立设备文件名（系统自己处理）-挂载（分配盘符，必须是空目录）

5. 挂载

   + 必须分区：
     + /
     + swap （交换分区，类比虚拟内存）
   + 推荐分区
     + /boot （一般200MB，防止根分区写满影响启动）

   ps. 根分区的子目录可指定独立的硬盘空空间

   <img src="C:\Users\Ashley\AppData\Roaming\Typora\typora-user-images\image-20210301192417748.png" alt="image-20210301192417748" style="zoom: 33%;" />

   

### 补充：硬盘的总线协议与接口

总线（bus）: 不同设备之间交互数据的通路 （eg. CPU 和 硬盘）

总线的带宽：总线单位时间能传输的数据量

协议（protocal）：两个设备具有相同协议才能通信

--> 硬盘和其他电脑原件交互数据：数据协议（沟通）+传输总线（媒介）+物理接口（接入)  (三者必须匹配)

+ 目前硬盘数据协议：
  + IDE(淘汰)
  + AHCI
  + NVMe
  + SCSI（服务器）

+ 目前数据传输总线：
  + SATA
  + PCIe
  + SAS(服务器)

![image-20210301191309437](C:\Users\Ashley\AppData\Roaming\Typora\typora-user-images\image-20210301191309437.png)

## Linux基本目录结构

![img](https://pic2.zhimg.com/v2-1f6cdbc3e0765ae8484624eaa2a08ab9_b.jpg)

+ **/bin 存放系统命令 -二进制可执行文件(ls,cat,mkdir等)**   普通用户和超级用户都能执行
+ /sbin：存放系统命令 （super：只有root能执行）
+ /usr/bin: 在单用户模式下不能执行
+ /boot: 系统启动目录表
+ /dev 用于存放设备文件
+ **/etc 存放系统配置文件**
 + /home 普通用户的家目录
 + /lib 系统调用的函数库
 + /lost+found: 系统意外崩溃或者关机时产生的碎片文件
+ /mnt 挂在额外设备（USB...）
+ /proc 保存在内存中（不要往里面写东西）
+ /sys   保存在内存中（不要往里面写东西）
+ **/root 超级用户的家目录**
+ /srv服务数据目录
+ /tmp 临时目录
+ /usr （Unix software resource）用于存放系统应用程序，比较重要的目录``/usr/local`  **本地管理员软件安装目录****
+ /var 存放动态数据（日志）

## linux 注意点

1. Linux所有内容以文件形式保存，所以命令行更改时临时生效

2. 严格区分大小写

3. 不靠扩展名区分文件类型（写扩展名是为了方便使用）

   | 压缩包       | .gz   .bz2     .tar    .bz2    . tgz |
   | ------------ | ------------------------------------ |
   | 二进制软件包 | .rpm                                 |
   | 网页文件     | .html   .php                         |
   | 脚本文件     | .sh                                  |
   | 配置文件     | .config                              |

4. linux 所有的存储设备都必须挂载后才能使用  （eg. USB， 光盘， 硬盘）

# 虚拟机

## 网络设置

+ 桥接： 利用真实网卡，可以和局域网内设备交流
+ 其他：只能和本机通信，但是不占用ip地址
  + NAT: 如果主机能访问互联网，虚拟机就能够访问
  + Host-only：只能和主机通信 （无网）

ps. 

1. ifconfig eth0 `ip地址` -> 配置ip地址   （if：interface网卡）

2. ping `ip地址` -> 测试是否能通信成功
3. 重启后ip地址丢失，要永久保留需要写入config文件  