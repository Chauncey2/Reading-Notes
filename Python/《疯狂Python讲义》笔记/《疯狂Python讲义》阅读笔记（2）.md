[TOC]
# 列表、元素和字典
## 序列封包和序列解包
+ 序列封包：程序把多个值给一个变量时，Python会自动将多个值封装成元组。
+ 序列解包：程序允许将序列（元组或列表等）直接赋值给多个变量，此时序列的各元素会被依次赋值
给每个变量（要求序列的元素个数和变量个数相等）。

## 字典

+ 字典的常用方法

++ clear():清空字典
++ get():根据key来获取value，如果key不存在字典会引起 KeyError
++ update():方法根据key-value更新以前的值，覆盖以前的值
++ items()
++ keys()
++ values()
++ pop():指定key，获取对应的value
++ popitem():方法用于随机弹出字典中的一个key-value
++ fromkeys():使用给定的key创建字典，这些key对应的value默认都是None

# 流程控制

## 循环使用else

Python的循环都可以使用else代码块，当循环条件为False时，程序会执行else代码块。
```
count i = 0
while count i < 5 :
print (' count l 小子5 ：’， count i)
count 工＋＝ 1
else:
print （’ count_i 大子或等于5: ’, count i)
```
运行结果
```
count i 小于5 : 0
count i 小于5 : 1
count i 小于5: 2
count l 小于5 : 3
count i 小于5 : 4
count l 大于或等于5: 5
```

## 常用工具函数
+ zip():
```
>> a = [ ’ a ’,’b ’, ’ c ’ ]
>> b = (1, 2 , 3]
>> [x for x in zip(a , b)]
[ (’ a ’, 1 ) I ( ’ b ’, 2) , ( ’ c ’, 3) l
```
+ reversed():对序列进行反转,返回一个reversed对象。迭代取值

+ sorted()

