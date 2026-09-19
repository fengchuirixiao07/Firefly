---
title: Python学习笔记-调用函数
published: 2026-08-31
author: fengchuirixiao
description: Python学习笔记-调用函数
tags: [Python]
category: Python
draft: false
---
# 1.调用函数
python内置了许多的函数，之前所运用的像`replace()`,`remove()`这种算内置的类型方法
调用一个函数，需要知道函数的名称和参数,如果只知道函数名，可以调用`help`来看函数的用途或者直接在pyhton的网站上搜索
```python
print(help(abs))
```
输出为：
```python
Help on built-in function abs in module builtins:                                                                                       

abs(x, /)
    Return the absolute value of the argument.

None
```
这样就可以知道`abs`函数的用途
```python
print(abs(20)) #输出为20
print(abs(-20)) #输出为20
```
## 1.1参数数量错误
```python
print(abs(1,2))
```
当输入的参数数量不对时。终端也会给出提示
```python
Traceback (most recent call last):
  File "e:\code\learning.py", line 53, in <module>
    print(abs(1,2))
          ~~~^^^^^
TypeError: abs() takes exactly one argument (2 given)
```
## 1.2参数类型错误
```python
print(abs('a'))
```
参数类型错误是无法被函数所接受的，就会直接反馈输入的参数是错误的类型
```python
Traceback (most recent call last):
  File "e:\code\learning.py", line 53, in <module>
    print(abs('a'))
          ~~~^^^^^
TypeError: bad operand type for abs(): 'str'
```
# 2.数据类型转换
python中有内置数据类型转换的函数
```python
a='12'
print(int(a)) #输出为12
print(str(a)) #输出为12
print(float(a)) #输出为12.0
print(bool(a)) #输出为True
```
但是`int`类型无法识别小数类型的数字
```python
Traceback (most recent call last):
  File "e:\code\learning.py", line 56, in <module>
    print(int(a))
          ~~~^^^
ValueError: invalid literal for int() with base 10: '12.34'
```
>函数名也只是一个指向函数对象的引用，可以对函数名设置别名
```python
a=abs
print(abs(-20)) #输出为20
```
# 3.练习
请利用Python内置的hex()函数把一个整数转换成十六进制表示的字符串：
```python
n1 = 255
n2 = 1000

print(???)
```
---
```python
n1 = 255
n2 = 1000

print(hex(n1))
print(hex(n2))
```
