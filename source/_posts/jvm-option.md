---
title: jvm参数
categories: java
date: 2018-04-24 21:25:44
tags:
---

<style>
table th:nth-of-type(1) {
    width: 250px;
}

table th:nth-of-type(2) {
    width: 30px;
}

table th:nth-of-type(3) {
    width: 300px;
}

table th:nth-of-type(4) {
    width: 150px;
}
</style>

# 空间
## 通用

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -Xms`<size><unit>` | ● | Heap初始化的大小 | |
| -Xmx`<size><unit>` | ● | Heap最大值 | |
| -Xmn`<size><unit>` | ● | 新生代的大小 | G1 GC下建议不设置该参数 |
| -Xss`<size><unit>` | ● | 线程栈的大小，默认1M| |
| -XX:MaxDirectMemorySize=`<size><unit>` | | 设置NIO的直接缓存最大容量 | |
| -XX:SurvivorRatio=`<size>` | ● | Eden和Survior(from和to)大小比例，默认是8 | | 
| -XX:AutoBoxCacheMax=`<size>` | | 设置自动装箱池缓存大小 | server模式专有 |
| -XX:+UseAdaptiveGCBoundary | | 动态化使用资源 | |
| -XX:+UseAdaptiveSizePolicy | | 动态调整各个代区的内存大小，每次minor gc后会重新计算eden，from和to的大小 | |
| -XX:NewSize=`<size><unit>` | | 设置新生代的初始大小 | |
| -XX:MaxNewSize=`<size><unit>` | | 设置新生代的最大值 | |
| -XX:NewRatio=`<size>` | | 设置新生代和老年代的比例 | |

## Java8之前

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:PermSize | ● | 方法区的初始化大小 | java8后废弃 |
| -XX:MaxPermSize | ● | 方法区的最大值 | java8后废弃 |

## Java8之后

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:MetaspaceSize=`<size><unit>` | ● | 元空间的初始化大小 | java8后新增 | 
| -XX:MaxMetaspaceSize=`<size><unit>` | ● | 元空间的最大值 | java8后新增，初始大小是21M |

## Java9之后

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:G1HeapRegionSize=`<size><unit>` | ● | 设置每个区域块大小 | 1M-32M 之间,必须是2的幂 |
| -XX:G1NewSizePercent=`<size>` |  | 设置年轻代大小占堆的最小值百分比 | 默认值是 Java 堆的 5% | 
| -XX:G1MaxNewSizePercent=`<size>` |  | 设置年轻代大小占堆的最大值百分比 | 默认值是 Java 堆的 60% | 

# 性能

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -server | ● | 虚拟机会以Server模式运行，该模式与C2编译器共同运行，更注重编译的质量，启动速度慢，但是运行效率高，适合用在服务器环境下，针对生产环境进行了优化 |  |
| -XX:+AlwaysPreTouch | ● | JVM启动时就会先访问所有分配给它的内存,让操作系统把内存真正的分配给JVM | |
| -XX:+UseTLAB | ● | 通过快速对象分配模式在TLAB（Thread Local Allocation Buffer）中进行分配对象 | |
| -XX:+PerfDisableSharedMem | ● | GC日志指向/dev/shm，避免IO造成的JVM停顿 | |
| -XX:-OmitStackTraceInFastThrow | ● | 强制要求JVM始终抛出含堆栈的异常 | |
| -XX:+UseCompressedOops | ● | 在64位环境下，压缩对象头 | |
| -XX:+UseFastAccessorMethods | | 原始类型的快速优化 | | 
| -XX:+UseFastEmptyMethods | | 空方法优化 | |
| -XX:+UseFastJNIAccessors | | 引用类或int的成员方法优化 | |
| -XX:LargePageSizeInBytes | | Heap中每页的最大值，不可设置过大 | |
| -XX:-UseBiasedLocking | | 禁用偏向锁 | 在存在大量锁对象的创建并高度并发的环境下禁用偏向锁能够带来一定的性能优化 | 
| -XX:BiasedLockingStartupDelay=`<size>` | | | 延迟(秒钟)启用偏向锁 |
| -XX:TLABSize=`<size>` | | 设置TLAB的初始化大小 | |
| -XXtlaSize:min=`<size><unit>`,preferred=`<size><unit>` | | 调整TLA，每个线程私有的空间的默认最小大小和默认首选大小 | |
| -XX:-ResizeTLAB | | 禁用自动调整TLAB | |
| -XX:+UseLargePages | | 启用Large Page的记忆体存取能力，需要OS支持 | |
| -XX:+AlwaysAtomicAccesses | | 实现对所有Access的原子性保证 | |
| -XX:+EliminateAllocations | | 开启标量替换 | |
| -XX:+DoEscapeAnalysis | | 进行逃逸分析之后，创建的可分解的对象都将由栈上分配 | 标量替换，栈上分配，受限于栈的空间大小 |
| -XX:+PrintEscapeAnalysis | | 查看逃逸分析结果 | |
| -XX:+EliminateLocks | | 分析并且消除无线程竞争下的锁 | 同步消除，必须开启 -XX:+DoEscapeAnalysis 和 -server模式 |
| -XX:+PrintEliminateAllocations | | 查看标量的替换情况 | |
| -XX:-ResizePLAB | | 关闭PLAB的大小调整 | 以避免大量的线程通信所导致的性能下降 |
| -XX:LiveNodeCountInliningCutoff=`<size>` | | max number of live nodes in a method | |

# GC
## 通用

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:+UseCompressedClassPointers | ● | 开启压缩类指针 | 64位平台上默认打开 |
| -XX:+UseParNewGC | ● | 设置年轻代为多线程收集 | 可以和CMS GC一起使用 |
| -XX:+ParallelRefProcEnabled | ● | 并行的处理对象标记过程 | |
| -XX:ParallelGCThreads=`<size>` | ● | 指定GC线程数 | 默认GC线程数为CPU的数量 |
| -XX:ParGCCardsPerStrideChunk=`<size>` | | 设置每个线程每次扫描的Card数量 | CardTable用来标记老年代的某一块内存区域中的对象是否持有新生代对象的引用,卡表的数量取决于老年代的大小和每张卡对应的内存大小 |
| -XX:+DisableExplicitGC | | 禁止代码中显示调用GC | CMS下可使用-XX:+ExplicitGCInvokesConcurrent替换 |
| -XX:+GCLockerInvokesConcurrent | | 并发的执行GC Lock | |
| -XX:CMSFullGCsBeforeCompaction=`<size>` | | 指定进行多少次Full GC之后，执行内存空间整理 | |
| -XX:MaxGCPauseMillis=`<size>` | | 设置每次年轻代垃圾回收的最长时间 | | 
| -XX:ParallelGCThreads=`<size>` | | 设置 STW 工作线程数的值 | |
| -XX:ConcGCThreads=`<size>` | | 设置并行GC的线程数 | 设置为并行垃圾回收线程数 (ParallelGCThreads) 的 1/4 左右 |
| -XX:MaxTenuringThreshold=`<size>` | | 设置新生代经过多少次YGC晋升到老生代 | |
| -XX:PretenureSizeThreshold=`<size>` | | 晋升老年代对象年龄 | 无默认值，Paralle Scavenge收集器无法识别 | 
| -XX:CompressedClassSpaceSize=`<size><unit>` | | 设置指针压缩空间大小 | |
| -XX:+OptimizeStringConcat | | 字符串concat优化 | 默认开启 |
| -XX:+UseParallelGC | | 选择垃圾收集器为并行收集器 | 不能和CMS GC一起使用,系统吨吐量优先 |
| -XX:GCTimeRatio=`<size>` | | 设置吞吐量大小 |
| -XX:+CMSScavengeBeforeRemark | | 重新标记之前对年轻代做一次Minor GC | |
| -XX:+CMSClassUnloadingEnabled | | 对永久代进行垃圾回收(类卸载) | 在Full GC时会扫描MetaSpace/PermGen |
| -XX:SoftRefLRUPolicyMSPerMB=`<size>` | | 设置每兆空间中软引用的生命周期(多少毫秒后清除) | |

## CMS

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:+UseConcMarkSweepGC | ● | 启用CMS低停顿垃圾收集器 | |
| -XX:+CMSParallelRemarkEnabled | ● | 开启并行标记 | |
| -XX:+ExplicitGCInvokesConcurrent | ● | 命令JVM无论什么时候调用系统GC，都执行CMS GC，而不是Full GC | |
| -XX:+ExplicitGCInvokesConcurrent <br/> AndUnloadsClasses | ● | 保证当有系统GC调用时，永久代也被包括进CMS垃圾回收的范围内 | |
| -XX:CMSInitiatingOccupancyFraction=`<size>` | ● | 指设定CMS在对内存占用率达到多少百分比的时候开始GC | |
| -XX:+UseCMSInitiatingOccupancyOnly | ● | 使用回收阈值 | 只有开启了这个参数，CMSInitiatingOccupancyFraction这个参数才会生效 |
| -XX:+CMSConcurrentMTEnabled | | 并发的CMS阶段将以多线程执行 | 默认开启 |
| -XX:+CMSParallelInitialMarkEnabled | | 开启初始标记过程中的并行化 | |
| -XX:ParallelCMSThreads=`<size>` | | 设定 CMS 的线程数量 | |
| -XX:UseCMSCompactAtFullCollection | | 在FULL GC的时候，对年老代的压缩 | 可能会影响性能,但是可以消除碎片 |

## G1

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:+UseG1GC | ● | 使用G1垃圾回收器 | |
| -XX:InitiatingHeapOccupancyPercent=`<size>` | | 设置触发并发标记周期时的堆内存占用率阈值. G1之类的垃圾收集器用它来触发并发GC周期,基于整个堆的使用率,而不只是某一代内存的使用比 | 值为 0 则表示"一直执行GC循环",默认值为 45 |
| -XX:+G1UseAdaptiveIHOP | | 开启自适应并发标记控制 | |
| -XX:+UseStringDeduplication | | 使用字符串去重机制 | G1收集器下生效 |
| -XX:G1MixedGCLiveThresholdPercent=`<size>` | | 设置混合垃圾回收周期中要包括的旧区域设置占用率阈值 | 默认占用率为 65% |
| -XX:G1HeapWastePercent=`<size>` | | 设置您愿意浪费的堆百分比，如果可回收百分比小于堆废物百分比，JVM不会启动混合垃圾回收周期 | 默认值是 10% |
| -XX:G1MixedGCCountTarget=`<size>` | | 设置标记周期完成后，对存活数据旧区域执行混合垃圾回收的目标次数 | 默认值是8 |
| -XX:G1OldCSetRegionThresholdPercent=`<size>` | | 设置混合垃圾回收期间要回收的最大旧区域数 | 默认值是 Java 堆的 10% |
| -XX:G1ReservePercent=`<size>` | | 设置作为预留存活区在heap中的百分比 | 默认值是 10% |
| -XX:+G1SummarizeConcMark | | | |
| -XX:+G1PrintHeapRegions | | 打印G1收集器收集的区域 | |
| -XX:+G1SummarizeRSetStats | | 打印标记过程引用信息(Print RSet processing information) | |
| -XX:G1SummarizeRSetStatsPeriod=`<size>` | | 指定GC周期频率报告 | |
| -XX:-G1EagerReclaimHumongousObjects | | 禁用G1优先尝试回收大对象 | |
| -XX:+G1TraceEagerReclaimHumongousObjects | | Prints details about live and dead Humongous objects during each collection | |
| -XX:+G1ConcRegionFreeingVerbose | | Debug JVM | |

## Graal

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:+EnableJVMCI | | 使用Graal | java10 |
| -XX:+UseJVMCICompiler | | 启动Graal JIT编译器 | java10 |

# JIT 

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:+TieredCompilation | ● | 启用分层编译策略，根据编译器编译、优化的规模与耗时，划分出不同的编译层次 | java8默认开启 |
| -XX:+UseCodeCacheFlushing | | 当代码缓存被填满时让JVM放弃一些编译代码 | |
| -XX:InitialCodeCacheSize=`<size>` | | 设置初始代码缓存的大小 | |
| -XX:ReservedCodeCacheSize=`<size><unit>` | | 设置代码缓存的最大值 | |
| -XX:CompileThreshold=`<size>` | | 当某个方法被调用+循环次数累计超过该值时，触发标准的JIT编译 | |
| -XX:InterpreterBackwardBranchLimit=`<size>` | | 当某个方法被调用+循环次数累计超过该值时，触发OSR形式的JIT编译 | |
| -XX:HugeMethodLimit=`<size>` | | JIT编译字节码大小超过size字节的方法就是巨型方法 | |
| -XX:+DontCompileHugeMethods | | 不编译巨型方法 | |
| -XX:-UseCounterDecay | | 禁止JIT调用计数器衰减 | |
| -XX:+CompileCommand=`command,method[,option]` | | 定制编译需求，比如过滤某个方法不做JIT编译 | 
| -XX:+InlineSynchronizedMethods | | 对同步方法进行内联 | |
| -XX:MaxInlineLevel=`<size>` | | 在进行方法内联前，方法的最多嵌套调用次数 | 默认是9 |
| -XX:MaxInlineSize=`<size>` | | 编译内联的方法的最大byte code大小 | 默认是35 |
| -XX:FreqInlineSize=`<size>` | | 内联频繁执行的方法的最大字节码大小 | |
| -XX:MaxTrivialSize=`<size>` | | maximum bytecode size of a trivial method to be inlined | |
| -XX:MinInliningThreshold=`<size>` | | 方法被内联的最小调用次数 | 默认是9 |

# 日志

<big><b>Java9之后</b></big>

jvm日志使用 `-Xlog[<what>][:[<output>][:[<decorators>][:<output-options>]]]` 记录
可以使用`java -Xlog:help`查看帮助文档

> 案例: -Xlog:disable -Xlog:gc+liveness=info,rt*=off:file=../logs/gc_%t.log:time,uptimemillis,pid:filecount=5,filesize=1024
> 先关闭所有日志，打开gc和存活对象的日志，关闭包含rt的日志级别，输出日志至文件，额外包含时间、耗时、进程id，以5个1M的文件循环写入再汇总至日志文件。

参考:
http://www.cnblogs.com/IcanFixIt/p/7259712.html
https://juejin.im/post/5a981f056fb9a028bf04bec4
https://blog.gceasy.io/2017/10/17/43-gc-logging-flags-removed-in-java-9/

* http://openjdk.java.net/jeps/158

## 通用

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:ErrorFile=`<file>` | ● | 当jvm出现致命错误时，生成错误文件，包括了导致jvm crash的重要信息 | |
| -XX:+PrintGC | ● | 打印每次GC的消息 | |
| -XX:+PrintGCApplicationStoppedTime | ● | 打印JVM的停顿时间 | |
| -XX:+HeapDumpOnOutOfMemoryError | ● | 遇到OutOfMemoryError时拍摄一个“堆转储快照”，并将其保存在一个文件中。 | |
| -XX:HeapDumpPath=`<file>` | ● | 指定导出堆的存放路径 | |
| -XX:-OmitStackTraceInFastThrow | ● | 要求JVM始终抛出含堆栈的异常 | |
| -verbose:class | | 在程序运行的时候有多少类被加载 | verbose:class来监视 java -verbose:class |
| -verbose:gc | | 在虚拟机发生内存回收时在输出设备显示信息 | |
| -verbose:jni | | 输出native方法调用的相关情况 | |
| -XX:+PrintHeapAtGC | | 在进行GC的前后打印出堆的信息 | |
| -XX:+PrintCompilation | | 简单的输出一些关于从字节码转化成本地代码的编译过程 | |
| -XX:+PrintFlagsInitial | | 显示所有可设置参数及默认值 | |
| -XX:+PrintFlagsFinal | | 显示可以获取到所有设置后参数及值 | |
| -XX:+PrintAssembly | | 通过使用外部的disassembler.so库打印汇编的字节码和native方法来辅助分析 | 需要和-XX:UnlockDiagnosticVMOptions一起使用 |
| -XX:+UnlockDiagnosticVMOptions | | 解锁对JVM进行诊断的选项参数 | |
| -XX:+PrintCommandLineFlags | | 显示出JVM初始化完毕后所有跟最初的默认值不同的参数及它们的值 | |
| -XX:+PrintInlining | | 打印内联优化的方法 | |
| -XX:+PrintTenuringDistribution | | 打印内存模型各代信息 | |
| -XX:+PrintGCCause | | 打印GC的原因 | |
| -XX:+DisplayVMOutput | | 打印JVM输出 | |
| -XX:+LogVMOutput | | 记录JVM输出到日志 | |
| -XX:+PrintJNIGCStalls | | 打印进入临界区(JVM传向JNI)的线程信息 | |
| -XX:+PrintGCApplicationConcurrentTime | | 打印JVM在两次停顿之间的正常运行时间 | |
| -XX:+PrintSafepointStatistics | | 输出safepoint的统计信息 | |
| -XX:+PrintSafepointStatisticsCount=`<size>` | | 输出safepoint的统计信息 | |
| -XX:+PrintInterpreter | | 打印解释过程中生成的汇编指令 | exclude，跳过编译指定的方法;compileonly，只编译指定的方法;inline/dontinline，设置是否内联指定方法;print，打印生成的汇编代码 |
| -XX:+PrintAssembly | | 打印JIT编译过程中生成的汇编指令 | 无法使用该参数可以用-XX:+PrintOptoAssembly来代替 |
| -XX:+PrintAdaptiveSizePolicy | | 打印自适应收集的大小 | |
| -XX:+PrintReferenceGC | | 跟踪系统内的软引用,弱引用,虚引用和finallize队列 | |
| -XX:+PrintClassHistogram | | 打印出实例的数量以及空间大小 | |
| -XX:+TraceClassLoading | | 动态跟踪类的加载 | |
| -XX:+TraceClassUnloading | | 动态跟踪类的卸载 | |

## CMS

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:LogFile=`<file>` | | 日志文件 | 例如 -XX:LogFile=/dev/shm/vm.log |
| -XX:+PrintGCDetails | ● | 打印GC日志详情 | 建议开启 |
| -XX:+PrintGCTimeStamps | | 输出GC的时间戳 | 以基准时间的形式 |
| -XX:+PrintGCDateStamps | | 输出GC的时间戳 | 以日期的形式，如 2013-05-04T21:53:59.234+0800 |

## G1

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -XX:G1LogLevel=fine, finer, finest | | 日志包含信息以及每个工作线程的信息 | |
| -XX:+PrintStringDeduplicationStatistics | | 打印字符串去重的影响 | |

# 其他

| 参数 | 常用 | 说明 | 备注 | 
| :- | :-: | :- | 
| -Dcom.sun.management.jmxremote <br/> -Dcom.sun.management.jmxremote.port=`<port>` <br/> -Dcom.sun.management.jmxremote.authenticate=false <br/> -Dcom.sun.management.jmxremote.ssl=false | ● | 开启远程监控 | |
| -Djava.rmi.server.hostname=`<host>` | | 指定RMI服务的对应ip | |
| -Djava.net.preferIPv4Stack=true | | 优先使用IPv4 | |
| -XX:+UnlockExperimentalVMOptions | | 解锁JVM无法识别参数 | |
| -XX:+AggressiveOpts | | 使用预设的优化參數 | |
| -Djava.awt.headless=true | | 使用计算能力模拟外设功能 | 例如创建图片 |
