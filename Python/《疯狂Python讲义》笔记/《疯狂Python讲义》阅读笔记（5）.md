[TOC]

# 异常处理

## 异常概念
异常机制己经成为衡量一门编程语言是否成熟的标准之一，使用异常处理机制的Python程序有更好的容错性，更加健壮。
## 异常处理机制

Python 的异常处理机制可以让程序具有极好的容错性，让程序更加健壮。当程序运行出现意外情况时，系统会自动生成一个Error 对象来通知程序，从而实现将“业务实现代码”和“错误处理代码”分离，提供更好的可读性。

### 使用Try...except捕获异常
```
try:
	# 业务实现代码
	···
except(Error1,Error2,...) as e:
	alert 输入不合法
	goto retry
```
不管程序代码块是否处于try块中，甚至包括except块中的代码，只要执行该代码块时出现了异常系统总会自动生成一个Error 对象。如果程序没有为这段代码定义任何的except 块，则Python解释器无法找到处理该异常的except块，程序就会退出。

### 异常类的继承体系
Python的所有异常类都从BaseException派生而来，提供了丰富的异常类，这些异常类之间有严格的继承关系。
+ BaseException
++ GeneratorExit
++ Exception
+++ ArithmeticError
++++ ZeroDivisionError
++++ FloatingPointError
++++ OverflowError
+++ BufferError
+++ LookupError
++++ IndexError
++++ KeyError
++ SystemExit
++ KeyboardInterrupt

### 多异常捕获

python的一个except块可以捕获多种类型的异常
在使用一个except块捕获多种类型的异常时，只是将多个异常类用括号括起来，中间用逗号隔开
```
import sys
try:
	a=int(sys.argv[1])
	b=int(sys.argv[2])
	c=a/b
	print('您输入的两个数相除的结果',c)
except (IndexError,ValueError,ArithmeticError) as e:
	print("程序发生了数组越界、数字格式异常、算数异常之一")
except:
	print("未知异常")
```

### 访问异常信息

所有的异常对象都包括如下几个常用属性和方法
+ args:该属性返回异常的错误编号和描述字符串。
+ errno:该属性返回异常的错误编号。
+ strerror:该属性返回异常的描述字符串。
+ with_traceback():通过该方法可处理异常的传播轨迹信息。

```
def foo (} :
	try:
		fis = open ("a.txt") ;
	except Exception as e :
		＃访问异常的错误编号和详细信息
		print(e.args)
		＃访问异常的错误编号
		print(e.errno)
		＃访问异常的详细信息
		print(e.strerror)
foo()
```
### else块

在Python的异常处理块中还可以添加一个else块，当try块没有出现异常时，程序会执行else块。例如如下程序：
```
s = input （’请输入除数：’）
try:
	result= 20 I int(s)
	print （’ 20 除以h 的结果是： 坷’ 毛（ s , result))
except ValueError :
	print （’值错误，您必须输入数值’）
except Arithmet 工cError:
	print （＇算术错误，您不能输入。’）
else:
	print （’没有出现异常’）
```

### 使用finally回收资源

有些使用，程序在try块里打开一些物理资源(例如数据库连接、网络连接和磁盘文件等)，
这些物理资源都必须被显式回收。
+ Python的垃圾回收机制不会回收任何物理资源，只能回收堆内存中对象所占用的内存。
为了保证一定能回收在try块中打开的物理资源，异常处理机制提供了finall模块。
+ 不管try块中的代码是否出现异常，也不管哪一个except 块被执行，甚至在try 块或except块中执行了return 语句,finally块总会被执行。

## 使用raise引发异常

### 引发异常
如果需要在程序中自行车引发异常，则应使用raise语句。raise语句有如下三种常用的用法。
+ raise：单独一个raise。该语句引发当前上下文中捕获的异常
+ raise 异常类：raise后带一个异常类。该语句引发指定异常类的默认实例
+ raise 异常对象：引发指定的异常对象。

### except和raise同时使用
```
class Auctio 日Exception(Exception) : pass
class Auct 工onTest:
	def 一＿ in 工t一（ self , init_price) :
		self.init pr 工ce = init price
	def bid(self, bid price):
		d = 0.0
		try :
			d = float (bid price)
		except Exception as e:
			＃此处只是简单地打印异常信息
			print （"转换出异常：", e)
			＃再次引发自定义异常
			raise AuctionExcept工∞｛”竞拍价必须是数值，不能包含其他字符！”） ＃①
		if self . init pr 工ce > d :
			raise AuctionException （ ” 竞拍价比起拍价低，不允许竞拍！”）
		initPrice = d
	def main() :
		at= AuctionTest(20.4)
		try:
			at.bid (” df”)
		except Auct 工onException as ae:
main()
```
### raise不需要参数
raise语句处于except块中，它将会自动引发当前上下文激活的异常；否则，默认引发RuntimeError异常。

## 异常处理规则
前面介绍了使用异常处理的优势、便捷之处，本节将进一步从程序性能优化、结构优化的角度给出异常处理的一般规则。成功的异常处理应该实现如下4 个目标。
+ 使用程序代码混乱最小化
+ 捕获并保留诊断信息
+ 通知合适的人员
+ 采用合适的方式结束异常活动

### 不要过度使用异常
不可否认， Python 的异常机制确实方便，但滥用异常机制也会带来一些负面影响。过度使用异常主要表现在两个方面。
+ 把异常和普通错误混淆在一起，不再编写任何错误处理代码，而是以简单地引发异常来代苦所有的错误处理。
+ 使用异常处理类代替流程控制。

### 不要使用过于庞大的try块
+ try块里放置大量的代码，看上去很简单，但因为try块里的代码过于庞大，业务过于复杂，就会造成try块中出现异常的可能性大大增加，从而导致分析异常原因的难度也大大增加。

### 不要忽略捕获到异常
不要忽略异常！既然已捕获到异常，那么except块里应该做些有用的事情——处理并修复异常。except块整个为空，或者仅仅打印简单的异常信息都是不妥的！
<br>
except 块为空就是假装不知道甚至瞒天过海，这是最可怕的事情一一程序出了错误，所有人都看不到任何异常，但整个应用可能已经彻底坏了。

+ 建议对异常采取适当措施：
++ **处理异常**：对异常进行合适的修复，然后绕过异常发生的地方继续进行
++ **重新引发新的异常**：把在当前运行环境下能做的事情尽量做完，然后进行转译，把异常包装成当前层的异常，重新传给上层调用者。
++ **在合适的层处理异常**:。如果当前层不清楚如何处理异常，就不要在当前层使用except语句来捕获该异常，让上层调用者来负责处理该异常。


