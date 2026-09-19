---
title: Python学习笔记-递归函数
published: 2026-09-02
author: fengchuirixiao
description: Python学习笔记-递归函数
tags: [Python]
category: Python
draft: false
---
# 1.递归函数
函数内部可以调用其他函数，如果调用的函数是自己本身的话，那就是递归函数
如果要表示一串数字连续相乘，就是n!这个用的就是递归函数
如n!=1*2*3*4*···*(n-1)*n，这样就可以设置一个函数名为fact(n)来表示n!
```python
def fact(n):
    if n==1:
        return 1
    else:
        return fact(n-1)*n# 这样就可以用递归来表示fact(n-1)*n
print(fact(5))# 输出为120
```
fact(5)其整个的计算过程就可以表示为：
>fact(5)=fact(4)*5
>fact(5)=fact(3)*(4*5)
>fact(5)=fact(2)*(3*4*5)
>fact(5)=fact(1)*(2*3*4*5)
>fact(5)=120
递归的优点就是逻辑清晰，代码较为简洁，任何类型的递归函数都可以用循环来写，但是用循环来写就更为复杂与繁琐