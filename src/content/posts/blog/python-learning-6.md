---
title: Python学习笔记-循环
published: 2026-08-20
author: fengchuirixiao
description: Python学习笔记-循环
tags: [Python]
category: Python
draft: false
---
# 1.循环
循环的意义与逻辑和之前学c和java的逻辑和道理是一样的,可以替代成百上千次重复的运算，打印输出具有规律性的图形，循环输出等
但是其表现形式和c和java两者都不相同，其实与c和java挺类似的
c和java的循环:
```c
for(int i=0;i<100;i++){

}
```
python中循环的格式：
```python
for i in range (0,101){
    print(i) #以此来实现1到100数字的打印输出
}
```
python的循环有两种，其根本逻辑也和c和java是类似的
## 1.1for循环
首先是在`list`或者`tuple`里面对其进行循环遍历输出
```python
fruits=('香蕉','苹果','梨')
for fruit in fruits:
    print(fruit)
fruits=['香蕉','苹果','梨']
for fruit in fruits:
    print(fruit)
```
二者的运行效果是一样的，就是里面中的每个元素每个占一行的输出出来
```python
香蕉
苹果
梨
```
第二种就是这种循环加法
```python
sum=0
for x in [1,2,3,4,5,6,7,8,9,10]:
    sum=sum+x
print(sum) 
```
但是如果想写这样的式子从1加到100就特别麻烦了，就得依靠`range()`这个函数，`range(1,5)`表示的是从1开始到小于5的整数，`range(5)`的话表示的就是0-4这5个数字，这样就比上面简单多了，用`range()`来改写上面这个代码
```python
print(list(range(5))) #输出为[0, 1, 2, 3, 4]
```
```python
sum=0
for x in range(11):
    sum=sum+x
print(sum)
```
这样输出的效果是一样的，结果都是55，但是就更加简略，方便
## 1.2while循环
`while`循环的逻辑与c和java也是一样的，只要条件一直满足，就会一直无限循环下去，条件不满足时退出循环
以计算100以内所有的奇数为例：
```python
sum=0
n=99
while n>0:
    sum=sum+n
    n=n-2
print(sum) #输出的结果为2500
```
其中    n不断自减，减到-1时，不再满足`while`的条件，则退出循环
# 1.3break
在循环中，`break`可以提前结束循环
下面以假设遍历1-10这10个数字，当数字为5时退出循环并打印
```python
for i in range(1,11):
    if i==5:
        break
    print(i)
print(f'当前的数字是{i}')
```
输出的结果为：
```python
1
2
3
4
当前的数字是5
```
当`i=5`时则跳出循环，直接去打印当前数字是什么
# 1.4continue
在循环语句中，可以通过`continue`来结束这次循环，开始新一轮的循环
以打印1-10的奇数之和为例：
```python
n=0
sum=0
while n<10:
    n=n+1
    if n%2==0:
        continue
    sum=sum+n
print(sum) #输出为25
```
通过`continue`就可以直接得到为奇数的n
# 2.练习
请利用循环依次对list中的每个名字打印出Hello, xxx!：
```python
L = ['Bart', 'Lisa', 'Adam']
```
---
```python
L = ['Bart', 'Lisa', 'Adam']
for name in L:
    print(f'Hello, {name}!')
```