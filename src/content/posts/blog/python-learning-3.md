# 1.list
`list`是Python中内置的一个数据类型，`list`是一种有序合集，可以随时增加和删除其中的元素
```python
letter=['a','b','c']
print(letter) #输出的结果为：['a', 'b', 'c']
```
># 1.1其中变量`letter`是一个`list`,也可以用之前在字符串之间数字符数量的`len()`函数来数list列表中的元素个数
```python
letter=['a','b','c']
print(len(letter)) #输出为3
```
># 1.2可以用索引来指定列表中的某一个元素位置，索引是从0开始计算的，和c语言中的数组是一样的
```python
letter=['a','b','c']
print(letter[0]) #a
print(letter[1]) #b
print(letter[2]) #c
```
># 1.3当索引超出数列的编号时就会弹出`IndexError`的错误，所以弄最后一位的索引时可以用`len(letter)-1`来避免超出范围
```python
letter=['a','b','c']
print(letter[len(letter)-1])
print(letter[3])
```
输出结果为:
```python
c
Traceback (most recent call last):
  File "e:/code/learning.py", line 3, in <module>
    print(letter[3])
          ~~~~~~^^^
IndexError: list index out of range
```
>同时索引除了可以正真1,2,3这样的列，也可以倒着用-1，-2，-3这样来列
```python
letter=['a','b','c'] 
print(letter[-1]) #输出为c
print(letter[-2]) #输出为b
print(letter[-3]) #输出为a
```