# 整理了100份C/C++面经的校招、社招C/C++面试题总结

<font style="color:rgb(51, 51, 51);">编者：由B站 程序员老廖 整理</font>

[<font style="color:rgb(65, 131, 196);">程序员老廖的个人空间-程序员老廖个人主页-哔哩哔哩视频 (bilibili.com)</font>](https://space.bilibili.com/3494351095204205)

<font style="color:rgb(51, 51, 51);">版权声明：面试题来源于网上各大程序员面经总结，如有侵权请在B站私信 程序员老廖 删除。</font>

<font style="color:rgb(51, 51, 51);">注：部分面试题分类不一定严格按类别分类。</font>

<font style="color:rgb(51, 51, 51);">200+道常见面试题。大部分的面试题对于C++后端、桌面开发、嵌入式、音视频开发都是通用的。</font>

**<font style="color:rgb(51, 51, 51);">通用的C/C++技术栈</font>**

1. <font style="color:rgb(51, 51, 51);">C/C++</font>
2. <font style="color:rgb(51, 51, 51);">数据结构算法</font>
3. <font style="color:rgb(51, 51, 51);">gdb/gcc/g++</font>
4. <font style="color:rgb(51, 51, 51);">cmake</font>
5. <font style="color:rgb(51, 51, 51);">设计模式</font>
6. <font style="color:rgb(51, 51, 51);">操作系统</font>
    1. <font style="color:rgb(51, 51, 51);">Linux系统原理</font>
    2. <font style="color:rgb(51, 51, 51);">Linux系统编程</font>
7. <font style="color:rgb(51, 51, 51);">网络</font>
    1. <font style="color:rgb(51, 51, 51);">网络原理</font>
    2. <font style="color:rgb(51, 51, 51);">网络编程</font>

**<font style="color:rgb(51, 51, 51);">Linux C/C++后台开发</font>**<font style="color:rgb(51, 51, 51);">，除了通用的C/C++技术栈外，重点掌握：</font>

1. **<font style="color:rgb(51, 51, 51);">mysql</font>**
2. **<font style="color:rgb(51, 51, 51);">redis</font>**
3. **<font style="color:rgb(51, 51, 51);">远程调用rpc，比如grpc</font>**
4. **<font style="color:rgb(51, 51, 51);">消息队列，比如kafka、zeromq</font>**
5. **<font style="color:rgb(51, 51, 51);">高性能服务器，可以重点研究workflow。</font>**
6. <font style="color:rgb(51, 51, 51);">分布式</font>
    1. <font style="color:rgb(51, 51, 51);">分布式锁</font>
    2. <font style="color:rgb(51, 51, 51);">注册服务发现</font>
7. <font style="color:rgb(51, 51, 51);">云原生（先以了解为主，校招不建议作为重点内容）</font>
    1. <font style="color:rgb(51, 51, 51);">docker</font>
    2. <font style="color:rgb(51, 51, 51);">k8s</font>

# <font style="color:rgb(51, 51, 51);">1 C/C++</font>
<font style="color:rgb(51, 51, 51);">常见问题：智能指针、多态、虚函数、stl原理。</font>

1. **<font style="color:rgb(51, 51, 51);">智能指针</font>**<font style="color:rgb(51, 51, 51);">实现原理</font>
2. <font style="color:rgb(51, 51, 51);">智能指针，里面的计数器何时会改变</font>
3. <font style="color:rgb(51, 51, 51);">智能指针和管理的对象分别在哪个区（智能指针本身在栈区，托管的资源在堆区，利用了栈对象超出生命周期后自动析构的特征，所以无需手动delete释放资源。</font>
4. <font style="color:rgb(51, 51, 51);">面向对象的特性：</font>**<font style="color:rgb(51, 51, 51);">多态</font>**<font style="color:rgb(51, 51, 51);">原理</font>
5. <font style="color:rgb(51, 51, 51);">介绍一下虚函数，虚函数怎么实现的</font>
6. <font style="color:rgb(51, 51, 51);">多态和继承在什么情况下使用</font>
7. <font style="color:rgb(51, 51, 51);">除了多态和继承还有什么面向对象方法</font>
8. <font style="color:rgb(51, 51, 51);">C++内存分布。什么样的数据在栈区，什么样的在堆区</font>
9. <font style="color:rgb(51, 51, 51);">C++内存管理（RAII啥的）</font>
10. <font style="color:rgb(51, 51, 51);">C++从源程序到可执行程序的过程</font>
11. <font style="color:rgb(51, 51, 51);">一个对象=另一个对象会发生什么（赋值构造函数）</font>
12. <font style="color:rgb(51, 51, 51);">如果new了之后出了问题直接return。会导致内存泄漏。怎么办（智能指针，raii）</font>
13. <font style="color:rgb(51, 51, 51);">c++11的智能指针有哪些。weak_ptr的使用场景。什么情况下会产生循环引用</font>
14. <font style="color:rgb(51, 51, 51);">多进程fork后不同进程会共享哪些资源</font>
15. <font style="color:rgb(51, 51, 51);">多线程里线程的同步方式有哪些</font>
16. <font style="color:rgb(51, 51, 51);">size_of是在编译期还是在运行期确定</font>
17. <font style="color:rgb(51, 51, 51);">函数重载的机制。重载是在编译期还是在运行期确定</font>
18. <font style="color:rgb(51, 51, 51);">指针常量和常量指针</font>
19. <font style="color:rgb(51, 51, 51);">vector的原理，怎么扩容</font>
20. <font style="color:rgb(51, 51, 51);">介绍一下const</font>
21. <font style="color:rgb(51, 51, 51);">引用和指针的区别</font>
22. <font style="color:rgb(51, 51, 51);">Cpp新特性知道哪些</font>
23. <font style="color:rgb(51, 51, 51);">类型转换</font>
24. <font style="color:rgb(51, 51, 51);">RAII基于什么实现的（生命周期、作用域、构造析构</font>
25. <font style="color:rgb(51, 51, 51);">手撕：Unique_ptr，控制权转移(移动语义）</font>

<font style="color:rgb(51, 51, 51);">手撕：类继承，堆栈上分别代码实现多态</font>

1. <font style="color:rgb(51, 51, 51);">unique_ptr和shared_ptr区别</font>
2. <font style="color:rgb(51, 51, 51);">右值引用</font>
3. <font style="color:rgb(51, 51, 51);">函数参数可不可以传右值</font>
4. <font style="color:rgb(51, 51, 51);">参考c/c++堆栈实现自己的堆栈。要求：不能用stl容器。</font>
5. <font style="color:rgb(51, 51, 51);">stl容器了解吗？底层如何实现：vector数组，map红黑树，红黑树的实现</font>
6. <font style="color:rgb(51, 51, 51);">完美转发介绍一下 去掉std::forward会怎样？</font>
7. <font style="color:rgb(51, 51, 51);">介绍一下unique_lock和lock_guard区别？</font>
8. <font style="color:rgb(51, 51, 51);">C代码中引用C++代码有时候会报错为什么？</font>
9. <font style="color:rgb(51, 51, 51);">静态多态有什么？</font><font style="color:rgb(51, 51, 51);"> 虚函数原理 虚表是什么时候建立的</font><font style="color:rgb(51, 51, 51);">为什么要把析构函数设置成虚函数？</font>
10. <font style="color:rgb(51, 51, 51);">map为啥用红黑树不用avl树？（几乎所有面试都问了map和unordered_map区别）</font>
11. <font style="color:rgb(51, 51, 51);">inline 失效场景</font>
12. <font style="color:rgb(51, 51, 51);">C++ 中 struct 和 class 区别</font>
13. <font style="color:rgb(51, 51, 51);">如何防止一个头文件 include 多次</font>
14. <font style="color:rgb(51, 51, 51);">lambda表达式的理解，它可以捕获哪些类型</font>
15. <font style="color:rgb(51, 51, 51);">友元friend介绍</font>
16. <font style="color:rgb(51, 51, 51);">move函数</font>
17. <font style="color:rgb(51, 51, 51);">模版类的作用</font>
18. <font style="color:rgb(51, 51, 51);">模版和泛型的区别</font>
19. <font style="color:rgb(51, 51, 51);">内存管理：C++的new和malloc的区别</font>
20. <font style="color:rgb(51, 51, 51);">new可以重载吗，可以改写new函数吗</font>
21. <font style="color:rgb(51, 51, 51);">C++中的map和unordered_map的区别和使用场景</font>
22. <font style="color:rgb(51, 51, 51);">他们是线程安全的吗</font>
23. <font style="color:rgb(51, 51, 51);">c++标准库里优先队列是怎么实现的？</font>
24. <font style="color:rgb(51, 51, 51);">gcc编译的过程</font>
25. <font style="color:rgb(51, 51, 51);">C++ Coroutine</font>
26. <font style="color:rgb(51, 51, 51);">extern C有什么作用</font>
27. <font style="color:rgb(51, 51, 51);">c++ memoryorder/elf文件格式/中断对于操作系统的作</font>
28. <font style="color:rgb(51, 51, 51);">C++的符号表</font>
29. <font style="color:rgb(51, 51, 51);">C++的单元测试</font>

# <font style="color:rgb(51, 51, 51);">2 数据结构算法</font>
<font style="color:rgb(51, 51, 51);">常见问题：链表、排序、二叉树</font>

1. <font style="color:rgb(51, 51, 51);">数组和链表区别和优缺点</font>
2. <font style="color:rgb(51, 51, 51);">快速排序</font>
3. <font style="color:rgb(51, 51, 51);">堆排序是怎么做的</font>
4. <font style="color:rgb(51, 51, 51);">冒泡排序</font>
5. <font style="color:rgb(51, 51, 51);">二分查找（复杂度）</font>
6. <font style="color:rgb(51, 51, 51);">hash表数据很大。rehash的代价很高，怎么办</font>
7. <font style="color:rgb(51, 51, 51);">二叉树前序遍历非递归</font>
8. <font style="color:rgb(51, 51, 51);">链表反转</font>
9. <font style="color:rgb(51, 51, 51);">二叉树输出每一层最右边的节点</font>
10. <font style="color:rgb(51, 51, 51);">千万级数组如何求最大k个数？（用最小堆反之最大堆）</font><font style="color:rgb(51, 51, 51);">千万数据范围有限，0到1000，有很多重复的，按频率排序怎么处理？</font>
11. <font style="color:rgb(51, 51, 51);">计算二叉树层高。</font>
12. <font style="color:rgb(51, 51, 51);">给一个连续非空子数组，找它乘积最大的（动态规划）</font>
13. <font style="color:rgb(51, 51, 51);">排序算法. 哪些是稳定的，哪些不稳定的</font>
14. <font style="color:rgb(51, 51, 51);">树的深度和高度。一开始分别用了一个层序遍历和一个dfs，然后面试官问能否都在一个dfs里面呢，提示了一下在dfs是否可以传一个参数，然后解决了。</font>
15. <font style="color:rgb(51, 51, 51);">布隆过滤器介绍</font>
16. <font style="color:rgb(51, 51, 51);">为什么不用布隆过滤器</font>
17. <font style="color:rgb(51, 51, 51);">.数据结构相关，图的种类，表示方法，图有哪些经典算法+描述算法</font>
18. <font style="color:rgb(51, 51, 51);">求最大的k个数字，解法：优先队列（堆）或者快速排序</font>
19. <font style="color:rgb(51, 51, 51);">一个大数问题，解法：转换为字符串解决，这题没写好，leetcode应该有很多类似的问题</font>
20. <font style="color:rgb(51, 51, 51);">hash解决冲突 （ 开放定址法、链地址法、再哈希法、建立公共溢出区 ），四种方式详细的过程、思路</font>
21. <font style="color:rgb(51, 51, 51);">链地址法和再哈希法之间的关联和区别，两者分别适用场景，两者底层的数据结构，关联和区别</font>
22. <font style="color:rgb(51, 51, 51);">链表和数组的底层结构设计、关联、区别、应用场景</font>
23. <font style="color:rgb(51, 51, 51);">死锁的概念，进程调度算法怎么解决死锁</font>

# <font style="color:rgb(51, 51, 51);">3 gdb/gcc/g++</font>
1. <font style="color:rgb(51, 51, 51);">怎么debug，怎么看内存泄漏。</font>
2. <font style="color:rgb(51, 51, 51);">gdb 使用 -> 多线程程序切换到某线程栈帧 -> 如何查看寄存器值</font>
3. <font style="color:rgb(51, 51, 51);">怎么分析C++的core文件</font>
4. <font style="color:rgb(51, 51, 51);">GDB有哪些命令</font>
5. **<font style="color:rgb(51, 51, 51);">gcc和g++的区别</font>**
6. **<font style="color:rgb(51, 51, 51);">Linux下程序有问题，如何调试？</font>**<font style="color:rgb(51, 51, 51);">（答GDB打开，打上Breakpoint进行调试）</font>

# <font style="color:rgb(51, 51, 51);">4 cmake</font>
1. <font style="color:rgb(51, 51, 51);">如何构建工程</font>
2. <font style="color:rgb(51, 51, 51);">如何生成动态库</font>
3. <font style="color:rgb(51, 51, 51);">如何生成静态库</font>
4. <font style="color:rgb(51, 51, 51);">如何引用第三方库</font>

# <font style="color:rgb(51, 51, 51);">5 设计模式</font>
1. <font style="color:rgb(51, 51, 51);">单例模式实现区别，比如懒汉、饿汉</font>
2. <font style="color:rgb(51, 51, 51);">策略模式实现</font>
3. <font style="color:rgb(51, 51, 51);">工厂模式</font>
4. <font style="color:rgb(51, 51, 51);">观察者模式</font>
5. <font style="color:rgb(51, 51, 51);">责任链模式</font>
6. <font style="color:rgb(51, 51, 51);">组合模式</font>

<font style="color:rgb(51, 51, 51);">重点准备5、6种即可，没有必要都同等重要。</font>

# <font style="color:rgb(51, 51, 51);">6 操作系统</font>
## <font style="color:rgb(51, 51, 51);">Linux系统原理</font>
1. **<font style="color:rgb(51, 51, 51);">线程和进程</font>**<font style="color:rgb(51, 51, 51);">的区别、应用场景。</font>
2. <font style="color:rgb(51, 51, 51);">多线程中各种锁，读写锁，互斥锁</font>
3. **<font style="color:rgb(51, 51, 51);">内存池</font>**
4. <font style="color:rgb(51, 51, 51);">内存管理</font>
5. <font style="color:rgb(51, 51, 51);">内存写漏</font>
6. <font style="color:rgb(51, 51, 51);">如果频繁进行内存的分配释放会有什么问题吗？</font>
7. <font style="color:rgb(51, 51, 51);">如果频繁分配释放的内存很大（>128k）,怎么处理？</font>
8. <font style="color:rgb(51, 51, 51);">虚拟内存以及堆栈溢出相关的问题，堆栈溢出怎么处理等等。</font>
9. <font style="color:rgb(51, 51, 51);">分段和分页的区别</font>
10. <font style="color:rgb(51, 51, 51);">进程间通信原理和方式</font>
11. <font style="color:rgb(51, 51, 51);">fork()读时共享写时拷贝</font>
12. <font style="color:rgb(51, 51, 51);">互斥锁+条件变量</font>
13. <font style="color:rgb(51, 51, 51);">如果非堆内存一直在增长，可能哪个区域的内存出了问题（Java）</font>
14. <font style="color:rgb(51, 51, 51);">堆和栈的区别。什么情况下会往堆里放</font>
15. <font style="color:rgb(51, 51, 51);">fork函数返回值是怎么实现的</font>
16. <font style="color:rgb(51, 51, 51);">用户级线程和内核级线程的区别</font>
17. **<font style="color:rgb(51, 51, 51);">线程池</font>**<font style="color:rgb(51, 51, 51);">和线程开销</font>
18. <font style="color:rgb(51, 51, 51);">线程切换的到底是什么</font>
19. <font style="color:rgb(51, 51, 51);">线程同步共享怎么实现</font>
20. <font style="color:rgb(51, 51, 51);">互斥同步的方法</font>
21. <font style="color:rgb(51, 51, 51);">信号量和自旋锁的区别</font>
22. <font style="color:rgb(51, 51, 51);">查看磁盘、cpu 占用、内存占用命令</font>
23. <font style="color:rgb(51, 51, 51);">linux虚拟地址空间结构/动态库地址无关代码 </font>
24. <font style="color:rgb(51, 51, 51);">top命令排查高占有率进程/top命令的占用率怎么算的</font>
25. <font style="color:rgb(51, 51, 51);">谈谈进程创建后在Linux中的内存分布？（回答内存四区，虚拟地址空间，栈内存堆内存）</font>
26. <font style="color:rgb(51, 51, 51);">在Linux系统下，使用for循环，一直进行new操作，会发生heap-overflow吗？如果不会，原因呢？（答应该不会，Linux系统可能会对此情况进行处理，面试官追问如果不用C++而用Java呢，答Java虚拟机等，胡扯了一些）</font>
27. <font style="color:rgb(51, 51, 51);">死锁的概念，进程调度算法怎么解决死锁</font>
28. <font style="color:rgb(51, 51, 51);">讲讲进程管理</font>

## <font style="color:rgb(51, 51, 51);">Linux系统编程</font>
1. <font style="color:rgb(51, 51, 51);">除了MQ和websocket之外，你还能想到什么异步通信的办法？</font>
2. <font style="color:rgb(51, 51, 51);">为什么要用多线程。多进程可以吗（webserver的）</font>
3. <font style="color:rgb(51, 51, 51);">为什么要用</font>**<font style="color:rgb(51, 51, 51);">线程池</font>**<font style="color:rgb(51, 51, 51);">，线程池中的线程是怎么运作的?</font>
4. <font style="color:rgb(51, 51, 51);">生产者消费者，信号量的使用</font>
5. <font style="color:rgb(51, 51, 51);">队列空时，消费者和生产者会发生什么</font><font style="color:rgb(51, 51, 51);">线程池请求队列是用什么实现的？（链表）</font>
6. <font style="color:rgb(51, 51, 51);">.C++多线程并发问题（场景千万级数量级怎么处理）</font>
7. <font style="color:rgb(51, 51, 51);">哪几种常见的 signal? SIGSEGV... -> 正常终止程序的信号？-> kill 进程，几号信号？</font>
8. <font style="color:rgb(51, 51, 51);">什么情况下会使用静态变量</font>
9. <font style="color:rgb(51, 51, 51);">多线程读写同一个静态变量你是怎么解决的</font>
10. <font style="color:rgb(51, 51, 51);">用过无锁编程吗，知道原子量吗</font>
11. <font style="color:rgb(51, 51, 51);">nginx 的master worker多进程模式了解吗</font>

# <font style="color:rgb(51, 51, 51);">7 网络</font>
## <font style="color:rgb(51, 51, 51);">网络原理</font>
1. <font style="color:rgb(51, 51, 51);">为什么握手是三次而挥手需要四次</font>
2. <font style="color:rgb(51, 51, 51);">tcp和udp的原理、区别、应用场景。</font>
3. **<font style="color:rgb(51, 51, 51);">TCP</font>**<font style="color:rgb(51, 51, 51);">慢启动，拥塞控制实现</font>
4. **<font style="color:rgb(51, 51, 51);">HTTP</font>**<font style="color:rgb(51, 51, 51);">是在OSI模型的哪一层</font>
5. <font style="color:rgb(51, 51, 51);">HTTPS用到的是对称加密还是非对称加密？分别体现在哪里？</font>
6. <font style="color:rgb(51, 51, 51);">http2和http1的区别</font>
7. <font style="color:rgb(51, 51, 51);">http1.0 / 1.1 / 2 / 3</font>
8. <font style="color:rgb(51, 51, 51);">get和post区别</font>
9. <font style="color:rgb(51, 51, 51);">WebSockt</font>
10. <font style="color:rgb(51, 51, 51);">tcp/ip五层模型</font>
11. <font style="color:rgb(51, 51, 51);">dns服务器用的是什么协议。</font>
12. <font style="color:rgb(51, 51, 51);">ping命令 用的是什么协议。在哪一层。</font>
13. <font style="color:rgb(51, 51, 51);">能详细讲一下有限状态机怎么解析http报文吗</font>
14. <font style="color:rgb(51, 51, 51);">如果解析http请求的时候，用户一次性没传完数据，（如果头部都没传完，请求报文长度字段都没传完，怎么办）</font>
15. <font style="color:rgb(51, 51, 51);">路由表说一下</font>
16. <font style="color:rgb(51, 51, 51);">路由表为空怎么找到下一跳</font>
17. <font style="color:rgb(51, 51, 51);">粘包拆包是什么，发生在哪一层</font>
18. <font style="color:rgb(51, 51, 51);">TCP在什么情况下会出现大量time_wait，哪个阶段出现</font>
19. <font style="color:rgb(51, 51, 51);">TCP 包头字段... 标志位-> 建立连接过程，终止连接过程-> TIME_WAIT, CLOSE_WAIT 分析，属于哪一方？</font>
20. <font style="color:rgb(51, 51, 51);">TCP 建立连接过程 -> SYN + ACK 包能不能拆开来发</font>
21. <font style="color:rgb(51, 51, 51);">讲讲quic/听说过哪些快速重传算法/timewait状态干啥用的</font>
22. <font style="color:rgb(51, 51, 51);">提到了TCP，黏包怎么解决？（固定包头接收，指定内存长度）</font>
23. <font style="color:rgb(51, 51, 51);">查看网络状况（以为是netstate，其实是ping、traceroute，紧张忘记说了）</font>
24. <font style="color:rgb(51, 51, 51);">抓包工具？（wireshark，紧张又给忘了靠）</font>
25. <font style="color:rgb(51, 51, 51);">TCP 2MSL说一下，为什么</font>

## <font style="color:rgb(51, 51, 51);">网络编程</font>
1. <font style="color:rgb(51, 51, 51);">为什么要用epoll</font>
2. <font style="color:rgb(51, 51, 51);">epoll实现原理，epoll使用的哪种模式， 除了epoll，了解select/poll吗</font>
3. <font style="color:rgb(51, 51, 51);">怎么理解多路复用机制的</font>
4. <font style="color:rgb(51, 51, 51);">reactor和proactor的好处和坏处。为什么要用reactor而不用proactor</font>
5. <font style="color:rgb(51, 51, 51);">select怎么用。底层原理</font>
6. <font style="color:rgb(51, 51, 51);">select为什么只能支持1024个。poll和epoll是怎么解决这个问题的。</font>
7. <font style="color:rgb(51, 51, 51);">epoll 底层为什么用红黑树不用hash</font>
8. <font style="color:rgb(51, 51, 51);">ET和LT的区别、IO多路复用</font>
9. <font style="color:rgb(51, 51, 51);">游戏中数据传输用啥协议（有没有改进的协议，基于UDP的可靠传输）</font>
10. <font style="color:rgb(51, 51, 51);">项目架构（webserver）两种高并发模式（问的很细）</font>
11. <font style="color:rgb(51, 51, 51);">除了Reactor模型，还有什么模型</font>

# <font style="color:rgb(51, 51, 51);">8 数据库</font>
## <font style="color:rgb(51, 51, 51);">MySQL</font>
1. <font style="color:rgb(51, 51, 51);">有哪些</font>**<font style="color:rgb(51, 51, 51);">引擎</font>**
2. <font style="color:rgb(51, 51, 51);">数据库的架构</font>
3. <font style="color:rgb(51, 51, 51);">不同引擎对</font>**<font style="color:rgb(51, 51, 51);">索引</font>**<font style="color:rgb(51, 51, 51);">的支持</font>
4. <font style="color:rgb(51, 51, 51);">InnoDB和MyISAM的区别</font>
5. <font style="color:rgb(51, 51, 51);">隔离级别</font>
6. <font style="color:rgb(51, 51, 51);">最左前缀原则</font>
7. <font style="color:rgb(51, 51, 51);">MySQL的集群是用什么样的方式去增加并发量</font>
8. <font style="color:rgb(51, 51, 51);">除了读写分离还有吗？</font>
9. <font style="color:rgb(51, 51, 51);">mysql的隔离级别和锁。</font>
10. <font style="color:rgb(51, 51, 51);">数据库delete和trancate区别(这个trancate没用过，没说出来)</font>
11. <font style="color:rgb(51, 51, 51);">mysql索引（B+树）</font>
12. <font style="color:rgb(51, 51, 51);">B树和B+树的区别</font>
13. <font style="color:rgb(51, 51, 51);">B+树树高怎么算？树高为4能支持多少数据量</font>
14. <font style="color:rgb(51, 51, 51);">数据库ACID怎么实现</font>
15. <font style="color:rgb(51, 51, 51);">binlog记录的是什么</font>
16. <font style="color:rgb(51, 51, 51);">mysql的ACLS（事务）</font>
17. <font style="color:rgb(51, 51, 51);">mysql的mvcc</font>
18. <font style="color:rgb(51, 51, 51);">mysql锁，每个锁的应用场景</font>
19. <font style="color:rgb(51, 51, 51);">什么情况下会照成死锁，举个例子</font>
20. **<font style="color:rgb(51, 51, 51);">事务</font>**<font style="color:rgb(51, 51, 51);">安全（隔离级别）</font>
21. <font style="color:rgb(51, 51, 51);">你的项目死锁怎么检测的</font>
22. <font style="color:rgb(51, 51, 51);">数据库三大范式（忘了）</font>
23. <font style="color:rgb(51, 51, 51);">如何加快数据检索的效率</font>
24. <font style="color:rgb(51, 51, 51);">.注册登陆的用户名和密码存在哪里？（数据库）</font>
25. <font style="color:rgb(51, 51, 51);">面试官灵魂4连问：乐观锁与悲观锁的概念、实现方式、场景、优缺点？</font>
26. <font style="color:rgb(51, 51, 51);">哪几种常见的 signal? SIGSEGV... -> 正常终止程序的信号？-> kill 进程，几号信号？</font>
27. <font style="color:rgb(51, 51, 51);">一千五百万行数据如何快速找到某一行数据，给出方案，设计数据库表结构</font>
28. <font style="color:rgb(51, 51, 51);">sql优化</font>
29. <font style="color:rgb(51, 51, 51);">事务</font>
30. <font style="color:rgb(51, 51, 51);">什么情况下使用读已提交</font>
31. <font style="color:rgb(51, 51, 51);">对于脏读的理解</font>
32. <font style="color:rgb(51, 51, 51);">慢查询怎么看，怎么优化</font>
33. <font style="color:rgb(51, 51, 51);">联合索引(a,b,c)，where a, b, c和where b, a, c区别</font>
34. <font style="color:rgb(51, 51, 51);">是否了解db底层</font>

## <font style="color:rgb(51, 51, 51);">redis</font>
1. <font style="color:rgb(51, 51, 51);">redis有什么数据结构</font>
2. <font style="color:rgb(51, 51, 51);">设计一个存储字符串kv对的数据结构，要考虑并发和持久化存储</font>
3. <font style="color:rgb(51, 51, 51);">redis 基本数据结构... zset-> zset 底层实现？-> skiplist 和 red-black tree 对比？</font>
4. <font style="color:rgb(51, 51, 51);">对于redis的理解</font>
5. <font style="color:rgb(51, 51, 51);">redis在项目中进行怎么样的使用</font>
6. <font style="color:rgb(51, 51, 51);">redis 为什么读取速度那么块 （io、单线程、内存）</font>
7. <font style="color:rgb(51, 51, 51);">为什么redis单线程会快 （完全基于内存、单线程避免不必要的上下文切换、cpu消耗、加锁问题。。。）</font>
8. <font style="color:rgb(51, 51, 51);">对于很多文件和数据，怎么进行数据的查找、排序，使用什么样的数据结构 （类似于TopK、这个主要是让你进行优化、类似于位图、hash、过滤器之类的）</font>

# <font style="color:rgb(51, 51, 51);">9 定时器</font>
1. <font style="color:rgb(51, 51, 51);">小根堆定时器是怎么弄的。如果一次pop一个的话。高并发情况下会不会有问题</font>
2. <font style="color:rgb(51, 51, 51);">心跳检测如何实现</font>
3. <font style="color:rgb(51, 51, 51);">为什么用小根堆实现定时器</font>

# <font style="color:rgb(51, 51, 51);">10 服务器开发</font>
1. <font style="color:rgb(51, 51, 51);">用户认证和鉴权（jwt）</font>
2. <font style="color:rgb(51, 51, 51);">从url下图片10000张图片（写了想法，代码没写出来，http的api忘了），10台机器并行下载，怎么实现（主线程给子线程分配下载任务，从线程池取（给自己挖坑））。</font>
3. <font style="color:rgb(51, 51, 51);">雪花算法原理</font>
4. <font style="color:rgb(51, 51, 51);">分布式锁</font>
5. <font style="color:rgb(51, 51, 51);">redis和MySQL数据一致性相关设计</font>
6. <font style="color:rgb(51, 51, 51);">长短连接的区别和应用场景</font>
7. <font style="color:rgb(51, 51, 51);">RAII实现数据库连接池，怎么实现的</font>
8. <font style="color:rgb(51, 51, 51);">http服务器，他的目标是什么，通过什么方式实现的</font>
9. <font style="color:rgb(51, 51, 51);">负载均衡的一些场景问题</font>
10. <font style="color:rgb(51, 51, 51);">为什么用vector实现缓冲区，有没有想过别的数据结构</font>

## <font style="color:rgb(51, 51, 51);">远程调用rpc</font>
1. <font style="color:rgb(51, 51, 51);">grpc怎么用的</font>
2. <font style="color:rgb(51, 51, 51);">为什么grpc速度快</font>
3. <font style="color:rgb(51, 51, 51);">rpc调用流程和机制，rpc超时计时器在什么位置，如果调用超时了怎么处理，当前连接还能继续使用吗</font>
4. <font style="color:rgb(51, 51, 51);">rpc调用过程种，服务端如何识别客户端是调用的哪个函数</font>
5. <font style="color:rgb(51, 51, 51);">rpc的服务寻址、数据流的序列化与反序列化和网路传输</font>

## <font style="color:rgb(51, 51, 51);">消息队列</font>
1. <font style="color:rgb(51, 51, 51);">为什么使用消息队列：解耦,异步,削峰</font>
2. **<font style="color:rgb(51, 51, 51);">如何保证高可用的</font>**
3. **<font style="color:rgb(51, 51, 51);">如何保证消息的顺序性</font>**
4. **<font style="color:rgb(51, 51, 51);">如何解决消息队列的延时以及过期失效问题？</font>**
5. <font style="color:rgb(51, 51, 51);">了解过哪些消息队列，比如kafka、zmq</font>

## <font style="color:rgb(51, 51, 51);">高性能服务</font>
1. <font style="color:rgb(51, 51, 51);">workflow高性能http框架分析</font>

## <font style="color:rgb(51, 51, 51);">注册与发现</font>
1. <font style="color:rgb(51, 51, 51);">讲下etcd的注册与发现原理</font>

# <font style="color:rgb(51, 51, 51);">11 开放性问题</font>
## <font style="color:rgb(51, 51, 51);">项目相关</font>
1. <font style="color:rgb(51, 51, 51);">介绍自己的c++项目，遇到的难点，实现了那些功能</font>
2. <font style="color:rgb(51, 51, 51);">看过源码嘛，轻量级服务器项目</font>
3. <font style="color:rgb(51, 51, 51);">计算机基础知识是怎么去补滴，之后的技术/职业规划</font>
4. <font style="color:rgb(51, 51, 51);">物联网有个简版的MQ协议叫做MQTT，你可以想一下扫码支付使用的机器，这些机器的服务器是怎么做到跟这千万级别的客户端通信的？你扫码支付完之后，服务器是怎么精准返回你这个客户端说它收到了多少钱？</font>
5. <font style="color:rgb(51, 51, 51);">情景题。手机店。不同品牌的不同型号手机有不同的业务逻辑。怎么设计系统</font>
6. <font style="color:rgb(51, 51, 51);">如果有两个服务器，一个服务器坏了，另一个服务器怎么判断并接手坏的服务器的用户数据（共用一个堆）</font>
7. <font style="color:rgb(51, 51, 51);">服务器进行过压测么</font>
8. <font style="color:rgb(51, 51, 51);">场景设计问题，UDP设计安全可靠的文件传输</font>
9. <font style="color:rgb(51, 51, 51);">客户端资源下载到一半突然网络中断怎么办，有进行处理吗？</font>
10. <font style="color:rgb(51, 51, 51);">有进行过压力测试吗？</font>
11. <font style="color:rgb(51, 51, 51);">在学校里或者公司中最有成就感的事。</font>
12. <font style="color:rgb(51, 51, 51);">井盖为什么是圆的</font>

# <font style="color:rgb(51, 51, 51);">12 音视频相关</font>
1. <font style="color:rgb(51, 51, 51);">音视频的编解码和同步问题能讲一下吗</font>
2. <font style="color:rgb(51, 51, 51);">你遇到过解码卡顿的情况吗</font>

# <font style="color:rgb(51, 51, 51);">13 参考文章</font>
<font style="color:rgb(51, 51, 51);">部分参考文章：</font>

[<font style="color:rgb(65, 131, 196);">https://www.nowcoder.com/feed/main/detail/61c62b0d97974a93a77030aeb278f880</font>](https://www.nowcoder.com/feed/main/detail/61c62b0d97974a93a77030aeb278f880)[<font style="color:rgb(65, 131, 196);">https://www.nowcoder.com/discuss/501752554045308928</font>](https://www.nowcoder.com/discuss/501752554045308928)[<font style="color:rgb(65, 131, 196);">https://www.nowcoder.com/feed/main/detail/a4bcfe4ed24247019cbdbd176c2cb0b8</font>](https://www.nowcoder.com/feed/main/detail/a4bcfe4ed24247019cbdbd176c2cb0b8)[<font style="color:rgb(65, 131, 196);">https://www.nowcoder.com/feed/main/detail/7b2e7b35e3ff4893aa2623b761103f15</font>](https://www.nowcoder.com/feed/main/detail/7b2e7b35e3ff4893aa2623b761103f15)[<font style="color:rgb(65, 131, 196);">https://www.nowcoder.com/feed/main/detail/b11ab7e902324190a96bd33c79b1f8c1</font>](https://www.nowcoder.com/feed/main/detail/b11ab7e902324190a96bd33c79b1f8c1)

<font style="color:rgb(51, 51, 51);">还有其他的文章太多了，如果侵权请告知删除。</font>



> 更新: 2025-04-19 15:30:37  
> 原文: <https://www.yuque.com/linuxer/gscfv1/huyibf1tp6v92ngq>