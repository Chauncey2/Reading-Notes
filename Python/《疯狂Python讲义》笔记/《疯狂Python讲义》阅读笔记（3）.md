[TOC]
# 函数
## 函数的参数
### 关键字（keyword）参数

+ 如果使用位置参数的方式来传入参数值，则必须严
格按照走义函数时指定的顺序来传入参数值：如果根据参数名来传入参数值，则无须遵守定义形参的顺序，这种方式被称为关键字（ keyword)参数

### 参数收集（个数可变的参数）

很多编程语言都允许定义个数可变的参数，这样可以在调用函数时传入任意多个参数。Python
当然也不例外， Python 允许在形参前面添加一个星号（叫，这样就意味着该参数可接收多个参数
值，多个参数值被当成元组传入。下面程序定义了一个形参个数可变的函数。

### 逆向参数收集
> 所谓逆向参数收集，指的是在程序已有列表、元组、字典等对象的前提下，把它们的元素“拆开”后传给函数的参数。
> 逆向参数收集需要在传入的列表、元组参数之前添加一个星号，在字典参数之前添加两个星号。
```
def test(name,message):
	print("用户是：",name)
	print("欢迎消息：",message)
my_list=['孙悟空','欢迎来疯狂软件']
test(*my_list)
```
 
***
###变量作用域

+ 局部变量：在函数中定义的变量，包括参数，都被称为局部变量
+ 全局变量：在函数外面、全局范围内定义的变量。

每个函数在执行时，系统都会为该函数分配一块“临时内存空间”，所有的局部变量都被保存在这块临时内存空间内。当函数执行完成后，这块内存空间就被释放了，这些局部变量也就失效了，因此离开函数之后就不能再访问局部变量了。

+ Python提供了三个工具函数来获取指定范围内的“变量字典”:

++ globals():返回全局范围内所有变量组成的“变量字典”
++ locals()：该函数返回当前局部范围内所有变量组成的“变量字典”
++ vars(object):获取在指定对象范围内所有变量组成的“变量字典”

## 局部函数

+ 在全局范围内定义的，它们都是全局函数。Python还支持在函数体内定义函数，这种被放在函数体内定义的函数被称为局部函数。在默认情况下，局部函数对外部是隐藏的，局部函数只能在其封闭（enclosing）函数内有效。封闭函数也可以返回局部函数，以便程序在其他作用域中使用局部函数。
```
def get_math_func(type,nn):

	def square(n):
		return n*n
	def cube(n):
		return n*n*n
	def factorial(n):
		result=1
		for index in range(2,n+1):
			result*=index
		return result

	if type=="square":
		return square(nn)
	elif type=="cube":
		return cube(nn)
	else:
		return factorial(nn)

print(get_math_func("square",3)) 
print(get_math_func("cube",3))
print(get_math_func("",3))
```
运行输出:

```
9
27
6
```

***
+ 局部函数的变量也会遮蔽它所在函数内的局部变量
啥意思，意思是，在局部函数里引用了封闭函数内定义的局部变量，并且改变了它的值，那么就会遮蔽封闭函数内的局部变量原来的值。
还是迷糊？看代码

```
def foo():
	# 局部变量name
	name='Charlie'
	def bar():
		# 访问bar函数所在foo函数内的name局部变量
		print(name)
		name="abc"
	bar()

foo()
```
程序输出
> UnboundLocalError: local variable 'name' referenced before assignment

** 该错误是由局部变量遮蔽局部变量导致的，在bar（）函数中定义的name 局部变量遮蔽了它所在
foo（）函数内的name 局部变量 **

> 为了生命bar()函数中的name='abc'赋值语句不是定义新的局部变量，只是访问它所在
foo （）函数内的name 局部变量。
> Python 提供了nonlocal关键字，通过nonlocal 语句即可声明访问赋
值语句只是访问该函数所在函数内的局部变量。

```
def foo():
	# 局部变量name
	name='Charlie'
	def bar():
		nonlocal name
		# 访问bar函数所在foo函数内的name局部变量
		print(name)
		name="abc"
	bar()
foo() # Charlie
```

** nonlocal和global功能大致相似，区别是global用于声明全局变量，而nonlocal用于声明访问当前函数所在函数内的局部变量 **

***

## 函数高级内容

### 函数变量

+ Python的函数也是一种值：所有函数都是function对象，这意味着可以把函数本身赋值给变量。
+ 当函数赋值给变量之后，程序就可以通过该变量来调用函数。

```
def pow(base,exponent):
	result=1
	for i in range(1,exponent+1):
		result*=base
	return result

my_func=pow
print(my_func(3,4))
```

### 使用函数作为函数形参

+ 有时候定义一个函数，函数的大部分逻辑可以确认，但某些处理逻辑暂时无法确定——某些程序代码需要动态改变。
+ ，如果希望调用函数时能动态传入这些代码，那么就需
要在函数中定义函数形参，这样即可在调用该函数时传入不同的函数作为参数，从而动态改变这段代码。

```
def map(data,fn):
	result={}
	for e in data:
		result.append(fn(e))
	return result

def square(n):
	return n*n

data=[3,4,9,5,7,]

print(map(data,square))
```
### 函数作为返回值

Python 支持使用函数作为其他函数的返回值

```
def get_math_func(type):
	
	def square(n):
		return n*n

	def cube(n):
		return n**3

	if type=='square':
		return square
	else:
		return cube

math_func=get_math_func('cube')
prit(math_func(5))
math_func=get_math_func('square')
print(math_func(5))
```

### lambda函数

```
def get math_func(type)
	result=l

	if type== 'square ’:
		return l扭曲da n: n * n 
	elif type == ’ cube ’:
		return l缸曲da n: n * n * n 
	else :
		return 1缸nbda n: (1 + n) * n I 2  
```
+ lambda表达式只能食单行表达式、不允许使用更复杂的函数形式。

+ 总体来说，函数的适用性要比lambda表达式强，lambda只只能创建简单的函数对象（它只适合函数体为单行的情形）。
+ 但lambda 表达式依然有如下两个用途：
++ 对于单行函数，使用lambda 表达式可以省去定义函数的过程，让代码更加简洁。
++ 对于不需要多次复用的函数， 使用lambda 表达式可以在用完之后立即释放，提高了性能

***




