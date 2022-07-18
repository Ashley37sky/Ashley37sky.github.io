---
title: C++ 基础入门
date: 2022-07-17 23:08:26
tags: C++
categories: C++
---

# [C++ 基础入门](https://github.com/AnkerLeng/Cpp-0-1-Resource/blob/master/%E7%AC%AC1%E9%98%B6%E6%AE%B5C%2B%2B%20%E5%8C%A0%E5%BF%83%E4%B9%8B%E4%BD%9C%20%E4%BB%8E0%E5%88%B01%E5%85%A5%E9%97%A8/C%2B%2B%E5%9F%BA%E7%A1%80%E5%85%A5%E9%97%A8%E8%AE%B2%E4%B9%89/C%2B%2B%E5%9F%BA%E7%A1%80%E5%85%A5%E9%97%A8.md)

## 1. 初识 

+ 注释

  + 单行注释

    ```cpp
    // 描述信息
    ```

  + 多行注释

    ```cpp
    /* 描述信息 */
    ```

<!--more-->

+ 常量

  ```cpp
  # define day 7 //宏常量
   
  const int month = 12; //const修饰变量
  ```

+ 命名规则

  + 字母或下划线 开头
  + 区分大小写

## 2. 数据类型

+ 统计数据类型所占内存大小 `sizeof()`

+ 整形

  + | **数据类型**        | **占用空间**                                    | 取值范围         |
    | ------------------- | ----------------------------------------------- | ---------------- |
    | short(短整型)       | 2字节                                           | (-2^15 ~ 2^15-1) |
    | int(整型)           | 4字节                                           | (-2^31 ~ 2^31-1) |
    | long(长整形)        | Windows为4字节，Linux为4字节(32位)，8字节(64位) | (-2^31 ~ 2^31-1) |
    | long long(长长整形) | 8字节                                           | (-2^63 ~ 2^63-1) |

+ 浮点型

  + 定义

    ```cpp
    float f1 = 3.14f; // 末尾f
    double d1 = 3.14;
    ```

  + 

  + | **数据类型** | **占用空间** | **有效数字范围** |
    | ------------ | ------------ | ---------------- |
    | float        | 4字节        | 7位有效数字      |
    | double       | 8字节        | 15～16位有效数字 |

+ 字符

  + `char ch = 'a'`
    + 单引号
    + 1 byte
  + 转义 `\n \t \\`

+ 字符串

  + C style 

    ```cpp
    char str1[] = "hello world";
    ```

  + C++ style

    ```cpp
    # include<string>
    string str = "hello world";
    ```

## 3 运算符

+ 逻辑
  + &&: and
  + ||: or

## 4 运行结构

+ 选择

  + ```cpp
    if ()
        ...;
    else if()
        ...;
    else
        ...;
    ```

  + ```cpp
    c = a > b ? a : b; // 三目运算
    ```

  + ```cpp
    switch(a) // int 或者 char
    {
    	case 1：
            执行语句;
            break;
    	case 2：
            执行语句;
            break;
    	default:
            执行语句;
            break;
    }
    ```

## 5 数组

+ 连续的内存

+ 一维数组

  + 定义

    ```cpp
    int score[10];
    int score2[10] = { 100, 90,80,70,60,50,40,30,20,10 };
    int score3[] = { 100,90,80,70,60,50,40,30,20,10 };
    ```

  + 数组名

    + 数组名是常量 -- 地址
    + 元素地址 &arr[0]

    ```cpp
    //1、可以获取整个数组占用内存空间大小
    cout << "整个数组所占内存空间为： " << sizeof(arr) << endl;
    cout << "每个元素所占内存空间为： " << sizeof(arr[0]) << endl;
    cout << "数组的元素个数为： " << sizeof(arr) / sizeof(arr[0]) << endl;
    
    //2、可以通过数组名获取到数组首地址
    cout << "数组首地址为： " << (int)arr << endl;
    cout << "数组中第一个元素地址为： " << (int)&arr[0] << endl;
    cout << "数组中第二个元素地址为： " << (int)&arr[1] << endl;
    ```

+ 二维数组

  + 定义
  
    ```cpp
    int arr[2][3];
    int arr3[2][3] = { 1,2,3,4,5,6 }; 
    int arr4[][3] = { 1,2,3,4,5,6 };
    
    //推荐
    int arr2[2][3] = 
    	{
    		{1,2,3},
    		{4,5,6}
    	};
    ```
    
  + 数组名
  
    ```cpp
    cout << "二维数组大小： " << sizeof(arr) << endl;
    cout << "二维数组一行大小： " << sizeof(arr[0]) << endl;
    cout << "二维数组元素大小： " << sizeof(arr[0][0]) << endl;
    cout << "二维数组行数： " << sizeof(arr) / sizeof(arr[0]) << endl;
    
    cout << "二维数组首地址：" << arr << endl;
    cout << "二维数组第一行地址：" << arr[0] << endl;
    cout << "二维数组第二个元素地址：" << &arr[0][1] << endl; // 元素&
    ```
  
## 6 函数

+ 函数是value传递，不影响实参

+ 声明 & 定义

  + 声明 （many times）

    ```cpp
    int function(int a, int b);
    ```

  + 定义 （once）

    ```cpp
    int function(int a, int b)
    {
        int sum = a + b;
        return sum
    }
    ```

+ 大型项目 - 分文件编写

  + 创建后缀名为.h的头文件

    ```cpp
    //swap.h文件
    #include<iostream>
    using namespace std;
    
    void swap(int a, int b); // 声明
    ```

  + 创建后缀名为function.cpp的源文件

    ```cpp
    //swap.cpp文件
    
    #include "swap.h" // 双引号-自定义文件
    
    void swap(int a, int b)
    {
    	int temp = a;
    	a = b;
    	b = temp;
    
    	cout << "a = " << a << endl;
    	cout << "b = " << b << endl;
    }
    ```

  + main 文件

    ```cpp
    #include "swap.h" // 自定义 头文件
    
    int main() {
    
    	int a = 100;
    	int b = 200;
    	swap(a, b);
    
    	system("pause");
    
    	return 0;
    }
    ```

## ** 7 指针

+ 概念

  + 内存编号是从0开始记录的，一般用十六进制数字表示

  + 指针变量 --> 保存地址 --> 间接访问内存 (解引用 `*`)

    ```cpp
    int a = 10;
    int * p = &a;
    // *p = a
    // p = &a
    ```

  + 所占内存空间  --> 传递给函数的时候，节省空间（不用复制）/ 修改实参

    + 32位系统：4 byte
    + 64位系统：8 byte

+ 空指针和野指针：指向非申请的空间，不能访问

  + 空指针：指向内存中编号为0的空间
    + 用途：初始化指针变量
    + 注意：空指针指向的内存是不可以访问
  + 野指针：指向非法的内存空间  `int * p = (int *)0x1100`

+ `const` 修饰

  + 常量指针 `const int * p1 ` --> 不能修改值（*p）
  + 指针常量 `int * const p1` --> 不能修改指向的地址 (p)
  + 修饰指针 + 常量 `const int * const p` 

+ 指针和数组

  ```cpp
  int arr[] = { 1,2,3,4,5,6,7,8,9,10 };
  
  int * p = arr;  //指向数组的指针
  
  // * p = arr[0]
  cout << "第一个元素： " << arr[0] << endl;
  cout << "指针访问第一个元素： " << *p << endl;
  
  for (int i = 0; i < 10; i++) //利用指针遍历数组
  {
  	cout << *p << endl;
  	p++; // 增加4byte到下一个位置
  }
  ```

  ```cpp
  void bubbleSort(int * arr, int len)  //int * arr 也可以写为int arr[]
  {
  	for (int i = 0; i < len - 1; i++)
  	{
  		for (int j = 0; j < len - 1 - i; j++)
  		{
  			if (arr[j] > arr[j + 1])
  			{
  				int temp = arr[j];
  				arr[j] = arr[j + 1];
  				arr[j + 1] = temp;
  			}
  		}
  	}
  }
  ```

  

+ 指针和函数：指针作为函数参数，会修改实参

  ```cpp
  void swap1(int a ,int b)
  {
  	int temp = a;
  	a = b; 
  	b = temp;
  }
  
  void swap2(int * p1, int *p2)
  {
  	int temp = *p1;
  	*p1 = *p2;
  	*p2 = temp;
  }
  
  swap1(a, b); // 值传递不会改变实参
  
  swap2(&a, &b); //地址传递会改变实参
  ```

## **8 结构体

+ what: 自定义数据类型

+ 定义和使用

  ```cpp
  struct Student
  {
  	//成员列表
  	string name;  //姓名
  	int age;      //年龄
  	int score;    //分数
  };
  
  Student stu1; // 或者 struct Student stu1;
  student stu2 = { "李四",19,60 };
  stu1.name = "张三";
  ```

+ 结构体数组：数组的成员是结构体

  ```cpp
  struct student arr[3]=
  	{
  		{"张三",18,80 },
  		{"李四",19,60 },
  		{"王五",20,70 }
  	};
  ```

+ 结构体指针：通过指针访问结构体中的成员

  ```cpp
  struct student * p = &stu;
  cout << p->name <<endl; 
  cout << (*p).name <<endl;
  ```

+ 结构体的嵌套

  ```cpp
  struct teacher
  {
  	string name;  //教师姓名
  	struct student stu; //子结构体 学生
  };
  ```

+ 结构体做函数参数

  + 值传递 `void printStudent(student stu );`
  + 地址传递 `void printStudent2(student *stu);`

+ `const` 防止误写操作

  + why: 一般使用函数的时候传入结构体指针，而不是复制本来节省空间；所以为了防止在函数中对于实际参数的值的改变，可以用`const`加以限制 (不能修改value)

    ```cpp
    void printStudent(const student *stu) //加const防止函数体中的误操作
    {
    	stu->age = 100; //操作失败，因为加了const修饰
    	cout << "姓名：" << stu->name << " 年龄：" << stu->age << " 分数：" << stu->score << endl;
    
    }
    ```

    
