# 9-Java安装



**<font style="color:#DF2A3F;">Java环境不是必须要的，这里只是提供参考。没有用到Java时不要安装。</font>**



1. <font style="color:rgb(17, 17, 17);">打开终端，输入以下命令安装  </font>

```plain
sudo apt-get update sudo 
apt-get install default-jdk 
```

2. <font style="color:rgb(17, 17, 17);">验证 Java 是否成功安装，输入：</font>

```plain
java -version
```

3. <font style="color:rgb(17, 17, 17);">设置 Java 环境变量，在终端输入：</font>

```plain
sudo vim /etc/profile
```

<font style="color:rgb(17, 17, 17);">在打开的文件末尾添加  </font>

```plain
export JAVA_HOME=/usr/lib/jvm/default-java 
export PATH=$PATH:$JAVA_HOME/bin
```

![1716618128442-8f2d97f7-10ea-4dbc-979c-1619db408361.png](./img/44549646_lk2gkGYIK-VXH1J-/1716618128442-8f2d97f7-10ea-4dbc-979c-1619db408361-806836.png)

4. <font style="color:rgb(17, 17, 17);">保存文件并退出。</font>
5. <font style="color:rgb(17, 17, 17);">重新加载环境变量，输入 </font>

```plain
source /etc/profile
```



> 更新: 2024-05-31 15:05:56  
> 原文: <https://www.yuque.com/linuxer/gscfv1/gs947t82p25cor98>