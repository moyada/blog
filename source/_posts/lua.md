---
title: Lua学习笔记
categories: Lua
date: 2017-03-28 15:51:07
tags:
---

# 注释

&emsp;两个减号是单行注释: 
```
-- 注释内容
```
&emsp;多行注释: 
```
--[[ 
注释内容
注释内容
--]]
```

# 数据类型
Lua是动态类型语言，变量不要类型定义,只需要为变量赋值。值可以存储在变量中，作为参数传递或结果返回。

## nil
这个最简单，只有值nil属于该类，表示一个无效值（在条件表达式中相当于false）。

## boolean
包含两个值：false和true。

## number
表示双精度类型的实浮点数

## string
字符串由一对双引号或单引号或[[和]]间的一串字符来表示

## function
由 C 或 Lua 编写的函数

## userdata
表示任意存储在变量中的C数据结构

## thread
表示执行的独立线路，用于执行协同程序

## table
Lua 中的表（table）其实是一个"关联数组"（associative arrays），数组的索引可以是数字或者是字符串。在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表。
对 table 的索引使用方括号 []。Lua 也提供了 . 操作。
```
t[i]
t.i                 -- 当索引为字符串类型时的一种简化写法
gettable_event(t,i) -- 采用索引访问本质上是一个类似这样的函数调用

site["key"] = "www.w3cschool.cc"
print(site["key"])
print(site.key)
```

# 变量
Lua 变量有三种类型：全局变量、局部变量、表中的域。Lua 中的变量全是全局变量，那怕是语句块或是函数里，除非用 local 显式声明为局部变量。

# 赋值

## 多个变量赋值 
在对多个变量同时赋值，变量列表和值列表的各个元素用逗号分开，赋值语句右边的值会依次赋给左边的变量。
```
a, b = 10, 20
```
<br/>
遇到赋值语句Lua会先计算右边所有的值然后再执行赋值操作，所以我们可以这样进行交换变量的值：
```
x, y = y, x                     -- swap 'x' for 'y'
a[i], a[j] = a[j], a[i]         -- swap 'a[i]' for 'a[j]'
```
<br/>
当变量个数和值的个数不一致时，Lua会一直以变量个数为基础采取以下策略：
a. 变量个数 > 值的个数             按变量个数补足nil
b. 变量个数 < 值的个数             多余的值会被忽略
```
a, b, c = 0, 1
print(a,b,c)             --> 0   1   nil
 
a, b = a+1, b+1, b+2     -- value of b+2 is ignored
print(a,b)               --> 1   2
 
a, b, c = 0
print(a,b,c)             --> 0   nil   nil
```
# 循环

## while
```
while(condition)
do
   statements
end
```

## for
```
for var=exp1,exp2,exp3 do  
    <执行体>  
end  
```
exp1为循环初始值。
exp2为循环中止条件。
exp3为步长递增，如果不指定则默认为1。
三个表达式在循环开始前一次性求值，以后不再进行求值。例如这些表达式都为函数的情况。

Lua有种类似java中的foreach语句，泛型for循环通过一个迭代器函数来遍历所有值。
```
for i,v in ipairs(a) -- i为元素的下表,v为元素的值,ipairs是Lua提供的一个迭代器函数，用来迭代数组。
	do print(v) 
end  
```

## repeat...until 
```
repeat
   statements
until( condition )
```
在条件进行判断前循环体都会执行一次。
如果条件判断语句（condition）为 false，循环会重新开始执行，直到条件判断语句（condition）为 true 才会停止执行。

# 流程控制

## if
```
if(布尔表达式)
then
   --[ 在布尔表达式为 true 时执行的语句 --]
end
```
Lua认为false和nil为假，true 和非nil为真。要注意的是Lua中 0 为 true。

## if...else
```
if(布尔表达式)
then
   --[ 布尔表达式为 true 时执行该语句块 --]
else
   --[ 布尔表达式为 false 时执行该语句块 --]
end
```

## if...elseif...else
```
if( 布尔表达式 1)
then
   --[ 在布尔表达式 1 为 true 时执行该语句块 --]

elseif( 布尔表达式 2)
then
   --[ 在布尔表达式 2 为 true 时执行该语句块 --]

elseif( 布尔表达式 3)
then
   --[ 在布尔表达式 3 为 true 时执行该语句块 --]
else 
   --[ 如果以上布尔表达式都不为 true 则执行该语句块 --]
end
```

# 函数
Lua 编程语言函数定义格式如下：
```
optional_function_scope function function_name( argument1, argument2, argument3..., argumentn)
	function_body
	return result_params_comma_separated
end
```
* optional_function_scope: 该参数是可选的制定函数是全局函数还是局部函数，未设置该参数默认为全局函数，如果你需要设置函数为局部函数需要使用关键字 local。


* argument1, argument2, argument3..., argumentn:
函数参数，多个参数以逗号隔开，函数也可以不带参数。

* result_params_comma_separated:
函数返回值，Lua语言函数可以返回多个值，每个值以逗号隔开。


## 可变参数
Lua函数可以接受可变数目的参数，和C语言类似在函数参数列表中使用三点（...) 表示函数有可变的参数。
Lua将函数的参数放在一个叫arg的表中，#arg 表示传入参数的个数。
```
function average(...)
   result = 0
   local arg={...}
   for i,v in ipairs(arg) do
      result = result + v
   end
   print("总共传入 " .. #arg .. " 个数")
   return result/#arg
end

print("平均值为",average(10,5,3,4,5,6))
```

# 字符串操作
* ..
连接两个字符串

* ＃
一元运算符，返回字符串或表的长度。

# 模块
模块是由变量、函数等已知元素组成的 table，类似java的类。
```
-- 文件名为 module.lua
-- 定义一个名为 module 的模块
module = {}
 
-- 定义一个常量
module.constant = "这是一个常量"
 
-- 定义一个函数
function module.func1()
    io.write("这是一个公有函数！\n")
end
 
local function func2()
    print("这是一个私有函数！")
end
 
function module.func3()
    func2()
end
 
return module
```

<br/>
Lua提供了一个名为require的函数用来加载模块:
```
require("<模块名>")
或者
require "<模块名>"
```
还给加载的模块定义一个别名变量，方便调用: local m = require("module")

## 加载机制
对于自定义的模块，模块文件不是放在哪个文件目录都行，函数 require 有它自己的文件路径加载策略，它会尝试从 Lua 文件或 C 程序库中加载模块。
当 Lua 启动后，会以环境变量 LUA_PATH 的值来初始环境变量用户加载模块。





