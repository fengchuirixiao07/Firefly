---
title: Python学习笔记-使用dict和set
published: 2026-08-21
author: fengchuirixiao
description: Python学习笔记-使用dict和set
image: ../images/713fd0d5ff97744729c06e80b8434c17.jpg
tags: [Python]
category: Python
draft: false
---
# 1.使用dict和set
## 1.1dict
python里面内置了字典的功能，不是`linux`中那种查功能，`dict`，全称为`dictionary`，在其他语言中被称为`map`，使用键-值（key-value）存储，具有极快的查找速度。
假如我要查下列学校的建校年份，用`list`来实现就需要两个`list`，第一个`list`里面装学校，第二个`list`里面装对应的建校年份
```python
school=['cuit','cdut','xhu','swpu']
years=[1951,1956,1960,1958]
```
如果先从学校的`list`里面找学校，再从年份的`list`里面找年份，那么`list`越长，搜寻时间也就越长
如果用`dict`来弄，逻辑就变成一个学校对应的建校年份表，就像查字典一样，根据名字直接找到所对应的值，就不会限制于这个表有多大，搜寻速度也不会慢
用dict来实现上面的那个例子:
```python
s_y={'cuit':1951,'cdut':1956,'xhu':1960,'swpu':1958}
print(s_y['cuit']) #输出为1951
```
就是用`dict`来实现的，使用键值来存储，代码中`key`指的就是这些学校名，`value`指的就是这些建校的年份，通过对应的`key`就会找到对应的`value`，是通过目录直接去找对应的页码，而不是去一页页的翻书去找对应的页码
把数据放入`dict`，除了之前初始化的方法，也可以先直接通过`key`放入
```python
s_y={'cuit':1951,'cdut':1956,'xhu':1960,'swpu':1958}
s_y['cdu']=1978
print(s_y['cdu']) #输出为1978
```
一个`key`只能对应一个`value`，如果后面对之前的一个`key`给了新的`value`的话，`key`就只会保留现在最新的`value`
```python
names={'Mike':18,'Jack':19}
print(names['Mike']) #输出为18
names['Mike']=19
print(names['Mike']) #输出为19
```
如果`key`不存在的话就会直接报错:
```python
s_y={'cuit':1951,'cdut':1956,'xhu':1960,'swpu':1958}
print(s_y['pku'])
```
报错的结果为：
```python
Traceback (most recent call last):
  File "e:\code\learning.py", line 12, in <module>
    print(s_y['pku'])
          ~~~^^^^^^^
KeyError: 'pku'
```
为了避免因为`key`不存在而造成不必要的报错，第一种方法是通过`in`来判断`key`是否存在
```python
s_y={'cuit':1951,'cdut':1956,'xhu':1960,'swpu':1958}
print('pku' in s_y) #输出为False
```
第二种是通过`get()`的方法来判断，`key`如果不存在，就会返回`None`，也可以自己设置返回的`value`，但是前面会有None
```python
s_y={'cuit':1951,'cdut':1956,'xhu':1960,'swpu':1958}
print(s_y.get('pku'),'invalid') #其输出结果为None invalid
```
如果没有`'invalid'`，其输出结果为`None`
如果删除一个`key`，就用`pop()`就可以删除，这个和java里面的栈弹出是类似的，只不过是弹出的是栈顶
```python
s_y={'cuit':1951,'cdut':1956,'xhu':1960,'swpu':1958}
s_y.pop('xhu')
print(s_y) #输出为{'cuit': 1951, 'cdut': 1956, 'swpu': 1958}
```
`dict`内部存放的顺序和放入key的顺序没有任何关联
>`dict`和`list`相比：
>>`dict`的特点是：
>>>1.不会因为`key`的数量增多，而增加其对应`value`的寻找时间
2.需要占用很多的内存，空间占用相对较大

>>`list`的特点是：
>>>1.随着`list`中元素的增加，去`list`中找对应的元素的时间会相对应的增长
2.需要内存较小，不浪费存储空间

所以`dict`的用法可以看作为一种空间换时间的做法
`dict`的`key`必须为不可变对象，因为`dict`的根据`key`计算`value`的存储位置，如果相同的`key`对应的不同`value`，那`dict`内部的排序就乱了，就有点类似于一把钥匙配一把锁的这个意思，这个`dict`通过`key`来计算位置的算法就叫hash(哈希)算法,为了确保hash算法的准确性，作为对象的`key`就不能变，在python中，除了`list`都可以变，这是因为`list`里面的对象可以变所造成的
```python
school=['cuit','cdut','xhu','swpu']
d={school}
d[school]=1
print(d[school])
```
报错为：
```python
Traceback (most recent call last):
  File "e:\code\learning.py", line 15, in <module>
    d={school}
      ^^^^^^^^
TypeError: unhashable type: 'list'
```
## 1.2set
`set`和`dict`也比较类似，`set`中存储的也是`key`，并且也是不可变的
创建一个`set`:
```python
schools={'cuit','cdut','xhu','swpu'}
print(schools) #输出为{'cuit', 'xhu', 'cdut', 'swpu'}
```
或者作为集合进行输入：
```python
schools=set(['cuit','cdut','xhu','swpu'])
print(schools) #输出为{'xhu', 'swpu', 'cuit', 'cdut'}
```
输入进去的是这个`list`，输出来的是带`{}`，`set`只是显示里面有哪些`key`
>`set()`能自动过滤掉重复的元素：
```python
schools={'cuit','cuit','cuit','cdut','xhu','swpu'}
print(schools) #输出为{'xhu', 'swpu', 'cdut', 'cuit'}
```
>用`add()`,`remove()`的方法可以往`set`里面添加或者删除`key`：
```python
schools={'cuit','cdut','xhu','swpu'}
schools.add('cdu')
schools.remove('cdut')
print(schools) #输出为{'xhu', 'swpu', 'cdu', 'cuit'}
```
由此可见之前显示的并不是`set`内的有序排序，而是自己输入进去`key`的顺序，但是用了`add`或者`remove`两种方法过后的输出则是排序后的
>`set`还可以实现集合上的并集和补集
```python
schools1={'cuit','cdut','xhu','swpu'}
schools2={'cuit','swjtu','sau'}
print(schools1&schools2) #交集输出为{'cuit'}
print(schools1|schools2) #并集输出为{'xhu', 'swpu', 'cuit', 'cdut', 'sau', 'swjtu'}
```
>`set`和`dict`大部分是相似的，他们同样都不能放入可变的对象，`dict`就是不能存对应的`value`值
# 2.不可变对象
如`str`类型为不可变对象，`list`类型为可变对象
>对于可变对象，如`list`，对`list`操作，里面的元素是会发生变化的
```python
a=['b','a','c']
a.sort()
print(a) #输出为['a', 'b', 'c']
```
>对于不可变对象，如`str`，对其进行`replace`的操作，返回的是一个新的字符串，而不是原本的字符串
```python
a='abc'
print(a.replace('a','A')) #输出为Abc
print(a) #输出为abc 
```
而那个对其进行替代的字符串虽然作为新的一串字符进行了打印输出，但是没有进行存储
如果用另外的一个变量被赋值这个被替代的字符串，就会更加直观的发现这是一个新的字符串，而不是原来的字符串
```python
a='abc'
b=a.replace('a','A')
print(a) #输出为abc
print(b) #输出为Abc
```
可见，不可变对象如`str`，是没有进行改变的