[TOC]

# Python类的特殊方法
## 常见的特殊方法

### 重写__repr__
### 析构方法：__del__
### __dir__ 方法
### __dict__ 属性
### __getattr__、__setattr__ 等


## 与反射相关的属性和方法
如果程序在运行过程中要动态判断是否包含某个属性(包括方法)甚至要动态设置某个属性值，则可通过Python的反射支持来实现。

### 动态操作属性
动态检查对象是否包含相关属性(方法)的函数：

+ hasattr(obj,name):检查obj对象是否包含名为name的属性或方法。
+ getattr(object,name[,default]):获取object对象中名为name的属性的属性值。
+ setattr(obj,name,value):将obj对象的name属性设为value。

### __call__ 属性

上面程序可用hasattr（函数判断指定属性（或方法）是否存在，但到底是属性还是方法，则需要进一步判断它是否可调用。程序可通过判断该属性（或方法）是否包含call 属性来确定它是否可调用。

```
class User:
	def 一一工nit一一（ self, name, passwd):
		self.name =name
		self .passwd = passwd
	def validLogin (self) :
		print('验证%s的登录'%self.name)
u =User ('crazyit','leegang')
＃判断u .n ame 是否包含'__call__'方法， !ljl判断它是否可调用
print(hasattr(u . n缸腿， '__call__')) # False
＃判断u . passwd 是否包含＿call__方法，即判断它是否可调用
print(hasattr(u.passwd, '__call__'｝｝ # False
＃判断u . va li dLogin 是否包含call 方法，即判断它是否可调用
print(hasattr(u.validLgin ，’'__call__'')) #True
```

## 与序列相关的特殊方法
Python 的序列可包括多个元素，开发者只要实现符合序列要求的特殊方法，就可以实现自己序列。

### 序列相关方法
序列最重要的特征就是可包含多个元素，因此和序列有关的特殊方法有：
+ **__len__(sekf):** 该方法的返回值决定序列中元素的个数。
+ **__getitem__(self,key):**：该方法获取指定索引对应的元素。
+ **__contains__(self,item):**该方法判断序列是否包含指定元素。
+ **__setitem__(self,key,value):**该方法设置指定索引对应的元素。
+ **__delite__(self,key):**该方法删除指定索引对应的元素。

### 实现迭代器

使用for循环遍历列表、元姐和字典等，这些对象都是司法代的，因此它们都属于法代器。
<br>
如果开发者需要实现迭代器，只要实现如下两个方法即可：
+ **__iter__(self):**该方法返回一个法代器（ iterator ），迭代器必须包含一个next（）方法，该方法返回迭代器的下一个元素。
+ **__reversed__(self):**该方法主要为内建的reversed（）反转函数提供支持，当调用reversed()函数对指定迭代器执行反转时,实际上是由该方法实现的。
```
# 定义一个代表斐波那契数列的迭代器
class Fibs:
	def __init__(self,len):
		self.first=0
		self.sec=1
		self.__len__=len
	# 定义迭代器所需的__next__方法
	def __next__(self):
		# 如果__len__属性为0，结束迭代
		if self.__len__==0:
			raise StopIteration
		# 完成数列计算
		self.first,self.sec=self.sec,self.first+self.sec
		# 数列长度减1
		self.__len__-=1
		return self.first
	def __iter__(self):
		return self

if __name__ == '__main__':
	fibs=Fibs(10)
	print(next(fibs))
	for el in fibs:
		print(el,end=' ')
```

### 扩展列表、元组和字典

很多时候，如果程序明确需要一个特殊的列表、元组或字典类，有两种选择
+ 自己实现序列、法代器等各种方法，自己来实现这个特殊的类。
+ 扩展系统已有的列表、元组或字典。

很明显，第一种方式有点烦琐，因为这意味着开发者要把所有方法都自己实现一遍； 第二种方式就简单多了，只要继承系统己有的列表、元组或字典类，然后重写或新增方法即可。
```
class ValueDict(dict):
	# 定义构造函数
	def __init__(self,*args,**kwargs):
		# 调用父类的构造函数
		super().__init__(*args,**kwargs)

	# 新增getkeys(self,val)
	def getkets(self,val):
		result=[]
		for keys,value in self.items():
			if value==val:result.append(keys)
		return result

if __name__ == '__main__':
	my_dict=ValueDict(chinese=92,math=89,English=92)
	# 获取92对应的所有key
	print(my_dict.getkets(92))
	my_dict['Program']=92
	print(my_dict.getkets(92))
```
扩展的ValueDict 类新增了一个getkeys方法,该方法可以根据value 获取它对应的所有key组成的列表.
<br>
从上面的输出结果不难看出，我们扩展的这个字典类可以根据va l ue 获取对应的key,运行非常好，而程序只要扩展diet类，井新增一个方法即可，非常方便。

## 生成器

生成器和迭代器非常相似，它也会提供__next__()方法，这意味着程序同样可调用内置的next()函数来获取生成器的下一个值，也可使用for循换来遍历生成器。
<br>
**生成器和迭代器的区别在于:**
+ 迭代器通常先定义一个迭代器类，然后通过创建实例来创建迭代器；
+ 生成器则先定义一个包含yield语句的函数，然后通过调用该函数来创建生成器。

### 创建生成器

创建生成器的两步操作:
+ 定义一个包含yield语句的函数
+ 调用上一步创建的函数得到的生成器

```
def test(val,step):
	print("**** function start ******")
	cur=0
	# 遍历0~val
	for i in range(val):
		# cur添加i*step
		cur+=i*step
		yield cur

if __name__ == '__main__':
	for item in test(10,5):
		print(item)
```
yield cur语句的作用有两点：
+ 每次返回一个值，有点类似return语句
+ 每次冻结执行，程序没执行到yield语句会被暂停
在程序被yield语句冻结之后，当程序再次调用next函数获取生成器的下一个值时，程序才会继续向下执行。
<br>
需要指出的是，调用包含住yield语句的函数并不会立即执行，它只是返回一个生成器。只有当程序通过next()函数调用生成器或遍历生成器时，函数才会真正执行。

**Tip:Python 2.x 不使用next()函数来获取生成器的下一个值，而是直接调用生成器的next()方法。也就是说，Python 2.x 中应该写成t.next()**

<br>

**Python主要提供了两种方法来创建生成器：**
+ 使用for循环的生成器推导式
+ 调用带yield语句的生成器函数

**使用生成器至少有以下几个优势：**
+ 当使用生成来生成多个数据时，程序是按需获取数据的，它不会一开始就把所有数据都生成出来，而是每次调用next()获取下一个数据时，生成器才会执行一次，因此可以减少代码的执行次数。
+ 当函数需要返回多个数据时，如果不使用生成器，程序就需要使用列表或元组来收集函数返回的多个值，当函数要返回的数据量较大时，这些列表、元组会带来一定的内存开销；如果使用生成器就不存在这个问题，生成器可以按需、逐个返回数据。
+ 使用生成器的代码更简洁

### 生成器的方法

在生成器函数内部，程序可通过yield表达式来获取send()方法发送的值——这意味着此时程序应该使用一个变量来接收yield语句的值。如果程序依然使用next()函数来获取生成器所生成的下一个值，那么yield语句返回None。

+ 外部程序通过send()方法发送数据
+ 生成器函数使用yield语句接收数据



```

```
