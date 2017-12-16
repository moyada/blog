---
title: scala学习笔记
categories: scala
date: 2017-03-02 09:33:13
tags:
---

# 数据类型
| 数据类型 | 	描述|
|--------|------|
|Byte	| 8位有符号补码整数|
|Short	|16位有符号补码整数|
|Int	|32位有符号补码整数|
|Long	|64位有符号补码整数|
|Float	|32位IEEE754单精度浮点数|
|Double	|64位IEEE754单精度浮点数|
|Char	|16位无符号Unicode字符, 区间值为 U+0000 到 U+FFFF|
|String	|字符序列|
|Boolean	|true或false|
|Unit	|表示无值，和其他语言中void等同。|
|Null	|null 或空引用|
|Nothing	|Nothing类型在Scala的类层级的最低端；它是任何其他类型的子类型。|
|Any	|Any是所有其他类的超类|
|AnyRef	|AnyRef类是Scala里所有引用类(reference class)的基类|

scala中变量类型可由编译器的类型推断来确定，也可以显示声明数据类型，如下形式即可
```
val str : String = null
```
在scala中有一种特殊的数据类型Unit，代表任意类型，也等价于Java中的void，以如下形式声明
```
var any = ()
any = 1
any = "any"
```


# var与val的区别
var用作声明可变变量，val用作声明不可变变量

# for 循环
```
for( var x <- Range ){
   statement(s);
}
```
Range 可以是一个数字区间表示 i to j ，或者 i until j。左箭头 <- 用于为变量 x 赋值。
i to j　的循环区间为[i,j]，而i until j 的循环区间为[i,j)。

嵌套循环在 for 循环 中可以使用分号 (;) 来设置多个区间，它将迭代给定区间所有的可能值。
```
for( var x <- Range; var y <- Range ){
   statement(s);
}
```

## 循环集合
```
for( var x <- List ){
   statement(s);
}
```

## 循环过滤
```
for( var x <- List
      if condition1; if condition2...) {
	statement(s);
}
```

## 存储返回值变量
```
 var retVal = for{ a <- numList 
                        if a != 3; if a < 8
                      }yield a

```

## break语句
```
// 导入以下包
import scala.util.control._

// 创建 Breaks 对象
val loop = new Breaks;

// 在 breakable 中循环
loop.breakable{
    // 循环
    for(...){
       ....
       // 循环中断
       loop.break;
   }
}
```

# 函数
函数定义格式如下：
```
def functionName ([参数列表]) : [return type] = {
   function body
   return [expr]
}
```
方法定义由一个def 关键字开始，紧接着是可选的参数列表，一个冒号"：" 和方法的返回类型，一个等于号"="，最后是方法的主体。

## 可变参数: 函数的最后一个参数可以是重复的，参数类型后面以*设置

## 默认参数值: 参数后跟 =参数值 可以设置默认参数值

Scala的解释器在解析函数参数(function arguments)时有两种方式：
* 传值调用（call-by-value）：先计算参数表达式的值，再应用到函数内部
* 传名调用（call-by-name）：将未计算的参数表达式直接应用到函数内部
在进入函数内部前，传值调用方式就已经将参数表达式的值计算完毕，而传名调用是在函数内部进行参数表达式的值计算的。
这就造成了一种现象，每次使用传名调用时，解释器都会计算一次表达式的值。
```
object Test {
   def main(args: Array[String]) {
        delayed(time());
   }

   def time() = {
      println("获取时间，单位为纳秒")
      System.nanoTime
   }
   def delayed( t: => Long ) = {
      println("在 delayed 方法内")
      println("参数： " + t)
      t
   }
}

\\ 输出 
\\ 在 delayed 方法内
\\ 获取时间，单位为纳秒
\\ 参数： 241550840475831
\\ 获取时间，单位为纳秒
```

## 高阶函数
高阶函数可以使用其他函数作为参数，或者使用函数作为输出结果。
```
object Test {
   def main(args: Array[String]) {

      println( apply( layout, 10) )

   }
   // 函数 f 和 值 v 作为参数，而函数 f 又调用了参数 v
   def apply(f: Int => String, v: Int) = f(v)

   def layout[A](x: A) = "[" + x.toString() + "]"  
}
```

## 匿名函数
定义匿名函数的语法很简单，箭头左边是参数列表，右边是函数体。
```
def add2 = new Function1[Int,Int]{  
	def apply(x:Int):Int = x+1;  
// 简写：
var inc = (x:Int) => x+1
} 
```
不给匿名函数设置参数则参数列表为()

## 偏应用函数
使用偏应用函数绑定第一个 date 参数，第二个参数使用下划线(_)替换缺失的参数列表，并把这个新的函数值的索引的赋给变量。
```
def main(args: Array[String]) {
  val date = new Date
  val logWithDateBound = log(date, _ : String)

  logWithDateBound("message1" )
  Thread.sleep(1000)
  logWithDateBound("message2" )
  Thread.sleep(1000)
  logWithDateBound("message3" )
}

def log(date: Date, message: String)  = {
 println(date + "----" + message)
}
```

## 函数柯里化(Currying)
柯里化(Currying)指的是将原来接受两个参数的函数变成新的接受一个参数的函数的过程。新的函数返回一个以原有第二个参数为参数的函数。

```
// 接收一个x为参数，返回一个匿名函数，该匿名函数的定义是：接收一个Int型参数y，函数体为x+y。
def add(x:Int)=(y:Int)=>x+y

// 接收两个个String型参数s1, s2，返回一个匿名函数，函数体为s1 + s2。
def strcat(s1: String)(s2: String) = {
  s1 + s2
}
```

# 类和对象
类(class)是抽象的，不占用内存，而对象(object)是具体的，占用存储空间。

## 继承
1. 重写一个非抽象方法必须使用override修饰符。
2. 只有主构造函数才可以往父类的构造函数里写参数。
3. 在子类中重写父类的抽象方法时，你不需要使用override关键字。

## 单例对象
使用单例模式时，除了定义的类之外，还要定义一个同名的 object 对象，它和类的区别是，object对象不能带参数。

# Trait(特征)
类似于Java的抽象类，除了定义未实现的方法外，还可以定义属性和方法的实现，
并且可以多继承，使用的关键字是 trait。

## 特征构造顺序
1. 调用超类的构造器；
2. 特征构造器在超类构造器之后、类构造器之前执行；
3. 特质由左到右被构造；
4. 每个特征当中，父特质先被构造；
5. 如果多个特征共有一个父特质，父特质不会被重复构造
6. 所有特征被构造完毕，子类被构造。

# 模式匹配
```
x match {
      case 1 => "one"
      case 2 => "two"
      case y: Int => "int"
      case _ => "many"
   }
```
match 对应 Java 里的 switch，但是写在选择器表达式之后。即： 选择器 match {备选项}。
只要发现有一个匹配的case，剩下的case不会继续匹配。

## 样例类
使用了case关键字的类定义就是就是样例类(case classes)，样例类是种特殊的类，经过优化以用于模式匹配。
```
object Test {
   def main(args: Array[String]) {
   	val alice = new Person("Alice", 25)
	val bob = new Person("Bob", 32)
   	val charlie = new Person("Charlie", 32)
   
    for (person <- List(alice, bob, charlie)) {
    	person match {
            case Person("Alice", 25) => println("Hi Alice!")
            case Person("Bob", 32) => println("Hi Bob!")
            case Person(name, age) =>
               println("Age: " + age + " year, name: " + name + "?")
         }
      }
   }
   // 样例类
   case class Person(name: String, age: Int)
}
```

在声明样例类时，下面的过程自动发生了：
* 构造器的每个参数都成为val，除非显式被声明为var，但是并不推荐这么做；
* 在伴生对象中提供了apply方法，所以可以不使用new关键字就可构建对象；
* 提供unapply方法使模式匹配可以工作；
* 生成toString、equals、hashCode和copy方法，除非显示给出这些方法的定义。

# 文件 I/O
Scala 进行文件写操作，直接用的都是 java中 的 I/O 类 （java.io.File)：
```
val writer = new PrintWriter(new File("test.txt" ))

writer.write("菜鸟教程")
writer.close()
```
## 从屏幕上读取用户输入
```
val line = Console.readLine
```
## 从文件上读取内容
```
Source.fromFile("test.txt" ).foreach{ 
	print 
}
```

# 异常处理
## 捕获异常

```
try {
 val f = new FileReader("input.txt")
} catch {
 case ex: FileNotFoundException => {
    println("Missing file exception")
 }
 case ex: IOException => {
    println("IO Exception")
 }
} finally {
 println("Exiting finally...")
}
```
## 抛出异常
```
throw new IllegalArgumentException
```

# 提取器(Extractor)
提取器是从传递给它的对象中提取出构造该对象的参数。

Scala 提取器是一个带有unapply方法的对象。unapply方法算是apply方法的反向操作：unapply接受一个对象，然后从对象中提取值，提取的值通常是用来构造该对象的值。
## 提取器使用模式匹配
在我们实例化一个类的时，可以带上0个或者多个的参数，编译器在实例化的时会调用 apply 方法。我们可以在类和对象中都定义 apply 方法。
就像我们之前提到过的，unapply 用于提取我们指定查找的值，它与 apply 的操作相反。 当我们在提取器对象中使用 match 语句是，unapply 将自动执行。
