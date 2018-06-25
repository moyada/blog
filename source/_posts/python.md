---
title: python学习笔记
categories: python
date: 2017-04-04 11:27:02
tags:
---

# 循环
enumerate 内置遍历函数
items()
zip(,)
for index, item in enumerate(sequence):
    process(index, item)

iter(array) 创建迭代器对象
next(iter) 输出迭代器的下一个元素
yield 的函数被称为生成器
生成器是一个返回迭代器的函数，产生连续的n值，用于迭代操作。
randomrange 

```
#!/usr/bin/python3

import sys

def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成

while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
```

# 函数
参数传递中，strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象

## 匿名函数
```
lambda [arg1 [,arg2,.....argn]]:expression
```

## global 和 nonlocal关键字
当内部作用域想修改外部作用域的变量时，
修改全局变量要用到global关键字，
修改嵌套作用域（enclosing 作用域，外层非全局作用域）中的变量则需要 nonlocal 关键字。

# lambda函数表达式
语法：lambda 参数列表:返回值


