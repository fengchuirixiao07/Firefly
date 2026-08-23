---
title: Python学习笔记-条件判断
published: 2026-08-17
author: fengchuirixiao
description: Python学习笔记-条件判断
images: ../images/3d15eb3d008daeb6d0f6b5d095280837.jpg
tags: [Python]
category: Python
draft: false
---
# 1.条件判断
## 1.1`if`与`else`
在程序中通过条件判断，来进行选择，用的是`if`来进行条件判断，如果`if`判断后为`true`，则整个程序就会运行，如果不是，则什么都不做，和`c`与`java`大体逻辑都是相同的,但是其表达形式与`c`和`java`不同，其不需要打圆括号，要的是打冒号
```python
age=20
if age>=18:
    print('He is an adult') #输出为He is an adult
```
>if与else配套使用，当if判断为false时，则会去执行else语句
```python
age=10
if age>=18:
    print('He is an adult')
else:
    print('He is not an adult') #输出为He is not an adult
```
>要做更为细致的判断则可以用`elif`，这个要与`c`中的`else if`区分开，不要混用
```python
age=15
if age>=18:
    print('He is an adult')
elif age>=13:
    print('He is a teenager')
else:
    print('He is a child') #输出为He is a teenager
``` 
>`if`语句执行的特点是由上到下，只要上面有一个`True`过后，就不会运行下面的`elif`和`else`
>`if`的条件判断还可以简写，只有`x`是非零数值，非空字符串，非空`list`等，就会判断为`true`，否则就是为`false`
```python
if x:
    print('True')
```
## 1.2`input`的用法
>`input`初次认为可以填进去，然后返回不同类型的内容，实际上`input`只会返回`str`类型的内容,如果是其他类型的就会报错
```python
birth=input('birth:')
if birth<2007:
    print("you are older than me")
else:
    print("You are younger than me")
```
输出的结果为：
```python
Traceback (most recent call last):
  File "e:\code\learning.py", line 2, in <module>
    if birth<2007:
       ^^^^^^^^^^
TypeError: '<' not supported between instances of 'str' and 'int'
```
正确的输入的话是应该对其进行类型的转换,以这个例子为例，只要对其`str`类型转化为int类型就可以了
```python
s=input('birth:')
birth=int(s)
if birth<2007:
    print("You are older than me")
else:
    print("You are younger than me")
```
如果输入`abc`进去，第一行代码是可以通过的，你无论输入什么返回的都是`str`类型的数据，但是在第二行`int`这里卡住了，`int`无法将`abc`这个字符转化为数字，所以就报错
```python
birth:abc
Traceback (most recent call last):
  File "e:\code\learning.py", line 2, in <module>
    birth=int(s)
ValueError: invalid literal for int() with base 10: 'abc'
```
# 2.练习
小明身高1.75，体重80.5kg。请根据BMI公式（体重除以身高的平方）帮小明计算他的BMI指数，并根据BMI指数：

低于18.5：过轻
18.5-25：正常
25-28：过重
28-32：肥胖
高于32：严重肥胖
用if-elif判断并打印结果：
```python
height = 1.75
weight = 80.5

bmi = ???

if ???:
    pass

```
---
```
height = input('height:')
weight = input('weight:')
height=float(height)
weight=float(weight)
bmi=weight/(height*height)
if bmi>32:
    print('严重肥胖')
elif 28<bmi<=32:
    print('肥胖')
elif 25<bmi<=28:
    print('过重')
elif 18.5<bmi<=25:
    print('正常')
else:
    print('过轻')
```
输出结果为：过重