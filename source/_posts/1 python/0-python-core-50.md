---
title: python_core_50
date: 2021-11-28 21:53:28
tags: python
categories: python
---

# python-core-50

## L1-7 basic

+ 一些规则

  + 缩进：**通常使用4个空格**，强烈建议大家**不要使用制表键来缩进代码**

  <!--more-->

  + **Flat is better than nested**  少用嵌套

+ 注释

  + 单行注释：以`#`和空格开头
  + 多行注释：三个引号 可以折行

+ 变量

  + 命名
    + 大小写敏感
    + 受保护的变量用单个下划线开头
    + 私有的变量用两个下划线开头
  + type()
    + `chr()` : 整数 -- 字符 
    + `ord()` : 字符 -- 整数

+ 运算符

  + 逻辑运算符： `and`  `or`  `not`
  + `>` `<`  `==` 优先级高于 `=`

+ 输入输出

  + 输入：`f = float(input('请输入华氏温度: '))`

  + `print` 输出多个值（用逗号隔开）  默认以空格分开

  + 输出: 

    + 以`f`打头的字符串中，`{变量名}`是一个占位符，会被变量对应的值将其替换掉

    ```python
    print('%.1f华氏度 = %.1f摄氏度' % (f, c))   # %.1f是一个占位符
    print(f'{f:.1f}华氏度 = {c:.1f}摄氏度')  # 冒号后是格式
    print(f'{i}*{j}={i * j}', end='\t')
    ```

+ `range` : 前开后闭

  + `range(101)`：0-100整数
  + `range(1, 101)`：1-100整数
  + `range(1, 101, 2)`：1-100的奇数，2是步长
  + `range(100, 0, -2)`：100-1的偶数，-2是步长

+ 分支

  + `if`   `  elif`    `else`

+ 循环

  + for - in

    ```python
    for x in range(1, 101):
        ...
    ```

  + while

  + break & continue

    + break 只能终止它所在的那个循环
    + continue  直接让循环进入下一轮  用来放弃本次循环后续的代码

## L8-12 常用数据结构

### 列表 - 可变

+ 创建 `list`: 创建列表对象的构造器

  ```python
  items1 = [35, 12, 99, 68, 55, 87]
  items1 = list(range(1, 10))
  items2 = list('hello') # ['h', 'e', 'l', 'l', 'o']
  ```

+ 生成式创建 （**强烈建议**）

  ```python
  items1 = [x for x in range(1, 10)]
  items2 = [x for x in 'hello world' if x not in ' aeiou']
  items3 = [x + y for x in 'ABC' for y in '12']
  ```

+ 运算符

  ```python
  items3 = items1 + items2 # 拼接
  items4 = ['hello'] * 3 # 重复
  print(100 in items3)  # 成员运算
  items3[-1] # 索引 0到N-1 / -1到-N
  print(items3[-5:-7:-1]) # 切片
  items5 == items6 # 对应索引位置上的元素比较
  items5 <= items7
  ```

+ 嵌套 （矩阵）

  ```python
  scores = [[0] * 3 for _ in range(5)]  
  scores[0][0] = 95
  # [[95, 0, 0], [0, 0, 0], [0, 0, 0], [0, 0, 0], [0, 0, 0]]
  
  scores = [[0] * 3] * 5  # 不要用：浅拷贝，两行仍指向同一内存地址
  scores[0][0] = 95 
  # [[95, 0, 0], [95, 0, 0], [95, 0, 0], [95, 0, 0], [95, 0, 0]]
  ```

+ 遍历

  ```python
  for index in range(len(items)):
      print(items[index])
  ```

  ```python
  for item in items:
      print(item)
  ```

+ 方法

  + 添加

    ```python
    items.append('a') # 尾部添加
    items.insert(2, 'a') # 指定索引插入
    ```

  + 删除

    ```python
    items.remove('a') 
    items.pop(0) #删除指位置的元素
    del items[1]
    items.clear() # 清空
    ```

  + 查找索引 + 次数

    ```python
    print(items.index('a')) # 返回index
    print(items.index('a', 3)) #从索引为3以后的位置查找
    print(items.count('a')) 
    ```

  + 排序和反转

    ```python
    items.sort()
    items.reverse()
    ```

### 元组 - 不可变

+ 创建

  ```python
  t1 = (30, 10, 55)
  a = () # <class 'tuple'>
  d = ('hello', ) # <class 'tuple'>
  b = ('hello') # <class 'str'>
  ```

  tips: 如果元组中只有一个元素，需要加上一个逗号，否则`()`就不是代表元组的字面量语法，而是改变运算优先级的圆括号

+ 应用场景

  + 打包解包

    ``` python
    a = 1, 10, 100 # (1, 10, 100) 打包
    i, j, k = a # 解包
    i, j* = a # 星号接收多个值
    a, *b, c = 'hello'  # h ['e', 'l', 'l'] o
    ```

+ 方法 （不可变）

  + **拼接**、**成员运算**、**索引和切片**
  + 没有  添加元素、删除元素、清空、排序等

### 字符串 - 不可变

+ 转义

  + `r`或`R`开头，原始字符串

+ 运算

  + `+` 拼接
  + `*`重复
  + `in`    `not in`

+ 比较

  + `< = >`  比较对应的编码的大小
  + `s1 is s2` 比较内存地址

+ 查找

  ```python
  print(s.find('shit'))      # -1
  print(s.index('shit'))     # ValueError: substring not found
  print(s.find('o', 5))      # 从索引为5的位置开始查找字符o出现的位置
  ```

+ 修剪 -  `strip` :剪掉左右两端空格之后的字符串

  ```python
  s = '   jackfrued@126.com  \t\r\n'
  print(s.strip())    # jackfrued@126.com
  ```

+ 替换 - `replace`

  ```python
  s = 'hello, world'
  print(s.replace('o', '@'))     # hell@, w@rld
  ```

+ 拆分 合并 - `split`  `join`

  ```python
  s = 'I love you'
  words = s.split() # ['I', 'love', 'you']   
  print('#'.join(words))  # I#love#you
  ```

### 集合 - 可变

+ 性质

  + 无序性: 不支持索引
  + 互异性: 不允许重复
  + 底层哈希存储 -- 性能好
    + 元素必须是`hashable`类型 -- 不可变类型 （集合是可变的 不能嵌套）

+ 创建：`{}` 至少有一个元素，否则是空字典

+ 运算

  + `in `   `not in`
  + `&`   `|`   `-`  `^`(对称差 - 除开相交的部分)

+ 方法

  ```python
  set1.add(33)
  set1.update({1, 10, 100, 1000})
  
  set1.discard(100)
  
  if 10 in set1:
      set1.remove(10)
      
  # pop方法可以从集合中随机删除一个元素并返回该元素
  print(set1.pop())
  
  print(set1.isdisjoint(set2))    # 有没有相同的元素
  ```

+ 不可变集合 -`frozenset` (不能添加和删除元素)

### 字典

+ 有利于数据检索

+ 创建

  ```python
  person = {'name': '王大锤', 'age': 55}  # {}
  person = dict(name='王大锤', age=55)    # dict 构造器
  items1 = dict(zip('ABCDE', '12345'))   # zip压缩两个序列并创建字典 {'A': '1', 'B': '2', 'C': '3', 'D': '4', 'E': '5'}
  items3 = {x: x ** 3 for x in range(1, 6)}
  ```

+ 成员运算和索引

  + `for`循环只是对字典的键进行了遍历

    ```python
    if 'age' in person:
        person['age'] = 25
    
    person['tel'] = '13122334455'
    
    for key in person:
        print(f'{key}: {person[key]}')
    ```

+ 嵌套的字典 （字典中的键必须是不可变类型）

  ```python
  students = {
      1001: {'name': '狄仁杰', 'sex': True},
      1002: {'name': '白元芳', 'sex': True},
      1003: {'name': '武则天', 'sex': False}
  }
  ```

+ 方法

  ```python
  students.get(1002) # dict.get(key, default=None) 默认返回值
  
  print(students.keys())      # dict_keys([1001, 1002, 1003])
  print(students.values())    # dict_values([{...}, {...}, {...}])
  print(students.items())     # dict_items([(1001, {...}), (1002, {....}), (1003, {...})])
  
  for key, value in students.items():
      print(key, '--->', value)
      
  stu1 = students.pop(1002) # pop方法通过键删除对应的键值对并返回该值
  del student['1002']
  key, value = students.popitem() # 删除字典中最后一组键值对并返回对应的二元组
  
  # more： setdefault  update
  ```

## L13-16 函数

+ 参数（位置参数 命名关键字 关键字参数）

  + 顺序： 不带默认值 + 默认值

  ``` python
  def add(a=0, b=0, c=0):
      return a + b + c
  
  print(add(1, 2, 3))  # 位置参数（对号入座）
  print(add(c=50, a=100, b=200))    # 非顺序
  ```

  + **可变参数** `*`   -- 元组（不能处理带参数名的）

  ```python
  def add(*args):
      total = 0
      # 可变参数可以放在for循环中取出每个参数的值
      for val in args:
          if type(val) in (int, float):
              total += val
      return total
  ```

  + **命名关键字**：必须用 参数名 = 参数值 传入参数

  ```python
  def is_triangle(*, a, b, c):  # *前面：位置参数，*后面的：命名关键字参数
      return a + b > c and b + c > a and a + c > b
  print(is_triangle(a=3, b=4, c=5))
  ```

  + **关键字参数** `**` （字典：参数名 ； 参数）

    ```python
    def calc(*args, **kwargs):
        result = 0
        for arg in args:
            if type(arg) in (int, float):
                result += arg
        for value in kwargs.values():
            if type(value) in (int, float):
                result += value
        return result
    
    print(calc(1, 2, c=3, d=4))    # 10
    ```

+ 模块管理函数 （不同文件名下有相同函数名）

  + 完全限定名 -- 模块名.函数名

  ```python
  import module1 as m1
  m1.foo()
  ```

  + 别名

  ```python
  from module1 import foo as f1
  f1()
  ```

+ 高阶函数：函数本身也可以作为函数的参数 / 返回值  

  + 只需要函数名 不要括号 （调用函数才加括号）

  + example

    + filter(function, iterable序列)
    + map(function, 序列, ...)
    + reduce

    ```python
    numbers2 = list(map(lambda x: x ** 2, filter(lambda x: x % 2 == 0, numbers1)))
    # 判断素数的函数
    is_prime = lambda x: x > 1 and all(map(lambda f: x % f, range(2, int(x ** 0.5) 
    # 传入的序列中所有布尔值都是True，all函数就返回True
    ```

+ Lambda 函数 （匿名函数：一行代码）

  + 格式：lambda 参数: 执行表达式
  + 返回值为表达式结果

+ 装饰器 (本质：函数)

  +  参数：被装饰的函数
  + 返回值：有装饰功能的函数
  + 语法糖：`@装饰器函数`
  + `functools`模块的`wraps`函数（放在`wrapper`函数上），可保留被装饰之前的函数，用被装饰函数的`__wrapped__`属性获得被装饰之前的函数

  ```python
  import time
  from functools import wraps
  
  def record_time(func):
      
      # 定义一个带装饰功能（记录被装饰函数的执行时间）的函数
      # 因为具体参数，用*args和**kwargs接收所有参数
      @wraps(func) # 可以取消装饰器（带语法糖的）
      def wrapper(*args, **kwargs):
          start = time.time()
          result = func(*args, **kwargs) # 执行被装饰的函数并获取返回值
          end = time.time()
          print(f'{func.__name__}执行时间: {end - start:.3f}秒')
          return result # 返回被装饰函数的返回值
      
      return wrapper # # 返回带装饰功能的wrapper函数
  
  @record_time #语法糖：download变为装饰器record_time中返回的wrapper函数
  def download(filename):
      print(f'开始下载{filename}.')
      time.sleep(random.randint(2, 6))
      print(f'{filename}下载完成.')
      
  download = record_time(download) # 直接调用装饰器
  download('MySQL从删库到跑路.avi') # 利用语法糖
  download.__wrapped__('MySQL必知必会.pdf') # 取消装饰器
  upload = upload.__wrapped__ # 取消装饰器
  upload('Python从新手到大师.pdf')
  ```

+ 递归调用

  + 递归公式
  + 收敛条件 （结束递归条件）
  + 通过内存中的栈空间来保存现场和恢复现场，栈空间通常都很小(一般默认1000)，所以递归如果不能迅速收敛，很可能会引发栈溢出错误，从而导致程序的崩溃

  ```python
  def fac(num):
      if num in (0, 1): # 收敛条件
          return 1
      return num * fac(num - 1)
  ```

## L18-20 面向对象

> **面向对象编程**：把一组数据和处理数据的方法组成**对象**，把行为相同的对象归纳为**类**，通过**封装**隐藏对象的内部细节，通过**继承**实现类的特化和泛化，通过**多态**实现基于对象类型的动态分派

+ 解决问题： 创建对象 + 向对象发消息

+ **三步走**

  + 定义类 （内置类：list set dict）
  + 创建对象 (内置对象)
  + 给对象发消息

+ 对象 vs 类

  + 类--抽象 ；  对象--具体
    + 属性 -- 数据
    + 方法 -- 行为（函数）： 本质上也是属性
  + 对象一定属于某一个类；对象们的相同属性和方法抽象 -- 类 （eg. 人类：老师 + 学生）
  + 类的本质也是对象。一切都是对象：数据 + 函数 + 类 ...

+ 魔法方法 （`__xx__`）:以两个下划线开头和结尾，有特殊用途和意义的方法

+ 基本

  + 行为 - 方法（函数）：方法的第一个参数通常都是`self` -- 接收这个消息的对象本身
  + 属性 - 初始化（把数据放进内存）

  ```python
  class Student:  # 定义类
      __slots__ = ('name', 'age')  # 不允许动态添加属性   
  
      def __init__(self, name, age):
          self.name = name
          self.age = age
          
      def study(self, course_name):
          print(f'{self.name}正在学习{course_name}.')
  
      def play(self):
          print(f'学生正在玩游戏.')
          
      '''
      def __repr__(self): # __repr__魔术打印信息，不是地址
      	return f'{self.name}: {self.age}'
      '''
        
  stu1 = Student('骆昊', 40) # 用构造器创建Student类下的对象
  print(stu1)    # <__main__.Student object at 0x10ad5ac50> 内存中的逻辑地址
  
  # 给对象发消息的2种方法
  Student.study(stu1, 'Python程序设计') # 类.方法（对象 + 参数）
  stu1.study('Python程序设计')  # 对象.方法（参数）
  
  stu.sex = '男' # 动态为对象添加属性
  ```

+ 方法

  + 对象方法

  + 静态方法/类方法 （类本质也是对象）

    ```python
    class Triangle(object):
        """三角形类"""
    
        def __init__(self, a, b, c):
            """初始化方法"""
            self.a = a
            self.b = b
            self.c = c
    
        @staticmethod # 静态方法参数不用加类
        def is_valid(a, b, c): 
            return a + b > c and b + c > a and a + c > b
    
        @classmethod # 类方法 第一个参数为类这个对象本身
        def is_valid(cls, a, b, c):
            return a + b > c and b + c > a and a + c > b
    ```

+ 可见性 （很少用）

  + 方法：public

  + 属性

    + 私有 private (`_name`) ：类外无法访问，但可以通过类中方法访问

      ```python
      print(stu.__name) # 'Student' object has no attribute '__name'
      ```

    + 受保护 protected (`__name`)

    ```python
    # property装饰器为“私有”属性提供读取和修改
    class Student:
    
        def __init__(self, name, age):
            self.__name = name
            self.__age = age
    
        # 属性访问器(getter方法) - 获取__name属性
        @property
        def name(self):
            return self.__name
        
        # 属性修改器(setter方法) - 修改__name属性
        @name.setter
        def name(self, name):
            # self.__name = name if name else '无名氏'
            self.__name = name or '无名氏'
    
    print(stu.name, stu.age)    # 王大锤 20
    stu.name = ''  # 修改 -- ‘无名氏’
    ```

+ 支柱：

  + 封装：隐藏实现细节，暴露接口（方法名字 + 参数）

  + 继承: 在已有类的基础上创建新类 （父类 --> 子类） （祖先：`object`）

    + 继承父类 属性/方法
    + 创建新的 属性/方法

    ```python
    class Person:
    
        def __init__(self, name, age):
            self.name = name
            self.age = age
        
        def eat(self):
            print(f'{self.name}正在吃饭.')
    
    
    class Teacher(Person):
    
        def __init__(self, name, age, title):
            # super(Teacher, self).__init__(name, age)
            super().__init__(name, age)
            self.title = title
        
        def teach(self, course_name):
            print(f'{self.name}{self.title}正在讲授{course_name}.')
            
    teacher = Teacher('武则天', 35, '副教授')
    teacher.eat('米饭')
    ```

  + 多态：子类对父类方法改写 -- 调用相同的方法，做了不同的事情

+ EXAMPLE：

  > **说明**：简单起见，我们的扑克只有52张牌（没有大小王），游戏需要将52张牌发到4个玩家的手上，每个玩家手上有13张牌，按照黑桃、红心、草花、方块的顺序和点数从小到大排列，暂时不实现其他的功能。

  ```PYTHON
  from enum import Enum # m
  import random
  
  class Suite(Enum):
      """花色(枚举)"""
      SPADE, HEART, CLUB, DIAMOND = range(4)
      
  class Card:
      """牌"""
      def __init__(self, suite, face):
          self.suite = suite
          self.face = face
  
      def __repr__(self):
          suites = '♠♥♣♦'
          faces = ['', 'A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']
          # 根据牌的花色和点数取到对应的字符
          return f'{suites[self.suite.value]}{faces[self.face]}'
      
      def __lt__(self, other): # < 运算符重载
          # 花色相同比较点数的大小
          if self.suite == other.suite:
              return self.face < other.face
          # 花色不同比较花色对应的值
          return self.suite.value < other.suite.value
      
  class Poker:
      """扑克"""
      def __init__(self):
          # 通过列表的生成式语法创建一个装52张牌的列表
          self.cards = [Card(suite, face) for suite in Suite
                        for face in range(1, 14)]
          # current属性表示发牌的位置
          self.current = 0
  
      def shuffle(self):
          """洗牌"""
          self.current = 0
          # 通过random模块的shuffle函数实现列表的随机乱序
          random.shuffle(self.cards)
  
      def deal(self):
          """发牌"""
          card = self.cards[self.current]
          self.current += 1
          return card
  
      @property
      def has_next(self):
          """还有没有牌可以发"""
          return self.current < len(self.cards)
      
  class Player:
      """玩家"""
      def __init__(self, name):
          self.name = name
          self.cards = []
  
      def get_one(self, card):
          self.cards.append(card)
  
      def arrange(self):
          self.cards.sort()
          
          
  poker = Poker()
  poker.shuffle()
  players = [Player('东邪'), Player('西毒'), Player('南帝'), Player('北丐')]
  for _ in range(13):
      for player in players:
          player.get_one(poker.deal())
  for player in players:
      player.arrange()
      print(f'{player.name}: ', end='')
      print(player.cards)
  ```

> **要求**：某公司有三种类型的员工，分别是部门经理、程序员和销售员。需要设计一个工资结算系统，根据提供的员工信息来计算员工的月薪。其中，部门经理的月薪是固定15000元；程序员按工作时间（以小时为单位）支付月薪，每小时200元；销售员的月薪由1800元底薪加上销售额5%的提成两部分构成。

```python
from abc import ABCMeta, abstractmethod # 抽象类：专门勇于继承

class Employee(metaclass=ABCMeta):

    def __init__(self, name):
        self.name = name

    @abstractmethod # 装饰器 抽象方法
    def get_salary(self):
        pass
    
class Manager(Employee):
    def get_salary(self):
        return 15000.0


class Programmer(Employee):
    def __init__(self, name, working_hour=0):
        super().__init__(name)
        self.working_hour = working_hour

    def get_salary(self):
        return 200 * self.working_hour


class Salesman(Employee):
    def __init__(self, name, sales=0):
        super().__init__(name)
        self.sales = sales

    def get_salary(self):
        return 1800 + self.sales * 0.05
    
emps = [
    Manager('刘备'), Programmer('诸葛亮'), Manager('曹操'), 
    Programmer('荀彧'), Salesman('吕布'), Programmer('张辽'),
]
for emp in emps:
    if isinstance(emp, Programmer): # 判断对象的类的匹配
        emp.working_hour = int(input(f'请输入{emp.name}本月工作时间: '))
    elif isinstance(emp, Salesman):
        emp.sales = float(input(f'请输入{emp.name}本月销售额: '))
    print(f'{emp.name}本月工资为: ￥{emp.get_salary():.2f}元')
```
