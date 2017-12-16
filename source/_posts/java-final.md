---
title: java final关键字效率
categories: java
date: 2017-02-24 17:17:43
tags:
---

# 在方法声明的意义
&emsp;&emsp;在一个方法声明了final的关键字后，编译器会找出你代码中的不变参数并且以最佳的方式优化它，以内联(inline)方式载入，所谓内联方式，是指编译器不用像平常调用函数那样的方式来调用方法，而是直接将方法内的代码通过一定的修改后copy到原代码中(将方法主体直接插入到调用处，而不是进行方法调用)。这样可以让代码执行的更快（因为省略了调用函数的开销）。

http://www.importnew.com/7553.html
http://stackoverflow.com/questions/4279420/does-use-of-final-keyword-in-java-improve-the-performance