---
title: java8
categories: java
date: 2017-02-25 02:53:03
tags:
---

# Lambda表达式
java8最特色的就是lambda表达式了，简单介绍下，lambda表达式主要由两部分组成。
左边是参数列表，可以指定类型，并且用逗号分割；
右边是执行代码块，用花括号声明，可使用左边定义的参数。
参数与代码块中间用 "->"lambda操作符来区分。

```
(int x, int y) -> {return x+y;} //

(x, y) -> x + y // 左边的参数列表可以由类型推断来省略，右边只有一行语句也可以省略花括号和return

x -> x * x // 在只有一个参数时括号也可以省略

() -> System.out.println("java8");  // 左边没有参数，右边是没返回值的代码块（返回void）
```

在java8可以使用lambda可以很方便的实现匿名内部类

# 方法引用
方法引用可理解为Lambda的另外一种表现形式，如果有方法对Lambda接口的方法进行实现了，则可以用其替代。

| lambda表达式  | 方法引用    |
|--------------|------------|
| x -> System.out.println(x) | System.out::println |
| x -> String.valueOf(x)     | String::valueOf |
| x -> x.toString() | Object::toString |
| () -> x.toString() | x::toString |
| () -> new ArrayList<>() | ArrayList::new |
| () -> new String[] | String[]::new |

可以使用方法引用的有：
* 类的静态方法
* 对象的方法
* 类的构造函数
* 继承的方法

# 函数式接口
函数式接口的特点为有且只有一个接口(可以存在默认方法) 。

JDK在java.util.function下内置了的函数式接口、其中核心的有四个：
* Consumer<T>：消费型接口，接收输入执行代码但不返回数据
* Function<T, R>：函数型接口，接收输入并通过一定的操作返回定义类型的数据
* Supplier<T>：供给型接口，直接返回数据，无需输入参数
* Predicate<T>：断言型接口，对输入参数进行判断，返回布尔值

当内置接口不满足需求时也可以自己创建，并且可以使用@FunctionalInterface注解检测当前接口是否为规范的函数式接口

```
// 定义IO操作函数式接口
@FunctionalInterface
public interface FileOpt<T, R, X extends Throwable> {
    R apply(T instance) throws X;
}

// 定义函数型接口，输入文件名及流式操作
public static List<String> read2List(String fileName, FileOpt<Stream<String>, List<String>, IOException> block) throws IOException {
    List<String> result;
    try (BufferedReader reader = Files.newBufferedReader(Paths.get(fileName),  StandardCharsets.UTF_8)){
        result = block.apply(reader.lines());
    }
    return result;
}
```

# 流式操作
流主要分为两大类：串行流、并行流。
其中并行流的实质是java7的Fork/Join框架，由于并行流的执行需要对线程框架进行实现，所以当在小操作量的情况下不必使用并行流

流式操作分为三个部分：创建、中间操作、终止操作
只有当终止操作发生时、才会触发中间操作的执行，也就是说，没有执行终止操作的流不会做进行动作。

## 创建流
Stream.of()：传入需要包含在流内的元素
Stream.generate()：生成一个无限流，参数内为流内元素的工厂方法
通过Collection子类的stream()方法或 parallelStream()方法获取流：
通过IO/NIO对象Files创建文件流或目录流

## 中间操作
转换流：map()、flatMap()、mapToInt()、parallel() 、sequential()
过滤：filter()、limit()、skip()、distinct()
排序：sorted() 无参数时需流自身实现了自然排序

## 终止操作
匹配：anyMatch()、allMatch()、noneMatch()
返回：findAny()、findFirst() 返回的为Optional对象
聚集：count()、collect() collect操作还需要Collectors、Collector的静态方法，比较复杂，但功能很强大

另外：当返回为Optional对象时，还可以调用orElse()或其他方法对对象为空时进行处理


# 异步线程

# 时间类


# IO\NIO 的改进



# 重复注解

# JVM内存模型修改

