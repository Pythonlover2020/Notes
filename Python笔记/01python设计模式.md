

# Python设计模式

> [Java设计模式](http://m.biancheng.net/design_pattern/)

## 设计思想

### 1.封装

#### 数据角度

> **多种数据合为一种数据**
>
> **优势：代码可读性高**
>
>            **将数据与行为相关联**
>
> **例如：电脑（内存，储存空间，...）**

#### 行为角度

> **提供[必要]功能，隐藏细节（方法体，方法本身）**
>
> **隐藏成员，以双下划线命名（如：\_\_a）**
>
> **属性：保护数据（加工数据，只读，只写）**

实例：

```python
class A:
    def __init__(self,n):
        self.__n = n
    @property
    def n(self):
        return self.__n
    @n.setter
    def n(self,value):
        self.__n = value
    @n.deleter
    def n(self):
        print("n被del了")
a = A(10)
print(a).n # 10
a.n = 20
print(a).n # 20
del a.n # n被del了
```

> [@property 详解](https://www.liaoxuefeng.com/wiki/1016959663602400/1017502538658208)

#### 设计角度

> **分而治之：需求分为多个类（行为）**
>
> **变则疏之：将变化点单独定义到类中**
>
> **高内聚：一个类的内部  干一件事。单一职责**
>
> **低耦合：类与类的关系松散**
>
> **类与类行为不同，对象与对象数据不同**

### 2.多态

> **传入不同的实例对象，做不同的事**

实例：

```python
class Player:
    def __init__(self,name):
        self.name = name
    def attack(self,player):
        print(f'{player.name} 被 {self.name} 攻击了')

player1 = Player('小宇宙zjy')
player2 = Player('小杜同学')
player1.attack(player2) # 小杜同学 被 小宇宙zjy 攻击了
```

## 设计原则

### 1.开闭原则

> **允许增加新功能，不允许修改客户端代码（调用方）**

比如Windows 的主题是桌面背景图片、窗口颜色和声音等元素的组合。用户可以根据自己的喜爱更换自己的桌面主题，也可以从网上下载新的主题。这些主题有共同的特点，可以为其定义一个抽象类（Abstract Subject），而每个具体的主题（Specific Subject）是其子类。用户窗体可以根据需要选择或者增加新的主题，而不需要修改原代码，所以它是满足开闭原则的，其[类图](http://m.biancheng.net/view/1319.html)如图所示。

![](http://m.biancheng.net/uploads/allimg/181113/3-1Q113100151L5.gif)

实例：

```python
from termcolor import colored
from abc import abstractmethod,ABCMeta
class Printer(metaclass=ABCMeta): # 抽象类（接口）
    @abstractmethod
    def print(self,text): # 子类必须实现该方法，否则报错，同时抽象类不能被实例化
        pass

class New_Printer(Printer):
    def print(self,text,color):
        print(colored(text=text,color=color))

class N_Printer(Printer):
    pass

n_P = N_Printer() # 报错：TypeError: Can't instantiate abstract class N_Printer with abstract methods print（无法使用抽象方法print实例化抽象类N_Printer）
p = Printer() # 报错：TypeError: Can't instantiate abstract class Printer with abstract methods print（无法使用抽象方法实例化抽象类Printer）
n = New_Prin()
n.print('Hello','red')
```

### 2.单一职责

> **一个类（函数）有且只有一个改变它的原因，否则类应该被拆分**

例：
大学学生工作主要包括学生生活辅导和学生学业指导两个方面的工作，其中生活辅导主要包括班委建设、出勤统计、心理辅导、费用催缴、班级管理等工作，学业指导主要包括专业引导、学习辅导、科研指导、学习总结等工作。如果将这些工作交给一位老师负责显然不合理，正确的做 法是生活辅导由辅导员负责，学业指导由学业导师负责，其类图如图所示。
![](http://m.biancheng.net/uploads/allimg/181113/3-1Q113133F4161.gif)

### 3.依赖倒置（依赖抽象）

> **客户端代码（调用的类）尽量依赖（使用）抽象。**
>
> **抽象不应该依赖细节，细节应该依赖抽象**

_依赖倒置原则是实现开闭原则的重要途径之一，它降低了客户与实现模块之间的耦合。_

实例：

```python
from abc import ABCMeta,abstractmethod

class Shop(metaclass=ABCMeta):
    @abstractmethod
    def sell(self):
        pass

class Shao_Guan_Shop(Shop):
    def sell(self):
        return '韶关土特产'

class Hei_Long_Jiang_Shop(Shop):
    def sell(self):
        return '黑龙江土特产'

class Customer:
    def __init__(self,shop_class_list):
        self.shop_class_list = shop_class_list
    def shopping(self):
        for self.shop_class in self.shop_class_list:
            print(self.shop_class.sell(self))

c = Customer([Shao_Guan_Shop,Hei_Long_Jiang_Shop])
print('顾客买了：')
c.shopping()
```

**_即：调用父类，不调用子类_**

### 4.组合复用原则

> **在代码复用时，要尽量先使用组合或者聚合等关联关系来实现，其次才考虑使用继承关系来实现。**

**_如果使用继承，必须确保父类所拥有的性质在子类中仍然成立_**

例：继承复用

```python
class A:
    def a(self):
        print('A -- a')

class B(A):
    def b(self):
        print('B -- b')
        # 继承复用
        super().a()

b = B()
b.b()
```

**缺点：紧耦合，父类变化直接影响所有后代<br />
可以使用组合复用**

例：

```python
class A:
    def a(self):
        print('A -- a')

class B(A):
    def __init__(self,a_object):
        self.a_object = a_object
    def b(self):
        print('B -- b')
        self.a_object.a() # 组合复用

# 如果传递其他对象，等同于修改了b方法的行为，还不影响b方法
b = B(A())
b.b()
```

### 5.迪米特法则

> **不要和陌生人说话**

**_类与类交互时，在满足功能要求的基础上，传递的数据量越少越好，这样可以降低耦合度_**
