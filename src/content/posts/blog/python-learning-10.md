---
title: Python学习笔记-函数的参数
published: 2026-09-01
author: fengchuirixiao
description: Python学习笔记-函数的参数
tags: [Python]
category: Python
draft: false
---
# 1.函数的参数
把函数的参数和位置确定下来，函数的接口定义就完成了，如果进行调用的话，直接去接这个函数的接口就可以了，函数的内部结构是不需要去了解的，这个和java中的封装是一个道理
python中定义函数较为简单，应用相对于其他语言来说是更为灵活的的，除了可以设置正常定义的必选参数，还可以设置默认参数，可变参数，关键词参数，使函数定义的接口不但能处理复杂的参数，还能简化调用者的代码
## 1.1位置参数
```python
def power(x):
    return x*x
print(power(2))
```
对于`power(x)`这个函数而言，这个x就是一个位置参数
当我们要调用这个函数时，就只需要填入这个唯一所需要的参数，就可以了
```python
print(power(2))# 输出为4
```
如果是不仅仅要满足平方的运算，是三次方，四次方的运算，不可能定义多个类似于`power(x)`的函数，就可以将`power(x)`修改为`power(x,n)`
```python
def power(x,n):
    s=1
    while n>0:
        n=n-1
        s=s*x
    return s
print(power(2,3))# 输出为8
```
新写的`power(x,n)`，其中x，n都是位置参数，会根据输入的值的顺序依次赋值给x，n  
## 1.2默认参数
之前在前面设置了两个power的函数，就会因为后面新添加了一个参数，导致前面定义的那个函数失效
```pyhton
Traceback (most recent call last):
  File "e:\code\learning.py", line 143, in <module>
    print(power(2))
          ~~~~~^^^
TypeError: power() missing 1 required positional argument: 'n'  
```
报错的信息就是丢失了一个应该输入的参数
这样就可以设置默认参数来解决，设置默认参数n=2，如果后面的那个次方数要大于n=2那就得对n重新赋值，明确把值传入给n
```python
def power(x,n=2):
    s=1
    while n>0:
        n=n-1
        s=s*x
    return s
print(power(2))# 输出为4
print(power(2,3))# 输出为8
```
>必选参数在前，默认参数在后,如果把这个默认参数放在前面，就会与输入进去的参数产生冲突，不知道是对这个默认参数进行新的传入，还是对后面还没有进行赋值的参数进行传入
使用默认参数的好处就是可以大幅度降低函数的调用的难度
```python
def name_inf(name,gender,age=18,city='Chengdu'):
    return f'name:{name}\ngender:{gender}\nage:{age}\ncity:{city}'

print(name_inf('xiaoming','A'))
print(name_inf('daming','B',19,'Beijing'))
```
输出为：
```python
name:xiaoming
gender:A
age:18
city:Chengdu
name:daming
gender:B
age:19
city:Beijing
```
只需要默认参数不符合的，才需要提供额外信息
默认参数降低了函数的调用难度，只用一个函数就可以完成复杂的调用
填入参数可以按照参数的顺序去调用，也可以不按照，这样就得把参数名给加上
如果默认参数指向的是一个可变对象，那么默认参数就是可变的，像列表，字典就是可变对象，可以进行积累，像字符串，数字还有None就是不可变对象，不会造成积累
```python
def add_end(L=[]):
    L.append('End')
    return L
print(add_end())# 输出为['End']
print(add_end())# 输出为['End', 'End']
print(add_end())# 输出为['End', 'End', 'End']
```
其中这个L为数组类型，就是可变的，会造成积累，每次调用该函数，L中的内容就变了
```python
def add_end(L=None):
    if L is None:
        L=[]
    L.append('End')
    return L
print(add_end())# 输出为['End']
print(add_end())# 输出为['End']
print(add_end())# 输出为['End']
```
这样就可以通过不变的对象来实现默认参数，这样就不会产生内部数据的更改，调用多少次也不会产生数据的污染
## 1.3可变参数
当参数个数不确定时，可以让参数通过list或者tuple的形式传入进来
```python
def calc(numbers):
    sum=0
    for n in numbers:
        sum=sum+n*n
    return sum
print(calc((1,2,3)))# 输出为14
print(calc([1,2,3]))# 输出为14
```
如果要简化输入的话，就是在参数名前面加`*`号，就是以元组的形式传入给参数的
```python
def calc(*numbers):
    sum=0
    for n in numbers:
        sum=sum+n*n
    return sum
print(calc(1,2,3))# 输出为14
print(calc())# 输出为0
```
可变参数可以实现传入多个参数或者0个参数，然后组装成tuple传入进去
如果已经写好了一个list或者tuple，要调用1个可变参数可以通过其中元素所在的位置填进去，也可以直接在名字前面加*号，之所以为什么是1个可变参数，是因为python中这个函数把这个list或者tuple的值给打包进了一个叫args的容器里
```python
def calc(*numbers):
    sum=0
    for n in numbers:
        sum=sum+n*n
    return sum
num1=(1,2,3)
num2=[1,2,3]
num3=[]
print(calc(num1[0],num1[1],num1[2]))# 输出为14
print(calc(num2[0],num2[1],num2[2]))# 输出为14
print(calc(*num1))# 输出为14
print(calc(*num2))# 输出为14
print(calc(*num3))# 输出为0
```
## 1.4关键字参数
可变参数，可以写入0个或者多个参数，以tuple的形式传入进去，关键词参数就是也可以写入0个或者多个参数，但是就是以dict形式传入进去
```python
def person(name,age,**other):
    return f'name:{name},age:{age},other:{other}'
print(person('xiaoming',18,school='cuit',gender='A'))# 输出为name:xiaoming,age:18,other:{'school': 'cuit', 'gender': 'A'}
print(person('xiaoming',18,school='cuit'))# 输出为name:xiaoming,age:18,other:{'school': 'cuit'}
print(person('xiaoming',18))# 输出为name:xiaoming,age:18,other:{}
```
关键词的作用就是能够保证能够收到name和age这两个参数，如果调用者提供更多的参数也能够收到
如果是多个参数先组装好了一个dict，就可以直接直接用关键词参数来实现必须要显示的要求
```python
def person(name,age,**other):
    return f'name:{name},age:{age},other:{other}'
p1={'city':'chengdu','gender':'A'}
print(person('xiaoming',18,city=p1['city'],gender=p1['gender']))# 输出为name:xiaoming,age:18,other:{'city': 'chengdu', 'gender': 'A'}
print(person('xiaoming',18,**p1))# 输出为name:xiaoming,age:18,other:{'city': 'chengdu', 'gender': 'A'}
```
把额外非关键词参数的参数放在外面，以dict的形式组装起来，再传入进去，简化的形式就是直接在前面加`**`
相当于就是把`**p1`传入给了`**other`,other相当于获得的是一个p1的拷贝，p1的改变是不会影响到函数外other内部的信息的
## 1.5命名关键字参数
对于关键字参数，任何参数可以不受限制的调入进去的，就得专门去检查除了前面专门提及的关键字参数，就得专门去查找
```python
def person(name,age,**other):
    if 'school' in other:
        pass
    if 'gender' in other:
        pass
    return f'name:{name},age:{age},other:{other}'
print(person('xiaoming',18,school='cuit',gender='A'))# 输出为name:xiaoming,age:18,other:{'school': 'cuit', 'gender': 'A'}
```
即使是用了`pass`，但调用者仍可以传入不受限制的关键字
如果要限制关键字参数的输入，就可以在关键字后面加个`*`，就只会接受后面提及的关键字，后面的被视为命名关键字参数
```python
def person(name,age,*,school,gender):
    return f'name:{name},age:{age},school:{school},gender:{gender}'
print(person('xiaoming',18,school='cuit',gender='A',sport='football'))
```
这样就属于多传入了参数的情况，就会直接报错
```python
Traceback (most recent call last):
  File "e:/code/learning.py", line 218, in <module>
    print(person('xiaoming',18,school='cuit',gender='A',sport='football'))
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: person() got an unexpected keyword argument 'sport'
```
如果把sport这个关键字去掉，就会和之前这样正常输出
```python
def person(name,age,*,school,gender):
    return f'name:{name},age:{age},school:{school},gender:{gender}'
print(person('xiaoming',18,school='cuit',gender='A'))# 输出为name:xiaoming,age:18,school:cuit,gender:A
```
如果前面有一个可变参数，就不用再多打一个`*`
```python
def person(name,age,*num,school,gender):
    return f'name:{name},age:{age},num:{num} school:{school},gender:{gender}'
num=[1,2,3]
print(person('xiaoming',18,num,school='cuit',gender='A'))
```
其输出结果为：
```python
name:xiaoming,age:18,num:([1, 2, 3],) school:cuit,gender:A
```
其中输出结果num中会多一个逗号，实则显示出其可变参数是以元组的形式传入进去的
```python
def person(name,age,*args,school,gender):
    return f'name:{name},age:{age},args:{args} school:{school},gender:{gender}'

print(person('xiaoming',18,'chengdu',1,school='cuit',gender='A'))# 输出为name:xiaoming,age:18,args:('chengdu', 1) school:cuit,gender:A
```
也可以直接把要写的元素写在person中所对应的位置，也就是用元组的方式返回过去，进行输出
命名关键词参数也可以有缺省值，可以简化
```python
def person(name,age,*,school='cuit',gender):
    return f'name:{name},age:{age},school:{school},gender:{gender}'
print(person('xiaoming',18,gender='A'))# 输出为name:xiaoming,age:18,school:cuit,gender:A
```
这样就不可以传入school的值
## 1.6参数组合
之前的参数有位置参数，默认参数，可变参数，关键字参数和命名关键字参数，顺序是位置参数，默认参数，可变参数，关键字参数，命名关键字参数
```python
def f1(a,b,c=0,*args,**kw):
    return f'a:{a},b:{b},c:{c},args:{args},kw:{kw}'
def f2(a,b,c=0,*,d,**kw):
    return f'a:{a},b:{b},c:{c},d:{d},kw:{kw}'
print(f1(1,2))# 输出为a:1,b:2,c:0,args:(),kw:{}
print(f1(1,2,C=3))# 输出为a:1,b:2,c:0,args:(),kw:{'C': 3}
print(f1(1,2,'a','b'))# 输出为a:1,b:2,c:a,args:('b',),kw:{}
print(f1(1,2,'a',x=1))# 输出为a:1,b:2,c:a,args:(),kw:{'x': 1}
print(f2(1,2,d=9,exp=None))# 输出为a:1,b:2,c:0,d:9,kw:{'exp': None}
args=(1,2,3,4)
kw={'d':0,'x':1}
print(f1(*args,**kw))# 输出为a:1,b:2,c:3,args:(4,),kw:{'d': 0, 'x': 1}
args=(1,2,3)
print(f2(*args,**kw))# 输出为a:1,b:2,c:3,d:0,kw:{'x': 1}
```
python调用时会按照这输入的顺序回传给参数，同时用一个tuple或者list也可以回传，但是要注意*和**所对应的类型，所以对于任何类型的函数都可以用func(*args,**kw)这种形式，任何类型的数据都可以回传进去
# 2.练习
以下函数允许计算两个数的乘积，请稍加改造，变成可接收一个或多个数并计算乘积：
```python
def mul(x, y):
    return x * y

# 测试
print('mul(5) =', mul(5))
print('mul(5, 6) =', mul(5, 6))
print('mul(5, 6, 7) =', mul(5, 6, 7))
print('mul(5, 6, 7, 9) =', mul(5, 6, 7, 9))
if mul(5) != 5:
    print('mul(5)测试失败!')
elif mul(5, 6) != 30:
    print('mul(5, 6)测试失败!')
elif mul(5, 6, 7) != 210:
    print('mul(5, 6, 7)测试失败!')
elif mul(5, 6, 7, 9) != 1890:
    print('mul(5, 6, 7, 9)测试失败!')
else:
    try:
        mul()
        print('mul()测试失败!')
    except TypeError:
        print('测试成功!')
```
---
```python
def mul(*number):
    if not number:
        raise f'mul()测试失败!'
    else:
        sum=1
        for n in number:
                sum=sum*n
        return sum
```