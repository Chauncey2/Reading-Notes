[TOC]
# 正则表达式
正则表达式（Regular Expression）用于描述一种字符串匹配模式（Pattern），它可用于检查一个字符串是否含有某个子串。

## Python的正则表达式支持

+ re.compile(pattern,flags=0)
+ re.match(pattern,string,flags=0)
+ re.search(pattern,string,flags=0)
+ re.findall(pattern,string,flags=0)
+ re.finditer(pattern,string,flags=0): 扫描整个字符串，并返回字符串中所有匹配pattern的子串组成的迭代器，迭代器的元素是_sre.SRE_Match对象。
+ re.funllmatch(pattern,string,flags=0):该函数要求整个字符串能匹配pattern,如果匹配则返回包含匹配信息的_sre.SRE_Match对象，否则返回None。
+ re.sub(pattern,repl,string,count=0,flags=0):替换匹配到的字符
+ re.purge(): 清除正则表达式缓存。
+ re.escape(pattern):对模式中除了ASCII字符，数值、下划线( _ )之外的其他字符进行转义。

## 正则表达式旗标

Python 支持的正则表达式旗标都使用该模块中的属性来代表，这些旗标如下所示：
+ **re.A 或re.ASCII:**该旗标控制\w,\W,\b,\B,\d,\D,\s和\S只匹配ASCII字符，而不匹配所有的Unicode字符。也是可以在正则表达式中使用（?a）行内旗标来代表。
+ **re.DEBUG：**显示编译正则表达的Debug信息。没有内旗标
+ **re.I或re.IGNORECASE：**不区分大小写
+ **re.L或re.LOCALE：**根据当前区域设置使用正则表达式匹配时不区分大小写。该旗标只能对bytes 模式起作用，
+ **re.M或re.MULTILINE**：多行模式的旗标。当指定该旗标后， “〈”能匹配字符串的开头和每行的开头（紧跟在每一个换行符的后面〕；“$”能匹配字符串的末尾和每行的末尾（在每一个换行符之前〉。在默认情况下，“〈”只匹配字符串的开头，“$”只匹配字符串的结尾，或者匹配到字符串默认的换行符（如果有〉之前。
+ **re.S或re.DOTALL:**让点“.”能匹配包括换行符在内的所有字符，如果不指定该旗标，则点“.”能匹配不包括换行符的所有字符。
+ **re.U或re.Unicode:**该旗标控制\w, \W, \b，\B，\d, \D, \s 和\S 能匹配所有的Unicode字符。
+ **re.X或re.VERBOSE:**通过该旗标允许分行书写正则表达式，也允许为正则表达式添加注释，从而提高正则表达式的可读性。

***
# 容器相关类

## set和frozenset
set集合有如下两个特征:
+ set 不记录元素的添加顺序
+ 元素不允许重复
set集合是可变容器，程序可以改变容器中的元素。与set对应的还有frozenset集合,
frozenset是set的不可变版本，它的元素是不可变的。
```
l=[e for e in dir(set) if not e.startswith('_')]
print(l)
```
对于上面这些方法，其方法名己经暗示了它们的作用,比如add()很明显就是向set 集合中添加元素,remove()、discard()就是删除元素,clear()就是清空白set集合。

> remove()与discard()方法都用于删除集合中的元素，但区别在于：如果集合中不包含被删除的元素，remove()方法会爆出KeyError异常，而discard()方法则什么也不做。

## 双端队列（deque）
战是一种特殊的线性表，它只允许在一端进行插入、删除操作，这一端被称为核顶（top)另一端则被称为核底（ bottom ）。
<br>
deque位于collections包下。

+ append和appendleft:在deque的右边或左边添加元素，默认在队列尾添加元素
+ pop和popleft:在deque的右边或左边弹出元素，也就是默认在队列尾弹出元素。
+ extend和extendleft:在deque的右边或左边添加多个元素，也就是默认在队列尾添加多个元素。

***
## Python的堆操作

Python 提供了关于堆的操作。

Python并没有提供“堆”这种数据结构，它是直接把列表当成堆处理的。Python提供的heapq包中有一些函数，当程序用这些函数来错操作列表时，该列表就会表现出“堆”的行为。
```
print(heapq.__all__)
```
输出
```
['heappush', 'heappop', 'heapify', 'heapreplace', 'merge', 'nlargest', 'nsmallest', 'heappushpop']
```
+ heappush(head,item):将item元素加入堆；
+ heappop(head): 将堆中最小元素弹出；
+ heapify(head):将堆属性应用到列表上；
+ heapreplace(heap,x):将堆中最小元素弹出，并将元素x入堆；
+ merge(*iterable,key=None,reverse=False):将多个有序的堆合并成一个大的有序堆，然后再输出；
+ heappushpop(heap,item):将item入堆，然后弹出并返回堆最小的元素；
+ nlargesr(n,iterable,key=None):返回堆中最大的n个元素；
+ nsmallest(n,iterable,key=None):返回堆中最小的n个元素
```
from heapq import *
my_data=list(range(10))
my_data.append(0.5)
# 此时my_data依然是一个list列表
print('my_data:',my_data)
# 对my_data应用堆属性
heapify(my_data)
print("before use heapq",my_data)
heappush(my_data,7.2)
print('insert 7.2 into heapq:',my_data)

# 弹出堆中最小的元素
print(heappop(my_data))
print(heappop(my_data))
```
***

# collection下的容器支持

在collections包中除包含deque容器来之外，还包含另外一些容器类，这些容器类可能不知前面介绍的容器类那么常用，但在实际开发中同样也是很实用。

## ChainMap对象
ChainMap是一个方便的工具类，它使用链的方式将多个dict“链”在一起，从而允许程序可直接获取任意一个dict所包含的key对象的value
<br>
ChainMap相当于把多个dict合并成一个大的dict，但实际上底层并未真正合并这些dict，因此程序无须调用update()方法将多个dict进行合并。
<br>
需要说明的是，由于ChainMap只是将多个dict链在一起，并未真正合并它们，因此在多个dict中完全可能具有重复key，在这种情况下，排在“链”前面的dict中的key具有更高的优先级。
```
from collections import ChainMap

a={'kotlin':90,'Python':86}
b={'Go':93,'Python':92}
c={'Swift':89,'Go':87}

cm=ChainMap(a,b,c)
print(cm)
# 获取Kotlin对应的value
print(cm['kotlin']) # 90
# 获取Python对应的value
print(cm['Python'])	# 86
# 获取Go对应的value
print(cm['Go'])		#93
```

## Counter对象
Counter可以自动统计容器中各元素出现的次数。
