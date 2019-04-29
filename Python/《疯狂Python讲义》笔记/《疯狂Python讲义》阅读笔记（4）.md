[TOC]
# 类和对象
在设计之初， Python 就被设计成支持面向对象的编程语言，因此Pyt hon 完全能以面向对象的
方式编程。而且Python 的面向对象比较简单，它不像其他面向对象语言提供了大量繁杂的面向对
象特征，它致力于提供简单、够用的语法功能。
正因为如此，在Python 中创建一个类和对象都很容易。Python 支持面向对象的三大特征：主才
装、继承和多态，子类继承父类同样可以继承到父类的变量和方法。

## 类和对象

+ 不用多说，类是对某一批对象的抽象，
+ Python对象的实例变量也可以动态增加或删除——只要对新实例变量赋值就是增加实例变量，因此程序可以在任何地方为已有的对象增加实例变量
+ 程序可通过del语句删除已有对象的实例变量
+ 在类中定义的方法默认是实例方法，定义实例方法的方法与定义函数的方法基本相同，知识实例方法的第一个参数会被绑定到方法的调用者（该类的实例）——因此实例方法至少应该一个参数，该参数通常会被命名为self

> 实例方法的第一个参数并不一定叫self，其实完全可以叫任意参数名，只是约定俗称地把该参数命名为self，这样具有最好的可读性。

在实例方法中有一个特别的方法：__init__，这个方法被称为构造方法。构造方法用于构造该类的对象，Python通过调用构造方法返回该类的对象（无须使用new）

## 方法

### 类也能调用实例方法
+ 在Python的类中定义的方法默认都是实例方法
+ Python的类在很大程度上是一个命名空间

### 类方法与静态方法
+ Python完全支持定义类方法，甚至支持定义静态方法。Python的类方法和静态方法很相似，它们都推荐使用类来调用（其实也可使用对象来调用）。
+ 类方法和静态方法的区别在于：Python会自动绑定类方法的第一个参数，类方法的第一个参数（通常建议参数名为cls）会自动绑定到类本身；但对于静态方法则不会自动绑定
+ 方式
++ @classmethod:修饰类方法
++ @staticmethod：修饰静态方法

```
class Bird:

	@classmethod
	def fly(cls):
		print('class method',cls)

	@staticmethod
	def info(p):
		print('static method',p)
# 调用类方法，Bird类会自动绑定到第一个参数
Bird.fly()
# 调用静态方法,不会自动绑定，因此程序必须手动绑定第一个参数
Bird.info('crazyit')

# 创建Bird对象
b=Bird()
# 使用对象调用fly()方法
b.fly()

b.info('abc')

```

在使用Python编程时，一般不需要使用类方法或静态方法，程序完全可以使用函数来代替类方法或静态方法。但是在特殊的场景（比如使用工厂模式）下，类方法或静态方法是不错的选择。

### @函数装饰器

+ @staticmethod和@classmethod 的本质就是函数装饰器，其中staticmethod和classmethod都是Python内置函数。

+ 自定义装饰器
```
def funA(fn):
	print('A')
	fn() # 执行传入的fn参数
	return 'fkit'

@funA
def funB():
	print('B')

print(funB)
```
输出

```
A
B
fkit
```
+ @funA修饰funB，程序完成了两步操作。
+ 将funB作为funA()的参数，也就是上面的粗体字代码相当于执行funA（funB） 
+ funB替换成第一步执行的结果，funA执行完成后fkit，因此funB就不再是函数，而是被替换成一个字符串

###  类命名空间
+ Python程序默认处于全局命名空间内，类体则处于类命名空间内，Python允许在全局范围内放置可执行代码

### 使用property函数定义属性
如果Python类定义了getter、setter等访问器方法，则可以使用property()函数将它们定义成属性（相当于实例变量）

```
class Rectangle:
	def __init__(self,width,height):
		self.width=width
		self.height=height

	def setsize(self,size):
		self.width,self.height=size

	def getsize(self):
		return self.width,self.height

	def delsize(self):
		self.width,self.height=0,0

	# 使用property定义属性
	size=property(getsize,setsize,delsize)

print(Rectangle.size.__doc__)
help(Rectangle.size.__doc__)
```

## 隐藏和封装
封装(Encapsulation)是面向对象的三大特征之一（另外两个是继承和多态）
+ Python 并没有提供类似于其他语言的private等修饰符，因此Python并不能真正支持隐藏。
+ Python类的成员命名为以双下划綫开头的，Python就会把它们隐藏起来。

## Python的动态性

+ Python是动态语言，动态语言的典型特征就是：类、对象的属性、方法都可以动态增加和修改。

### 动态属性与__slots__
+ 如果希望为所有实例都添加方法，则可通过为类添加方法类实现。
```
class Cat:
	def __init__(self,name):
		self.name=name

def walk_func(self):
	print('working',self.name)

d1=Cat('gar')
d2=Cat('kitty')

# 为Cat动态添加walk方法，该方法的第一个参数会自动绑定
# d1.walk() # AttributeError
Cat.walk=walk_func
d1.walk()
d2.walk()
```
+ Cat类动态添加了一个方法walk(),为类动态添加方法时，不需要使用MethodType进行包装，该函数的第一个参数会自动绑定。
+ Python的这种动态性有其优势，比如灵活，但也给程序带来一定的隐患：
++ 程序定义好的类，完全有可能在后面被其他程序修改，带来了不确定性



