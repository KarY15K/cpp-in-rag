# Linux 静态库(.a)转换为动态库(.so)

Linux 静态库转换为动态库



首先将.a文件转为.so文件是可以实现的

原因是：.a 文件其实是 .o 文件的压缩包，所以你需要去解压缩，然后再生成 .so 就可以了

 

举例:

编辑 test.c

```plain
#include <string.h>
void test(char *str)
{
    strcpy (str, "haha");
}
```



 

1 生成静态库：libtest.a

```plain
1.生成 .o 文件，这里必须加上 -fpic 否则后面生成 so 是会报错
gcc -fpic -c test.c
2.生成 .a 文件
ar -r libtest.a test.o
```



 

2 开始将 .a 转换为 .so:libtest.so

```plain
1.分离出 test.o
ar -x libtest.a
2.编译生成 .so 文件 
gcc -shared *.o -o libtest.so
```



3 查看导出函数

<font style="background-color:rgb(245, 245, 245);">nm -D libtest.so</font>



可以看到 test() 函数已经导出

 

4 使用 libtest.so

编辑 main.c

```c
#include <stdlib.h>
#include <stdio.h>
int main ( int argc, char *argv[] )
{
    char buf[32] = {};
    test(buf);
    printf ("buf is:%s\n", buf);
    return 0;
}
```

 

编译连接

<font style="background-color:rgb(245, 245, 245);">gcc main.c -ltest -L./</font>



设置运行的环境变量

<font style="background-color:rgb(245, 245, 245);">export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:./</font>

运行

./a.out



> 更新: 2024-07-04 21:46:04  
> 原文: <https://www.yuque.com/linuxer/gscfv1/uc5qoncvbqqv2vn0>