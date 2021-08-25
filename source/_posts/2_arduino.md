---
title: Arduino基础
date: 2020-06-27 20:41:20
tags: Arduino
categories: Arduino
---

# 编程入门

## C\C++语言基础

<!--more-->

+ 浮点型数据的运算，速度较慢且可能有精度丢失。通常我们会把浮点型转换为整型来处理相关运算。如9.8cm，我们通常把换算为98mm来计算
+ 存储字符时，字符需要用单引号引用，如`char col=’C’;` 
+ 相较于数组形式的定义方法，使用String类型定义字符串会占用更多的存储空间
+ Switch后的表达式结果只能是整形或字符型。如果要使用其他类型，则必须使用if语句

## 常用电子元件

#### 面包板

![面包内部、.jpg](https://www.arduino.cn/data/attachment/forum/201704/15/202407hn5028ql688082nn.jpg)

#### 电阻

![1.jpg](https://www.arduino.cn/data/attachment/forum/201704/15/202658ek379kisa3pi3vzh.jpg)



下拉电阻：将某节点通过电阻接地的做法，叫做下拉，这个电阻叫做下拉电阻

上拉电阻：同下拉电阻一样，可以稳定I/O口电平，不同的是电阻连接到VCC，将引脚稳定在高电位。

​					可以用`pinMode(buttonPin,INPUT_PULLUP)`或者物理连接实现

#### 电容

![2.jpg](https://www.arduino.cn/data/attachment/forum/201704/15/202736q9kswuxis5nl825i.jpg)

电容也有很多作用，如去耦、滤波、储能等

#### 二极管

![3.jpg](https://www.arduino.cn/data/attachment/forum/201704/15/202812i9v9k8tly9hwhvxl.jpg)

二极管在电路中使用广泛，作用众多，如整流、稳压、电路保护等

#### LED（发光二极管

![4.jpg](https://www.arduino.cn/data/attachment/forum/201704/15/202849hnqjq8mqtntmj8zm.jpg)

一般LED的最大能承受的电流为25mA

#### 三极管

![5.jpg](https://www.arduino.cn/data/attachment/forum/201704/15/202929l3to1oz4wtwf6chn.jpg)

三极管，能起放大、开关等作用的元件。
有发射极（emitter,E）、基极（base,B）和集电极（collector,C）三级，
有pnp和npn 两种类型的三极管。

#### 按键

![img](https://www.arduino.cn/data/attachment/forum/201801/20/212948cbd22kta69immltw.jpg)

#### 电位器

![img](https://www.arduino.cn/data/attachment/forum/201803/05/210944q3dre8qok1daddvt.png)

电位器是一个可调电阻

#### 光敏电阻

#### <img src="https://www.arduino.cn/data/attachment/forum/201803/05/212142at6fw8a8rkx8n6ka.png" alt="img" style="zoom:67%;" />

电阻值随照射光强度增加而下降的电阻



# 编程基础

![img](https://www.arduino.cn/data/attachment/forum/201801/20/210906pl8cbzbqyeiq8ill.jpg)

## 数字I/O的使用

1. 新建变量

   ```
   #define LED 13
   int led = 13;
   ```

2. 配置引脚模式

   ```c
   pinMode(pin, mode); 
   ```

   > 可使用的三种模式:
   >
   > + **INPUT**     输入模式
   > + **OUTPUT**     输出模式
   > + **INPUT_PULLUP**     输入上拉模式

3. 输出高低电平

   ```
   digitalWrite(pin, value); //value= HIGH/LOW 
   ```

4. 读取外部输入的数字信号

   ```
   int value = digitalRead(pin) 
   ```

   > Arduino UNO会将大于3V的输入电压视为高电平，小于1.5V的电压视为低电平。超过5V的输入电压可能会损坏Arduino UN

*在Arduino核心库中，OUTPUT被定义等于1，INPUT被定义等于0，HIGH被定义等于1，LOW被定义等于0，所以可以写成`pinMode(13,1);` ` digitalWrite(13,1)`。但是不建议，因为可读性变差*

5. 延时函数。在写程序的时候要注意写delay()

   ```
   delay() ; //ms
   ```

## 模拟I/O的使用

1. 编号带“A”的引脚是模拟输入引脚。读取引脚上输入的电压大小。Arduino UNO上，可以接受0～5V的模拟信号。Arduino 模拟输入功能有10位精度，即可以将0～5V的电压信号转换为0～1023的整数形式表示

   ```
   int value = analogRead(pin)
   ```

2. 模拟输出功能。以脉冲宽度调制（PWM）来达到输出近似模拟值的效果。这里仅仅是得到了近似模拟值输出的效果，如果要输出真正的模拟值，还需要加上外围滤波电路

   ```
   analogWrite(pin,value)
   ```

   value指定是PWM的脉冲宽度，范围为0～255。

## 串口的使用

Arduino与计算机通信最常用的方式就是串口通信。串口通信时，Arduino控制器上的标有RX/TX的两LED灯会闪烁提示，接收数据时，RX灯会点亮；发送数据时，TX灯会点亮。

1. 使用串口与计算机通信，需要先使用Serial. Begin() 初始化Arduino的串口通信功能

   ```
   Serial.begin(speed);
   ```

   peed是指串口通信波特率,**波特率**是衡量通信速度的参数，表示每秒钟传送的bit的个数。串口通信的双方必须使用同样的波特率，方能正常进行通信。

2. 串口输出。使用Serial. Print() 或Serial.println() 向计算机发送各种类型的信息

   ```c
   Serial.print(val);
   Serial.println(val);  //回车换行
   ```

3. 串口监视器是Arduino IDE自带的一个小工具，可以查看到串口传来的信息，也可以向连接的设备发送信息

4. 缓冲区。在使用串口时，Arduino Uno会在SRAM中开辟一段大小为256 bytes的空间，串口接收到的数据都会被暂时存放进这个空间中，这个存储空间，我们称之为缓冲区。当你调用Serial. Read()语句时，Arduino便会从缓冲区取出一个字节的数据

   ```c
   Serial.available(); //缓冲区中接收到的数据字节数
   ```

5. 串口输入

   ```c
   erial.read(); //每次都会返回一个字节的数据
   ```

   当缓冲区中没有可读数据时，read()函数会返回int型值-1，而int型-1对应的char型数据是乱码。所以要先判断缓冲区是否有数据。

   ```
   if( Serial.available() > 0 ) 
   while( Serial.available() > 0 )
   ```

6. 特殊用法。等待串口监视器开启，开启串口监视器后，!Serial将为假

   ```
   while (!Serial) { }
   ```

   

