---
title: Python学习笔记-2
published: 2026-08-07
author: fengchuirixiao
tags: [Python]
category: Python
draft: false
---
# 1.Python的字符串
在Python中，字符是用Unicode来进行编码的，这意味着字符串里面可以显示多种语言
对于单个字符编码，提供了`ord()`函数可以获取字符的整数表示，`chr()`函数可以可以把编码转化为对应的字符
```python
a=ord('A') #输出的是65
b=chr(65) #输出的是A
```
Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes
Python对bytes类型的数据用带b前缀的单引号或双引号表示：
```python
x=b'abc'
```
区分abc和b'abc'，二者内容上虽然一样，但是前者是字符，后者是字节
>1.以Unicode表示的str通过encode()方法可以编码为指定的bytes，如：
```python
a='cuit'.encode('ascii')
print(a) #输出结果为b'cuit'
b='成信大'.encode('utf-8')
print(b) #输出结果为b'\xe6\x88\x90\xe4\xbf\xa1\xe5\xa4\xa7'
```
纯英文的str可以用ASCII编码为bytes，内容是一样的，含有中文的str可以用UTF-8编码为bytes。含有中文的str无法用ASCII编码，因为中文编码的范围超过了ASCII编码的范围，Python会报错。
在bytes中，无法显示为ASCII字符的字节，用`\x##`显示。
>2.反过来，如果在网络上或者磁盘上读取了字节流，其类型就是为bytes，可以用decode()的方法来转化为str
```python
b=b'\xe6\x88\x90\xe4\xbf\xa1\xe5\xa4\xa7'.decode('utf-8')
print(b) #输出结果为成信大
```
当bytes中有一小部分无效字节，可以传入errors='ignore'忽略错误的字节：
```python
b=b'\xe6\x88\x90\xe4\xbf\xa1\xe5\xa4\xa7\x00'.decode('utf-8')
print(b)
```
```python
b=b'\xe6\x88\x90\xe4\xbf\xa1\xe5\xa4\xa7\xff'.decode('utf-8')
print(b) 
```
输出为：
```python
Traceback (most recent call last):
  File "e:\code\learning.py", line 2, in <module>
    b.decode('utf-8')
    ~~~~~~~~^^^^^^^^^
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
```
```python
b=b'\xe6\x88\x90\xe4\xbf\xa1\xe5\xa4\xa7\xff'.decode('utf-8',errors='ignore')
print(b) #输出为成信大
```
我引入了最为常见的无意义,在utf-8中不允许的值`\xff`,通过引入`errors='ignore'`就可以来忽略
>计算str有多少字符，可以用len()函数
```python
a=len('cuit')
b=len('成信大')
print(a) #输出为4
print(b) #输出为3
```
len()计算的是字符数，换成bytes就是算的字节数
```python
a='cuit'.encode('ascii')
b='成信大'.encode('utf-8')
a=len(a) #b'cuit'
b=len(b) #b'\xe6\x88\x90\xe4\xbf\xa1\xe5\xa4\xa7'
print(a) #输出为4
print(b) #输出为9
```
相当于在`utf-8`编码中，一个英文字符占用一个字节，一个中文字符占用三个字节
为了避免str和bytes两者转化之间出现乱码，所以坚持使用`utf-8`编码来对两者进行转换
所以在有些正规的代码前面会有以下的抬头，会有以下两行
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```
>第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；
>第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，源代码中写的中文输出可能会有乱码。
# 2.格式化
占位符也和c有大部分是相似的
>`%`:用来格式化字符串
>`%s`:表示用字符串替换
>`%d`:表示用整数替换
>`%f`:表示用浮点数替换
>`%x`:表示用十六进制数替换
如：
```python
print('%s' %'cuit') #输出为cuit
print('%d,%s' %(12345,'cuit')) #输出为12345，cuit
```
打印输出的格式和c略有差异，就是%号之前，c是用&取地址，在前面打逗号，Python直接用%后面来写替代的内容就可以了，比用打逗号
>如果你不确定去替代的内容是什么类型的，直接用`%s`来替代也是可以的
```python
print('%s,%s' %(12345,'cuit')) #输出为12345,cuit
```
我12345用的是`%s`来进行替代，一样可以正常输出
>如果是想打印`%`，则可以用`%%`来替代
```python
print('%d %%' %100) #输出为100 %
```
# 3.format()
另外一种格式化字符串的方法就是`format`，和java中的`String.format()`非常的像,传入的参数依次来替代字符串中的占位符{0}，{1}，{2}...
>1.`format`相较于`%`的好处就是可以不用考虑数据的类型，直接替换带入
```python
print('{} {}'.format(123,'cuit')) #输出为123 cuit
```
>2.同时`format`可以编号避免重复的去输入要去替代的内容
```python
print('{0} {1} {0}'.format(123,'cuit')) #输出为123 cuit 123
```
>3.还可以不用数字进行编号，也可以用编码来命名
```python
print('{number} {school} {number}'.format(number='123',school='cuit')) #输出为123 cuit 123
```
>4.可以在输出中进行限定，格式化语法更直观，比如补0、对齐、保留小数，不用记紧凑的符号
```python
print("{:04d}".format(12)) # 补0到4位 → 0012
print("{:*^10}".format("cuit")) # 居中对齐，宽度10，用*填充 → ***cuit***
print("{:.2%}".format(0.75)) # 百分比格式 → 75.00%
print('{0}是前{1:.3f}%'.format('小明',10)) #保留小数个数
#同时提醒，只要{}里面编了号了，所有空就都得标号
```
综合下来就是`format`要比`%`更加灵活与方便
# 4.f-string
格式化字符串的方法是在前面加使用f开头的字符串，被称为f-string,与先前的format()相比更加现代，更加方便，存在{xxx}则可以直接代入替换
```python
school='cuit'
name='xiaoming'
number=10
print(f'{name} is in {school}') #输出为xiaoming is in cuit
print(f'{name} is in the top {number:.2f}%') #输出为xiaoming is in the top 10.00%
```
{school}被school替换
{name}被name替换
{number}被number替换，并且:后面的.2f指定了格式化参数（即保留两位小数）

# 5.练习
小明的成绩从去年的72分提升到了今年的85分，请计算小明成绩提升的百分点，并用字符串格式化显示出'xx.x%'，只保留小数点后1位：
```python
s1 = 72
s2 = 85
r = ???
print('???')
```
```python
s1 = 72
s2 = 85
r=((s2-s1)/s1)*100
name='小明'
print(f'{name}的成绩提升了{r:.1f}%')
```
输出的结果为
```python
小明的成绩提升了18.1%
```