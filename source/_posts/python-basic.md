---
title: python基础
date: 2020-11-19 20:52:15
tags: python
categories: python
---

# python基础

## from kaggle

+ help()   : pass the name, not the result

  ```python
  help(round)
  # help(round(3.0))
  ```

+ docstring：provide a description of a function

  <!--more-->

  + triple-quoted string
  +  after the header of a function
  + help() --> shows the docstring

  ```python
  def least_difference(a, b, c):
      """Return the smallest difference between any two numbers
      among a, b and c.
      
      >>> least_difference(1, 5, -5)
      4
      """
      diff1 = abs(a - b)
      diff2 = abs(b - c)
      diff3 = abs(a - c)
      return min(diff1, diff2, diff3)
  ```

+ and 优先级 > or

+ objects carry something, and the stuff is accessed using Python's dot syntax

  + carry: funciotn --> method
  + carry: non-function --> attributes

### list

+ lists (mutable)

  + indexing `list[0]`

  + slicing `list[0:3] `   `list[-1:1]` 

  + change `list[1]="second"`

  + list comprehensions

    ```python
    squares = [n**2 for n in range(10) if n%2==0 ]
    
    squares = [
        		n**2 
        		for n in range(10) 
        		if n%2==0 
    			]
    ```

    

  + functions

    + len(list)
    + sorted(list)
    + sum(list)
    + max(list)   \   min(list)

  + method   --> help(list)  --> learn all the method

    + list.append()
    + list.pop()  : removes and returns the last element
    + searching
      + "element" in list
      + list.index('element')

### tuple

+ tuples(immutable)
  + create
    + t=(1, 2, 3)
    + t=1, 2, 3
  + a, b = b, a

### loop

+ loops
  + for planet in planets
  + for i in range(5)
  + while i<10

### string

+ Double quotes -->  contains single quote charater

+ single quotes --> contains double quote character

+ escaping --> `\`

+ string --> sequnces of characters --> list (immutable)

  + index

  + slice

  + funcion

  + method for string

    + .split()
    + .join()   `'/'.join([month, day, year])`
    + str.format()

    ```python
    "{}, you'll always be the {}th planet to me.".format(planet, position)
    
    
    s = """Pluto's a {0}.
    No, it's a {1}.
    {0}!
    {1}!""".format('planet', 'dwarf planet')
    
    '{:10.5}'.format('xyz')    #十个占位，5位有效数字
    ```

### dictionaries

+ create

  ```python
  numbers = {'one':1, 'two':2, 'three':3}
  ```

  

+ index  `index["one"]`

+ add `numbers["four"]=4`]

+ change

+ dictionary comprehensions

+ 'one' in numbers

+ 遍历

  ```python
  for k in numbers:
      print("{} = {}".format(k,numbers[k]))
      
  for k,num in numbers.items():
      print("{} = {}".format(k,num) )
      # dict.items() = dict.keys() + dict.values()
  ```

  




### external libraries

+ understanding strange objects

  + `type()`   -->  what is it 

  + `dir()`  --> what can be done  (模块中定义的模块，哈桑函数，变量)
  + `help()`  --> more info 

+ **operator overloading**

  

### 基础

+ cmd `python` -> 进入到交互环境；`quit`
+ 注释 `#` `'''` `"""`

+ `end=''`表示输出后不换行 （默认的结束符`\n`）

+ 大小写敏感

+ | 函数                | 说明                                                         |
  | ------------------- | ------------------------------------------------------------ |
  | `chr`               | 整数->字符串                                                 |
  | `float`             | 个字符串转换成浮点数                                         |
  | `type`              | 函数变量的类型进行检查                                       |
  | `bin`               | 整数->`'0b'`开头的二进制字符串                               |
  | `ord`               | 字符串->整数                                                 |
  | `hex`               | 整数->'0x'`开头的十六进制字符串，例如：`hex(123)`会返回`'0x7b'` |
  | `input`             | 从输入中读取一行，返回读到的字符串。                         |
  | `open`              | 打开一个文件并返回文件对象                                   |
  | `round`             | 四舍五入                                                     |
  | `range(100, 0, -2)` | 100到1的偶数                                                 |
  | `/`  `//` `**`      | 除法 整除  指数                                              |
  | `print()`           | 来实现换行输出                                               |

+ | `is` `is not`    | 身份运算符 |
  | ---------------- | ---------- |
  | `in` `not in`    | 成员运算符 |
  | `not` `or` `and` | 逻辑运算符 |

+ 
  ```python
  f = float(input('请输入华氏温度: '))
  ```

+ 
  ```python
  print('%.1f华氏度 = %.1f摄氏度' % (f, c))
  print(f'{f:.1f}华氏度 = {c:.1f}摄氏度')
  print(f'{i}*{j}={i * j}', end='\t')
  print()   #来实现换行输出
  ```

  
  
+ 
  ```python
  if ... :
  elif:
  else:
  ```

+ **Flat is better than nested**

+ **for-in循环**

  ```python
  for x in range(1, 101):   #[1,101)
  ```

+ 产生一个1-100范围的随机数

  ```python
  import random
  answer = random.randint(1, 100)
  ```

## **函数**

  ```python
  def fac(num):
  	....
  	return result
  ```

  + 函数也可以没有自变量，但是函数名后面的圆括号是必须有的

  + `def roll_dice(n=2):` 默认值为2,如果没有指定参数,那么n使用默认值2;传入参数3，变量n被赋值为3

  + 带默认值的参数必须放在不带默认值的参数之后

  + **可变参数**: 可变参数其实就是将多个参数打包成了一个元组

    ```python
    def add(*args):
        total = 0
        # 可变参数可以放在for循环中取出每个参数的值
        for val in args:
            total += val
        return total
    ```

  + 用模块管理函数->解决命名冲突

    ```python
    import module1 as m1
    import module2 as m2
    
    from module1 import foo
    from module1 import foo #导入同名函数，后导入覆盖先导入
    
    from module1 import foo as f1
    from module2 import foo as f2
    ```

    

## 数据结构

### 字符串(string '')

+ **不可变类型**->**不能通过索引运算修改字符串中的字符**

+ `\`（反斜杠）来表示转义

  + \t    制表符
  + 本身包含了`'  \`这些特殊的字符，通过`\`进行转义处理

+ 原始字符串：以`r`或`R`开头 `s2 = r'\time up \now'`

+ `+`和`*` -> 拼接和重复

+ 
  ```python
  # 比较字符串的内容
  print(s1 == s2, s2 == s3)    # True True
  # 比较字符串的内存地址
  print(s1 is s2, s2 is s3)    # False True
  ```

+ 成员运算

  ```python
  print('wo' in s1)    # True
  print('wo' not in s1)
  ```

+ 索引和切片

  + 正向索引 : 0`到`N-1
  + 负向索引: -1`到`-N
  + 切片: [i:j:k]

+ 循环遍历

  + 下标

    ```python
    for index in range(len(s1)):
        print(s1[index])
    ```

  + 直接

    ```python
    for ch in s1:
        print(ch)
    ```

+ 查找

  + find

    ```python
    # 找到了返回字符串中另一个字符串首字符的索引
    print(s.find('or'))        # 8
    # 找不到返回-1
    print(s.find('shit'))      # -1
    ```

  + index

    ```python
    # 找到了返回字符串中另一个字符串首字符的索引
    print(s.index('or'))       # 8
    # 找不到引发异常
    print(s.index('shit'))     # ValueError: substring not found
    ```

  + 逆向查找:rfind`和`rindex

+ 格式化

  ```python
  # center方法以宽度20将字符串居中并在两侧填充*
  print(s.center(20, '*'))  # ****hello, world****
  # rjust方法以宽度20将字符串右对齐并在左侧填充空格
  print(s.rjust(20))        #         hello, world
  # ljust方法以宽度20将字符串左对齐并在右侧填充~
  print(s.ljust(20, '~'))   # hello, world~~~~~~~~
  ```

+ | 变量值      | 占位符     | 格式化结果      | 说明                   |
  | ----------- | ---------- | --------------- | ---------------------- |
  | `-1`        | `{:+.2f}`  | `'-1.00'`       | 带符号保留小数点后两位 |
  | `3.1415926` | `{:.0f}`   | `'3'`           | 不带小数               |
  | `123`       | `{:0>10d}` | `0000000123`    | 左边补`0`，补够10位    |
  | `123`       | `{:x<10d}` | `123xxxxxxx`    | 右边补`x` ，补够10位   |
  | `123456789` | `{:,}`     | `'123,456,789'` | 逗号分隔格式           |
  | `0.123`     | `{:.2%}`   | `'12.30%'`      | 百分比格式             |
  | `123456789` | `{:.2e}`   | `'1.23e+08'`    | 科学计数法格式         |

+ `s.strip()` 修剪掉左右两端空格

### 列表(list [])

+ 允许**有重复**的数据，可**修改**。列表底层是一个可以动态扩容的数组。

+ 定义

  + 生成式 (最好)

    ```python
    items1 = [x for x in 'hello world' if x not in ' aeiou']
    items2 = [x + y for x in 'ABC' for y in '12']
    print(items3)    # ['A1', 'A2', 'B1', 'B2', 'C1', 'C2']
    ```

    

  + `[]`字面量语法

    ```python
    items1 = [35, 12, 99, 68, 55, 87]
    ```

  + 内置的`list`函数

    ```python
    items1 = list(range(1, 10))
    print(items1)    # [1, 2, 3, 4, 5, 6, 7, 8, 9]
    ```

+ 拼接、重复、成员运算、索引,切片,比较

  + 
    ```python
    # 列表的重复
    items4 = ['hello'] * 3
    print(items4)    # ['hello', 'hello', 'hello']
    ```

+ 添加和删除

  + 
    ```python
    # 使用append方法在列表尾部添加元素
    items.append('Swift')
    ```

    ```python
    # 使用insert方法在列表指定索引位置插入元素
    items.insert(2, 'SQL')
    ```
    
  + 
    ```python
    # 删除指定的元素
    items.remove('Java')
    ```
  
  + 
    ```python
    # 删除指定索引位置的元素
    items.pop(0)
    #和pop基本相同，但会返回删除的元素
    del items[1]
    ```
  
  + 
    ```python
    items.clear()
    # 清空列表中的元素    
    ```
  

+ 元素位置和次数

  + 
    ```python
    # 查找元素的索引位置
    print(items.index('Python'))   
    #从索引为3这个位置开始
    print(items.index('Java', 3))  
    ```
  
+ 
    ```python
    print(items.count('Python')) 
    ```
  
+ 排序和反转

  ```python
  items.sort()
  items.reverse()
  ```

+ 嵌套的列表 -> 表格或数学上的矩阵

  + 不能用`[[0] * 3] * 5]`这种方式来创建嵌套列表

  + 
    ```python
    scores = [[0] * 3 for _ in range(5)]
    ```

### 元组--tuple () 

+ 可重复，元组是**不可变**类型

+ 定义

  + 
    ```python
    # 定义一个四元组
    t2 = ('骆昊', 40, True, '四川成都')
    ```

  + `()`表示空元组

  + 只有一个元素，需要加逗号，否则是改变运算优先级的圆括号。`('hello', )`和`(100, )`才是一元组，而`('hello')`和`(100)`只是字符串和整数

+ 打包和解包 （解包语法对所有的序列都成立）

  ```python
  a = 1, 10, 100 # 打包
  i, j, k = a	# 解包
  
  a = 1, 10, 100, 1000
  i, j, *k = a  #星号变量会变成列表
  ```

+ 交换变量值

  ```python
  a, b, c = b, c, a
  ```

+ 让函数返回多个值 -> `return max_one, min_one`装成一个二元组然后返回,通过解包语法将二元组中的两个值分别赋给两个变量

+ 元组和列表相互转换 `list()`  `tuple()`

+ `enumerate`  循环遍历取到一个二元组，解包之后第一个值是索引，第二个值是元素

  ```python
  for index in range(len(items)):
      print(f'{index}: {items[index]}')
  
  for index, item in enumerate(items):
      print(f'{index}: {item}')
  ```

### 集合(set {})

+ 无序性->不能索引 ； 互异性-> 去重 ； 确定性：`(not) in`成员运算

+ 集合中的元素必须是`hashable`类型,通常不可变类型都是`hashable`类型 (集合本身也是可变类型)。集合底层使用了**哈希存储**的方式

+ 不可变类型的集合，名字叫`frozenset`,可以作为`set`中的元素

+ 创建

  + `{}`字面量语法 (至少有一个元素,否则是空字典)

    ```python
    set1 = {1, 2, 3, 3, 3, 2}
    ```

  + 构造器语法`set` (空集合可以使用`set()`)

    ```python
    set2 = set('hello')
    ```

  + 生成式（同list）

    ```python
    set4 = {num for num in range(1, 20) if num % 3 == 0 or num % 5 == 0}
    ```

+ 运算

  + `in`和`not in `
  + 交并差 (&|-)

+ 添加或删除

  + 
    ```
    set1.add(33)
    ```

  + 
    ```python
    set1.discard(100)
    ```

    ```python
    #建议先做成员运算再删除,因为不在集合中会引发KeyError异常
    if 10 in set1:
        set1.remove(10)
    print(set1)    # {33, 1, 55, 1000}
    ```

    ```python
    # pop->随机删除一个元素并返回该元素
    print(set1.pop())
    ```

    ```python
    # clear方法可以清空整个集合
    set1.clear()
    ```

+ 判断有无相同的元素可以使用` print(set1.isdisjoint(set2))  `

### 字典（zip{}）

+ **字典中的键必须是不可变类型** (列表，集合，字典不行，但是他们可以为键值)

+ 定义

  + 
    ```python
    person = {'name': '王大锤','age': 55,'weight': 60, }
    ```

  + 内置函数`dict`

    ```python
    person = dict(name='王大锤',age=55,weight=60)
    ```

    ```python
    # 可以通过Python内置函数zip压缩两个序列并创建字典
    items1 = dict(zip('ABCDE', '12345'))
    print(items1)    # {'A': '1', 'B': '2', 'C': '3', 'D': '4', 'E': '5'}
    ```

  + 生成式语法

    ```python
    items3 = {x: x ** 3 for x in range(1, 6)}
    ```

+ 修改

  ```python
  if 'age' in person:
      person['age'] = 25
  ```

+ 方法

  + 通过键获得值

    ```python
    # 取不到返回None或设定的默认值
    print(students.get(1005))    # None
    print(students.get(1005, {'name': '无名氏'}))    # {'name': '无名氏'}
    ```

  + 
    ```python
    # 获取字典中所有的键
    print(students.keys())      # dict_keys([1001, 1002, 1003])
    # 获取字典中所有的值
    print(students.values())    # dict_values([{...}, {...}, {...}])
    # 获取字典中所有的键值对
    print(students.items()) 
    ```

  + 删除

    ```python
    # 使用pop方法通过键删除键值,并返回该值
    stu1 = students.pop(1002)
    
    # 使用popitem方法删除字典中最后一组键值,返回对应的二元组
    # 如果字典中没有元素，调用该方法将引发KeyError异常
    key, value = students.popitem()
    
    #del,引不到对应的值，一样会引发KeyError异常
    person = {'name': '王大锤', 'age': 25}
    del person['age']
    ```

  + 更新

    ```python
    #setdefault可以更新字典中的键对应的值或向字典中存入新的键值对
    result = students.setdefault(1005, {'name': '方启鹤', 'sex': True})
    
    # 使用update更新字典元素，相同的键会用新值覆盖掉旧值，不同的键会添加到字典中
    ```

    