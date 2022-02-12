---
title: Arduino_代码
date: 2020-06-27 21:46:16
tags: Arduino
categories: Arduino
---

# 编程基础

## 数字I/O

### 数字信号

<!--more-->

```c
// 在大多数Arduino控制板上 13号引脚都连接了一个标有“L”的LED灯
int led = 13;

void setup(){
  pinMode(led, OUTPUT);     
}

void loop() 
{
  digitalWrite(led, HIGH);   // 点亮LED
  delay(1000);           // 等待一秒钟
  digitalWrite(led, LOW);   // 通过将引脚电平拉低，关闭LED
  delay(1000);           // 等待一秒钟
}
```

### 流水灯实验

![img](https://www.arduino.cn/data/attachment/forum/201801/20/211432ahrihu0suh111hrh.png)

```c
void setup() 
{
  for(int i=2;i<8;i++)
    pinMode(i,OUTPUT);
}
 
void loop() 
{
  // 从引脚2到引脚6，逐个点亮LED，等待1秒再熄灭LED
  for(int i=2;i<7;i++)
  {
    digitalWrite(i,HIGH);
    delay(1000);
    digitalWrite(i,LOW);   
  }
  // 从引脚7到引脚3，逐个点亮LED，等待1秒再熄灭LED
  for(int i=7;i>2;i--)
  {
    digitalWrite(i,HIGH);
    delay(1000);
    digitalWrite(i,LOW);   
  } 
}
```

### 按键控制LED实验

![](https://www.arduino.cn/data/attachment/forum/201801/20/212234wfsxa702afgaamad.png)

**限流电阻**
一般LED的最大能承受的电流为25mA，如若直接将LED连接到电路中，当其点亮时，如果电流过大，很容易烧毁。如图2-24所示，我们在LED一端串联了一个电阻R2，这样做可以控制流过LED的电流，防止损坏LED。这个电阻我们称之为限流电阻。

**下拉电阻**
在2号引脚到GND之前，连接了一个阻值10K的电阻。如果没有该电阻，当未按下按键时，2号引脚会一直处于悬空 状态，此时使用digital Read() 读取2号引脚状态，会得到一个不稳定的值（可能是高，也可能是低）。添加这个R1电阻到地就是为了稳定引脚的电平，当引脚悬空时，就会识别为低电平。而这种将某节点通过电阻接地的做法，叫做下拉，这个电阻叫做下拉电阻。

```c
const int buttonPin = 2;     // 连接按键的引脚
const int ledPin =  13;      // 连接LED的引脚
 
int buttonState = 0;         // 存储按键状态的变量
 
void setup() {
  pinMode(ledPin, OUTPUT);      
  pinMode(buttonPin, INPUT);     
}
 
void loop(){
  // 读取按键状态并存储在变量中
  buttonState = digitalRead(buttonPin);
 
  // 检查按键是否被按下
  // 如果按键按下，那buttonState应该为高电平
  if (buttonState == HIGH) {     
    // 点亮LED
    digitalWrite(ledPin, HIGH);  
  } 
  else {
    // 熄灭LED
    digitalWrite(ledPin, LOW); 
  }
}
```

#### 进阶（上拉电阻）

![img](https://www.arduino.cn/data/attachment/forum/201801/20/212701dvu4tmkujk3lvsdx.png)

```c
int buttonPin = 2;
int ledPin = 13;
int buttonState = 0; 
 
void setup() 
{
  pinMode(buttonPin,INPUT_PULLUP);
  pinMode(ledPin,OUTPUT);
}
 
void loop() 
{
  buttonState = digitalRead(buttonPin);
  // 按住按键时，点亮LED；放开按键后，熄灭LED。
  if(buttonState==HIGH)
  {
    digitalWrite(ledPin,LOW);   //上拉电阻
  }
  else
  {
    digitalWrite(ledPin,HIGH);
  }
}
```

#### 进阶（按一下亮，按一下熄灭）

```c
int buttonPin = 2;
int ledPin = 13;
boolean ledState=false;  
boolean buttonState=true;  
 
void setup() 
{
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(ledPin,OUTPUT);
}
 
void loop() 
{
// 等待按键按下
while(digitalRead(buttonPin)==HIGH){}
 
  // 当按键按下时，点亮或熄灭LED
  if(ledState==true)
  {
    digitalWrite(ledPin,LOW);
    ledState=!ledState;
  }
  else
  {
    digitalWrite(ledPin,HIGH);
    ledState=!ledState;
  }
  delay(500);  // 很重要
}
```

程序末尾有一个`delay(500)` 的延时，它在这里是极其**重要**的。删去这个延时操作，按键出现失灵。这是因为程序运行的非常快，没有了延时操作，你按下按键到放开按键的间隔时间虽然极短，但loop中的语句可能已经运行了很多次，很难确定你放开按键时正在运行的loop() 循环是点亮还是熄灭LED。我们使用延时操作来使两次按键间产生一定的间隔时间，在间隔时间内Arduino会忽略按键按下情况，从而达到**区分两次按键**的目的。

## 模拟I/O的使用

### 呼吸灯实验

![img](https://www.arduino.cn/data/attachment/forum/201803/05/210943ipvv9qj2tvdxxqpk.png)

```c
nt led = 9;           // LED灯连接在9号引脚
int brightness = 0;     // LED灯亮度
int fadeAmount = 5;   // 亮度渐变值
 
void setup() {
  pinMode(led, OUTPUT);
}
 
void loop() {
  analogWrite(led, brightness);
  brightness = brightness + fadeAmount;
  if (brightness == 0 || brightness == 255) {
    fadeAmount = -fadeAmount ;
  }
  delay(30);
}
```

delay(30) 的延时语句，这是为了让我们肉眼能观察到亮度调节的效果。如果没有这个语句，整个变化效果将一闪而过。

#### 进阶（控制呼吸灯的呼吸频率）

![img](https://www.arduino.cn/data/attachment/forum/201803/05/210944d0b0izhh002bbnqq.jpg)

```c
int ledPin = 9;  // 9号引脚控制LED
int pot=A0;    // A0引脚读取电位器输出电压
void setup(){} 
 
void loop(){ 
  // LED逐渐变亮
  for(int fadeValue = 0 ; fadeValue <= 255; fadeValue +=5) 
  { 
    analogWrite(ledPin, fadeValue);
// 读取电位器输出电压，除以5时为了缩短延时时间
int time=analogRead(pot)/5;
    delay(time);  // 将time用于延时
  } 
  // LED逐渐变暗
  for(int fadeValue = 255 ; fadeValue >= 0; fadeValue -=5) 
  { 
	analogWrite(ledPin, fadeValue);
    delay(analogRead(pot)/5);  // 读取电位器输出电压，并用于延时
  } 
}
```

### 光敏电阻检测环境光实验



![img](https://www.arduino.cn/data/attachment/forum/201803/05/212143wldvvylf33xqv33b.jpg)

```
void setup()
{
  // 初始化串口
  Serial.begin(9600);
}
void loop() 
{
// 读出当前光线强度，并输出到串口显示
  int sensorValue = analogRead(A0);
  Serial.println(sensorValue);
  delay(1000);
}
```

![img](https://www.arduino.cn/data/attachment/forum/201803/05/212420xai1n4aicakauqii.jpg)

由于电源波动或外界干扰等原因，输出的数据可能也会受到一定的影响，例如波动较大等现象，这时你可以通过读取多次传感器数值，求平均数的方法，减小数据的波动。

## 串口的使用

#### 串口输出

```
int counter=0; // 计数器
 
void setup() {
// 初始化串口
  Serial.begin(9600);
}
 
void loop() {
// 每loop循环一次，计数器变量加1
counter = counter+1;
// 输出变量
Serial.print(counter);
// 输出字符
Serial.print( ':' );
// 输出字符串;
Serial.println("Hellow World");
delay(1000);
}
```

#### 串口输入

```
void setup() {
  // 初始化串口
  Serial.begin(9600);
}
 
void loop() {
// 如果缓冲区中有数据，则读取并输出
if(Serial.available()>0)
  {
    char ch=Serial.read();
    Serial.print(ch);
  }
}
```

​    