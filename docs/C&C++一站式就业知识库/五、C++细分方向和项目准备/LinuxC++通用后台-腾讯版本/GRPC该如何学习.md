# GRPC该如何学习

<font style="color:#117CEE;">老廖提供主流C/C++就业课程服务：</font>

1. <font style="color:#117CEE;">C/C++通用后台开发</font>
2. <font style="color:#117CEE;">音视频开发</font>
3. <font style="color:#117CEE;">QT开发</font>
4. <font style="color:#117CEE;">游戏开发</font>

<font style="color:#117CEE;">等课程，包括视频教学、资料代码、课程答疑、简历指导、面试复盘等服务，详情咨询</font>**<font style="color:#117CEE;">微信laoliao6668</font>**

**<font style="color:#117CEE;">建议案例参考（点击链接即可）：</font>**

+ [<font style="color:#DF2A3F;">音视频方向就业案例分析</font>](https://www.yuque.com/linuxer/ks4fip/nwb4zib3lw4h3t82?singleDoc)
+ [<font style="color:#DF2A3F;">Linux后台开发就业案例分析</font>](https://www.yuque.com/lingshengxueyuan/0voice/rw8cgz#lmRCO)

<font style="color:#DF2A3F;"></font>

<font style="color:rgb(6, 6, 7);">gRPC是一种高性能、开源和通用的RPC（远程过程调用）框架，由Google主导开发。它允许你使用协议缓冲区（Protocol Buffers）作为接口定义语言来定义服务，并自动生成跨多种语言的服务端和客户端代码。以下是gRPC C++入门的步骤：</font>

1. **<font style="color:rgb(6, 6, 7);">安装gRPC和Protocol Buffers</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">在开始之前，需要在你的开发环境中安装gRPC和Protocol Buffers。可以参考gRPC官方文档</font>[<font style="color:rgb(6, 6, 7);">https://grpc.io/docs/languages/cpp/quickstart/</font>](https://grpc.io/docs/languages/cpp/quickstart/)
2. **<font style="color:rgb(6, 6, 7);">理解核心概念</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">学习gRPC的核心概念，包括RPC、服务定义、存根、服务器和客户端等。参考：</font>[<font style="color:rgb(6, 6, 7);">https://blog.csdn.net/sinat_38816924/article/details/131448161</font>](https://blog.csdn.net/sinat_38816924/article/details/131448161)
3. **<font style="color:rgb(6, 6, 7);">定义服务</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">使用Protocol Buffers语法在你的</font>.proto<font style="color:rgb(6, 6, 7);">文件中定义你的服务接口和消息类型。每个RPC都由一个请求和一个响应消息定义。</font>
4. **<font style="color:rgb(6, 6, 7);">生成代码</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">使用</font>protoc<font style="color:rgb(6, 6, 7);">编译器和gRPC插件为C++生成服务器和客户端代码。Makefile</font>5<font style="color:rgb(6, 6, 7);">可以帮助你编译源代码和生成必要的</font>.pb.h<font style="color:rgb(6, 6, 7);">和</font>.pb.cc<font style="color:rgb(6, 6, 7);">文件。</font>
5. **<font style="color:rgb(6, 6, 7);">编写服务器代码、客户端代码</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">创建一个服务端实现类，该类需要实现你在</font>.proto<font style="color:rgb(6, 6, 7);">文件中定义的所有RPC方法。参考：</font>[<font style="color:rgb(6, 6, 7);">https://blog.csdn.net/lihao21/article/details/104138126</font>](https://blog.csdn.net/lihao21/article/details/104138126)
6. **<font style="color:rgb(6, 6, 7);">编译和运行</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">使用编译工具（如CMake）来编译你的服务端和客户端代码，并运行它们以验证它们之间的通信。</font>
7. **<font style="color:rgb(6, 6, 7);">异步编程</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">学习如何使用gRPC的异步API来提高性能，特别是在高并发场景下。gRPC官方文档</font>24<font style="color:rgb(6, 6, 7);">提供了异步编程的示例。</font>
8. **<font style="color:rgb(6, 6, 7);">进阶特性</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">学习更高级的特性，如流式RPC、认证和授权、拦截器等。</font>
9. **<font style="color:rgb(6, 6, 7);">实践和示例</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">通过实践来加深理解，可以查看gRPC官方提供的示例</font>1<font style="color:rgb(6, 6, 7);">，或者在CSDN博客</font>5<font style="color:rgb(6, 6, 7);">中找到入门教程和示例。</font>
10. **<font style="color:rgb(6, 6, 7);">阅读文档和教程</font>**<font style="color:rgb(6, 6, 7);">：</font>
    - <font style="color:rgb(6, 6, 7);">阅读gRPC的官方文档和在线教程，以获得更深入的理解和指导 </font>[<font style="color:rgb(6, 6, 7);">https://grpc.io/docs/languages/cpp/quickstart/</font>](https://grpc.io/docs/languages/cpp/quickstart/)<font style="color:rgb(6, 6, 7);">。</font>

<font style="color:rgb(6, 6, 7);">通过遵循这些步骤，你将能够构建起gRPC C++开发的基础知识，并能够创建简单的服务端和客户端应用程序。记住，实践是学习的关键，所以尝试构建自己的项目并解决实际问题将非常有用。</font>



> 更新: 2024-04-19 22:34:50  
> 原文: <https://www.yuque.com/linuxer/gscfv1/ooc8g2ch90zfv1ln>