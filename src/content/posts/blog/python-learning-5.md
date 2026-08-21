---
title: Python学习笔记-模式匹配
published: 2026-08-17
author: fengchuirixiao
description: Python学习笔记-模式匹配
tags: [Python]
category: Python
draft: false
---
# 1.模式匹配
如果考虑的条件划分较细，这就会用很多的`if···elif···else`，这样的结果造成的就是代码的可读性较差
针对某个变量要对应若干种情况，之前用的条件判断就可能不再适用，就可以去使用`match`语句
```python
score='A'
if score=='A':
    print('score=A') #输出为score=A
elif score=='B':
    print('score=B')
elif score=='C':
    print('score=C')
else:
    print('invalid score')
```
>这个就是用`if`语句写了一个判断学生成绩的代码，学生的成绩只能是`A，B，C`，下面是用`match`语句改写
```python
score='E'
match score:
    case 'A':
        print('score=A')
    case 'B':
        print('score=B')
    case 'C':
        print('score=C')
    case _:
        print('score=?') #输出为score=?
```
`match`语句的使用是通过`case`来用条件进行匹配的，并且可以在最后并且这个只能加在最后加个`case _`来表示任意值，就是与以上条件不符，所会输出的结果
相比`match`语句和`if`语句解构这才是`match`比`if`好得多的地方：不仅仅是值的比较，更是结构的匹配与提取。这是`if`语句很难优雅实现的能力,`match`语句的可读性更强，穷尽性与安全性也更强，`if`只要少了一个`elif`的条件判断就有可能会产生bug，而`match`相当于是在一个字典里面去找对应的值，配上了`case _`相当于也有一个兜底的处理，代码会更加的健壮
# 2.复杂匹配
`match`语句除了可以匹配单个值，还可以匹配多个值，还有范围条件
```python
age=input('age:')
x=int(age)
match x:
    case x if x<10:
        print(f'Your age is {x} and less than 10')
    case 10:
        print('You are 10 years old')
    case 11|12|13|14|15|16|17:
        print(f'Your age is {x} but not an adult')
    case x if x>=18:
        print(f'Your age is {x} and you are an adult')
```
和`c`中部分语法还是比较相似的，这个程序中第一个`case`加入`if x<10`加入了范围，第二个`case`是一个仅匹配单值，第三个`case 11|12|13|14|15|16|17`,`|`和`c`里面一样是表示或的意思
# 3.匹配列表
`match`语句还可以匹配列表，以下面这段代码为例：
```python
args = ['gcc', 'hello.c', 'world.c']
match args:
    case ['gcc']:
        print('gcc: missing source file(s).')
    case ['gcc', file1, *files]:
        print('gcc compile: ' + file1 + ', ' + ', '.join(files)) #输出为gcc compile: hello.c, world.c
    case ['clean']:
        print('clean')
    case _:
        print('invalid command.')

```
第一个`case`是列表中有且只有一个元素名字叫`gcc`，这个才匹配

第二个`case`中，指的是列表中第0个元素叫`gcc`，然后第1个元素取出来并赋值给`file1`，然后后面的元素通过`*files`被全部收集起来放到一个列表里面

第三个`case`中列表中有且仅有`clean`才匹配

第四个`case`指的以上三种情况不匹配就运行第四种

其中第二个`case`所对应的`print`中`', '.join(files)`其中`join()`这个函数起到了一个胶水的作用，假设`files`这个新的列表中的内容是'香蕉'，'苹果'，'梨'
```python
files=('香蕉','苹果','梨')
print('-'.join(files))
```
则其输出得到结果中每个元素就会以`-`来连接，其输出的结果就是`香蕉-苹果-梨`
