# 第04章 Linux缓存IO

**<font style="color:blue;background-color:#FFFF00;">B站 程序员老廖</font>** **编辑整理**， **需要配套代码加我微信**：**<font style="color:red;background-color:#FFFF00;">laoliao6668</font>** **获取。**



**<font style="color:red;background-color:#FFFF00;">一站式从0快速学习Linux编程，比视频更高效的Linux图文学习教程。</font>** 





# 4 Linux缓存I/O
**块 (block) 是文件系统中最小存储单元的抽象**。在内核中，所有的文件系统操作都是基于块来执行的。实际上，块是I/O中的基本概念。 因此，所有I/O 操作都是在块大小或者块大小的整数倍上执行，也就是说，也许你 只想读取一个字节，实际上需要读取整个块。想写4.5个块的数据?你需要写5个块的数据，也就是说读取最后一块整块的数据，更新(删掉)后半部分内容，然后 再把整个块写出去。



你很快会发现这个问题：对非整数倍块大小的操作效率很低。操作系统需要对I/O 进行“修补”,确保所有操作都是在块大小整数倍上执行，并且和下一个最大块对 齐。问题在于，用户空间的应用在实现时并不会考虑到块的概念。绝大多数应用都 是在更高层抽象上执行的，比如成员变量和字符串，其大小变化和块大小无关。最 糟糕的是，用户空间应用可能每次只读写一个字节!这会带来很多不必要的开销。 实际上，对于每次写一个字节的应用，实际上往往也是要写整个块。



额外的系统调用所带来的开销会导致操作性能急剧下降。举个例子，假设要读取 1024个字节，如果每次读一个字节需要执行1024次调用，而如果一个读取1024 字节的块则只需要调用一次。对于前一种，提升其性能的途径是“用户缓冲I/O”  (user-buffered   I/O),通过缓冲I/O, 从用户角度，读写数据并没有任何变化，而实际上，只有当数据量大小达到文件系统块大小整数倍时，才会执行真正的I/O 操作。

# 1 用户缓冲I/O
需要对普通文件执行很多轻量级I/O 请求的程序通常会采用用户缓冲 I/O。**用户缓 冲I/O 是在用户空间**而不是内核中完成的，**它可以在应用程序中设置，也可以调用标准库**，对用户而言“透明”执行。 出于性能考虑，**内核通过延迟写、合并相邻I/O请求以及预读等操作来缓冲数据**。通过不同的方式， 用户缓冲也是旨在提升性能。



以用户空间程序dd 的使用为例：

```plain
lqf@ubuntu:~$ dd   bs=1   count=2097152   if=/dev/zero   of=pirate
```



执行结果：

> 2097152+0 records in  
2097152+0 records out  
2097152 bytes (2.1 MB, 2.0 MiB) copied, **4.44459 s, 472 kB/s**
>



由于设置了参数 bs=1,dd 命令会从设备/dev/zero(提供值全为0的文件流的虚拟设 备)中拷贝2MB的数据到文件 pirate 中，每次操作拷贝一个字节，共执行2097152 次操作。也就是说，该程序大约会执行两百万次的读写操作：每次一个字节。

**命令功能概述**

`dd` 是一个在 Linux 和类 Unix 系统中非常强大的命令，主要用于复制文件并对数据进行转换和格式化。由于设置了参数 bs=1,dd 命令会从设备/dev/zero(提供值全为0的文件流的虚拟设 备)中拷贝2MB的数据到文件 pirate 中，每次操作拷贝一个字节，共执行2097152 次操作。也就是说，该程序大约会执行两百万次的读写操作：每次一个字节。

**命令参数详解**

+ `bs=1`：`bs` 是 `block size` 的缩写，即块大小。这里将块大小设置为 1 字节，表示每次从输入文件（`if` 指定）读取和向输出文件（`of` 指定）写入的数据量为 1 字节。在实际应用中，为了提高读写效率，通常会设置较大的块大小。
+ `count=2097152`：`count` 参数指定 `dd` 命令要复制的块数。结合前面的 `bs=1`，此命令会复制 2097152 个 1 字节的块，也就是总共复制 2097152 字节的数据。由于 1MB = 1024 * 1024 字节，所以 2097152 字节恰好是 2MB。
+ `if=/dev/zero`：`if` 是 `input file` 的缩写，用于指定输入文件的路径。`/dev/zero` 是一个特殊的设备文件，它会不断地返回 ASCII 码为 0 的字节（即空字符 `'\0'`）。当你从 `/dev/zero` 读取数据时，它可以提供任意数量的零值字节。
+ `of=pirate`：`of` 是 `output file` 的缩写，用于指定输出文件的路径。这里将输出文件命名为 `pirate`，如果该文件不存在，`dd` 会创建它；如果文件已存在，则会覆盖原有内容。





再来看看另一种使用方式，同样是拷贝2MB的数据，每次操作1024字节的数据块 

```plain
lqf@ubuntu:~$ dd bs=1024 count=2048 if=/dev/zero of=pirate

```

执行结果：

> 2048+0 records in  
2048+0 records out  
2097152 bytes (2.1 MB, 2.0 MiB) copied, 0.00664102 s, 316 MB/s
>



该操作也是拷贝相同的2MB 字节数据到相同的文件中，但是和前一种方式相比， 它的读写操作次数仅仅是前一种的1/1024，**其性能提升是非常明显的。



# 2 块大小
实际应用中，块大小一般是**512字节、1024字节、2048字节或4096字节**。内核和硬件之间的交互单元是块，因此，使用块大小或者块大小的约数可以保证I/O 请求是块对齐的，可以避免内核内其他冗余操作。 

I/O 操作最简单的方式是设置一个较大的缓冲区大小，**是标准块大小的整数倍**，比如设置成**4096**或**8192**,性能都不错。 



那么,每次I/O请求都处理4KB或 8KB的块大小，性能一定就会很高吗?不一定。 问题在于程序很少是以块为单位处理的。程序处理的是变量、行和字符，而不是像块这样的抽象单元。

因此，应用使用自己的抽象结构，**通过用户缓冲I/O**, 实现块操作。其工作原理很简单，却很有效：

+ 当数据被写入时，它会被存储到程序地址空间的缓冲区中。当缓冲区数据大小达到给定值，即缓冲区大小 (buffer    size),整个缓冲区会通过一次写操作全部写出。
+ 同样，读操作也是一次读入缓冲区大小且块对齐的数据。应用的各种大小不同的读请求不是直接从文件系统读取，而是从缓冲区中一块块读取。应用读取的数据越来越多，缓冲区一块块给出数据。最后，当缓冲区为空时，会读取另一个大的块对齐的数据。
+ 通过这种方式，虽然应用设置的读写大小很不合理，数据会从缓冲区中获取，因此对文件系统还是发送大的块对齐的读写请求。其最终结果是对于大量数据，系统调用次数更少，且每次请求的数据大小都是块对齐的，通过这种方式，可以确保有很大的性能提升。



用户可以在自己的程序中实现缓冲，实际上很多关键应用就是自己实现了用户缓 冲。不过，大部分程序可以通过：

+ 标准 I/O库 (**C 标准库**)
+ 或iostream库 (**C++标准库**)来实现

它们实现了健壮有效的用户缓冲方案。



# 3 标准I/O
C标准库中提供了标准I/O库(简称stdio),它实现了跨平台的用户缓冲解决方案。这个标准I/O库使用简单，且功能强大。



本章的剩余部分将会讨论用户缓冲I/O,因为它和文件I/O相关，而且是在C标准库中实现：即通过C标准库完成打开、关闭和读写操作。应用是使用**标准I/O**(可定制用户缓冲方式)，还是直接使用**系统调用**，这些都需要开发人员慎重权衡应用的需求和行为后才能确定。



C标准通常会把细节留给每个实现本身，而具体的实现则通常又会加入一些额外特性。本章以及本书的其他章节，主要探讨现代Linux系统的接口和行为，这些接口和行为主要是通过glibc实现的。



# 4 文件指针
标准I/O 程序集并不是直接操作文件描述符。相反，它们通过唯一标识符，即文**件指针 (file pointer) 来操作**。

在C 标准库里，文件指针和文件描述符一一映射。文 件指针是由指向类型定义FILE 的指针表示，FILE 类型定义在<stdio.h>中。

在标准I/O 中，打开的文件称为“流”(**stream**) 。 流可以被打开用来读(输入流)、写(输出流)或者二者兼有(输入/输出流)。



# 5 打开文件
## 5.1 打开文件fopen
文件是通过fopen()  打开以供读写操作： 

```c
#include <stdio.h>

FILE *fopen(const char *filename, const char *mode);
```



该函数根据mode 参数，按指定模式打开 path 所指向的文件，并给它关联上新的流。

**参数解析：**

+ `filename`：指向要打开的文件名的字符串指针。这是一个以空字符结尾的字符串，指定了文件的路径和名称。你需要确保提供的文件名是正确有效的，并且你有足够的权限来访问该文件。
+ `mode`：指向指定文件打开模式的字符串指针。常见的打开模式有：
    - `"r"`：以只读方式打开文件。如果文件不存在，则打开失败。流指针指向文件的开始。
    - `"r+"`：以读写方式打开文件。文件必须存在。流指针指向文件的开始。
    - `"w"`：以只写方式打开文件。如果文件存在，则截断文件长度为 0；如果文件不存在，则创建新文件。
    - `"w+"`：以读写方式打开文件。如果文件存在，则截断文件长度为 0；如果文件不存在，则创建新文件。
    - `"a"`：以追加写方式打开文件。如果文件不存在，则创建新文件。写入的数据会被追加到文件的末尾。
    - `"a+"`：以追加读写方式打开文件。如果文件不存在，则创建新文件。读取从文件开头开始，写入的数据会被追加到文件的末尾。

 **返回值解析**：

+ 返回值是一个`FILE *`类型的指针。如果打开文件成功，该指针指向一个`FILE`结构体，这个结构体包含了与文件相关的信息，如文件描述符、缓冲区等。你可以使用这个指针来进行后续的文件操作，如读取、写入、关闭等。
+ 如果打开文件失败，则返回`NULL`，并相应设 置errno 值。可能的失败原因包括文件不存在、没有足够的权限、磁盘故障等。在使用`fopen`函数后，应该始终检查返回值，以确保文件成功打开。



fopen应用范例：

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main() {
    FILE *fp;
    char filename[] = "test.txt";
    char mode[] = "r";

    fp = fopen(filename, mode);
    if (fp == NULL) {
        // 将错误码转换为字符串
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("无法打开文件 %s，错误信息：%s\n", filename, errorMsg);
    } else {
        printf("成功打开文件 %s。\n", filename);
        // 进行文件操作
        //...
        fclose(fp);
    }

    return 0;
}
```

		在这个例子中，首先尝试以只读模式打开文件 `"test.txt"`。如果打开失败，通过 `strerror_r` 函数将错误码 `errno` 转换为错误信息字符串，并输出错误原因。如果打开成功，则输出成功信息，并可以在后续进行文件的读取、写入等操作。





## 5.2 通过文件描述符打开流fdopen
函数fdopen()会把一个已经打开的文件描述符 (fd) 转换成流： 

```c
#include  <stdio.h>

FILE  *fdopen(int fd,const char  *mode);
```



fdopen()的可能模式和fopen()相同，而且必须和初始打开文件描述符的模式匹配。 

可以指定模式w 和 w+, 但是它们不会清空原文件。

流指针指向文件描述符指向的文件位置。



一旦文件描述符被转换成流，**则在该文件描述符上不应该直接执行I/O 操作**，虽然这么做是合法的。需要注意的是，文件描述符并没有被复制，而只是关联了一个新的流。**关闭流也会关闭相应的文件描述符**。



fdopen()执行成功时，返回一个合法的文件指针；失败时，返回NULL 并设置相应 的 errno 值。



举个例子，以下代码通过open()系统调用打开test.txt文件，然后通过该文件描述符创建一个关联的流：

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>

int main() {
    int fd = open("test.txt", O_RDONLY);
    if (fd == -1) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("无法打开文件 test.txt，错误信息：%s\n", errorMsg);
        return 1;
    }

    FILE *fileStream = fdopen(fd, "r");
    if (fileStream == NULL) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("无法将文件描述符转换为文件流，错误信息：%s\n", errorMsg);
        close(fd);
        return 1;
    }

    char buffer[100];
    if (fgets(buffer, sizeof(buffer), fileStream)!= NULL) {
        printf("读取到的内容：%s", buffer);
    } else {
        perror("读取文件失败");
    }

    fclose(fileStream);

    return 0;
}
```

在这个例子中，首先使用 `open` 函数打开一个文件，如果打开失败，将错误码转换为错误信息字符串并输出错误原因。然后使用 `fdopen` 将文件描述符转换为文件流指针，如果转换失败，同样将错误码转换为错误信息字符串并输出错误原因。如果转换成功，尝试从文件流中读取内容并输出。最后，关闭文件流。





# 6 关闭流
fclose()函数会关闭给定的流： 

```c
#include <stdio.h>

int fclose(FILE *stream);
```

在关闭前，所有被缓冲但还没有写出的数据都会被写出。fclose() 成功时，返回0。 失败时，返回EOF 并且相应地设置errno 值。



# 7 从流中读数据
了解了如何打开关闭流之后，现在我们来看一些有用的：

+ 先从一个流中读数据，再把数据写到另一个流中。



C 标准库实现了多种从流中读取数据的方法，有很常见的，也有不常用的。本节会 考察其中最常用的三种：

+ 每次读取一个字节
+ 每次读取一行
+ 以及读取二进制数据。

为了从流中读取数据，该流必须以适当模式打开成输入流，也就是说，除了w 或 a 之外的任何模式都可以。



## 7.1 每次读取一个字节
### 7.1.1 fgetc读取一字节
通常情况下，理想的I/O 模式是每次读取一个字符。函数fgetc() 可以用来从流中读 取单个字符：

```c
#include <stdio.h>

int fgetc(FILE *stream);
```

该函数从 stream 中读取一个字符，并把该字符强制类型转换成unsigned int返回。 强制类型转换是为了能够表示文件结束或错误：在这两种情况下都会返回 EOF。

fgetc()的返回值必须保存成int 类型。把返回值保存为char 类型是经常犯的危险错 误，因为这么做会漏掉错误检查。



下面这个例子会从stream 中读取一个字符，检查错误，然后以字符方式打印结果：

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fp;
    char filename[] = "test.txt";

    fp = fopen(filename, "r");
    if (fp == NULL) {
        perror("无法打开文件");
        return 1;
    }

    int ch = fgetc(fp);
    if (ch == EOF) {
        if (ferror(fp)) {
            perror("读取文件时发生错误");
        } else if (feof(fp)) {
            printf("已到达文件末尾，没有更多字符可读。\n");
        }
    } else {
        printf("读取到的字符是：%c\n", (char)ch);
    }

    fclose(fp);

    return 0;
}
```

在这个程序中，首先尝试打开指定的文件。然后使用`fgetc`读取一个字符。如果读取到的字符是`EOF`，则进一步检查是因为错误还是到达文件末尾。如果是错误，使用`perror`打印错误信息；如果是到达文件末尾，则输出相应的提示信息。如果成功读取到一个字符，则将其以字符形式打印出来。最后关闭文件





### 7.1.2 ungetc把字符放回到流中
标准输入输出提供了一个函数可以把字符放回到流中。当读取流的最后一个字符， 如果不需要该字符的话，可以把它放回流中。

```c
#include <stdio.h>

int  ungetc(int  c, FILE  *stream);
```

**参数解析**：

+ `c`：要推回到输入流的字符。这个字符可以是任何有效的字符值，包括 EOF（但通常不应该将 EOF 推回，因为它用于表示文件结束）。
+ `stream`：指向要推回字符的输入流的`FILE`结构体指针。这通常是通过`fopen`等函数打开的文件流，或者是标准输入流`stdin`等。

**返回值解析**：

+ 如果成功地将字符推回到输入流，返回被推回的字符。这意味着如果后续从该输入流读取字符，将首先读取到被推回的字符。
+ 如果发生错误，例如输入流不支持推回操作或者出现其他错误情况，返回`EOF`。

需要注意的是，不是所有的输入流都支持推回操作，例如某些只读文件流或者从管道、套接字等创建的流可能不支持推回。此外，推回的字符数量通常是有限制的，具体取决于实现。一般来说，可以安全地推回一个字符，但多次推回可能会导致不确定的行为。



`ungetc` 函数的范例

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main() {
    FILE *fp;
    char filename[] = "test.txt";

    // 以只读方式打开文件
    fp = fopen(filename, "r");
    if (fp == NULL) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("无法打开文件 %s，错误信息：%s\n", filename, errorMsg);
        return 1;
    }

    int ch = fgetc(fp);
    if (ch == EOF) {
        if (ferror(fp)) {
            char errorMsg[100];
            strerror_r(errno, errorMsg, sizeof(errorMsg));
            printf("读取文件时发生错误：%s\n", errorMsg);
            fclose(fp);
            return 1;
        } else if (feof(fp)) {
            printf("已到达文件末尾，没有更多字符可读。\n");
            fclose(fp);
            return 0;
        }
    }

    // 将读取到的字符推回到输入流
    int result = ungetc(ch, fp);
    if (result == EOF) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("将字符推回到输入流时发生错误：%s\n", errorMsg);
        fclose(fp);
        return 1;
    }

    // 再次读取字符，应该是刚才推回的字符
    ch = fgetc(fp);
    if (ch!= EOF) {
        printf("再次读取到的字符是：%c\n", (char)ch);
    } else {
        if (ferror(fp)) {
            char errorMsg[100];
            strerror_r(errno, errorMsg, sizeof(errorMsg));
            printf("再次读取文件时发生错误：%s\n", errorMsg);
        } else if (feof(fp)) {
            printf("已到达文件末尾，没有更多字符可读。\n");
        }
    }

    fclose(fp);

    return 0;
}
```

在这个例子中，首先打开文件，读取一个字符，然后尝试将该字符推回到输入流。如果推回成功，再次读取字符并输出。如果在任何过程中发生错误，将错误码转换为字符串并输出错误信息。





## 7.2 每次读一行
函数 fgets()  会从指定流中读取一个字符串： 

**函数原型：**

```c
#include <stdio.h>

char   *fgets(char   *str,int   size,FILE   *stream);
```

**参数解析**：

+ `s`：指向字符数组的指针，用于存储读取到的字符串。这个字符数组必须有足够的空间来存储读取的字符串以及结尾的空字符。
+ `n`：指定要读取的最大字符数。这个值通常应该比字符数组的大小小 1，以留出空间存储结尾的空字符。如果在读取到`n - 1`个字符之前遇到换行符`'\n'`或者文件结束标志`EOF`，读取将提前结束。
+ `stream`：指向要读取的文件流的`FILE`结构体指针。这通常是通过`fopen`等函数打开的文件流，或者是标准输入流`stdin`等。

**返回值解析**：

+ 如果成功读取到一行字符串（包括换行符），返回`s`，即指向存储字符串的字符数组的指针。
+ 如果在读取过程中遇到文件结束标志`EOF`且没有读取到任何字符，返回`NULL`。
+ 如果发生错误，也返回`NULL`。此时可以通过检查`errno`变量来确定具体的错误原因。

例如，如果要从文件中逐行读取并处理内容，可以这样使用`fgets`：

```c
#include <stdio.h>

int main() {
    FILE *fp;
    char buffer[100];

    fp = fopen("test.txt", "r");
    if (fp == NULL) {
        perror("无法打开文件");
        return 1;
    }

    while (fgets(buffer, sizeof(buffer), fp)!= NULL) {
        // 对读取到的每一行进行处理
        printf("%s", buffer);
    }

    fclose(fp);

    return 0;
}
```



## 7.3 读二进制文件
对于某些应用，每次读取一个字符或一行是不够的。在某些场景下，开发人员希望读写复杂的二进制数据，比如C 结构体。为了解决这个entire,标准I/O库提供了 fread()函数：

**函数原型：**

```c
#include <stdio.h>

size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
```

**参数解析**：

+ `ptr`：指向存储读取数据的内存缓冲区的指针。这个缓冲区必须有足够的空间来存储读取的数据。
+ `size`：指定每个数据元素的大小（以字节为单位）。例如，如果要读取整数，`size`可能是`sizeof(int)`。
+ `nmemb`：指定要读取的数据元素的数量。
+ `stream`：指向要读取的文件流的`FILE`结构体指针。这通常是通过`fopen`等函数打开的文件流，或者是标准输入流`stdin`等。

**返回值解析**：

+ 返回实际读取到的数据元素的数量。如果这个值小于`nmemb`，可能是因为到达了文件末尾或者发生了错误。
+ 如果返回值为 0，并且`feof(stream)`返回非零值，说明已经到达了文件末尾。
+ 如果返回值为 0，并且`ferror(stream)`返回非零值，说明在读取过程中发生了错误。

例如，以下是一个使用`fread`从文件中读取整数的例子：

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fp;
    int buffer[10];
    size_t elementsRead;

    fp = fopen("data.bin", "rb"); //b意思是读取二进制方式
    if (fp == NULL) {
        perror("无法打开文件");
        return 1;
    }

    elementsRead = fread(buffer, sizeof(int), 10, fp);
    if (elementsRead < 10) {
        if (feof(fp)) {
            printf("到达文件末尾，实际读取到 %ld 个整数。\n", elementsRead);
        } else if (ferror(fp)) {
            perror("读取文件时发生错误");
        }
    }

    fclose(fp);

    // 处理读取到的整数
    for (size_t i = 0; i < elementsRead; i++) {
        printf("%d ", buffer[i]);
    }

    return 0;
}
```





# 8 向流中写数据
和读相同，C 标准库定义了一些向打开的流中写数据的函数。本节将介绍三个最常 用的写数据的方法：

+ 每次写一个字节，
+ 每次写一个字符串，
+ 和写二进制数据。

这些不同的写方法很适合**采用缓冲I/O**。为了向流中写数据，必须**以适当的输出模式打开流**，也就是说，除了模式r 之外的所有合法模式。



## 8.1 写入单个字符
和 fgetc()函数相对应的是 fputc(): 

**函数原型：**

```c
#include <stdio.h>

int fputc(int c, FILE *stream); 
```

**参数解析**：

+ `c`：要写入的字符。可以是任何有效的字符值，包括 ASCII 字符和扩展字符集。
+ `stream`：指向要写入字符的文件流的`FILE`结构体指针。这通常是通过`fopen`等函数打开的文件流，或者是标准输出流`stdout`等。

**返回值解析**：

+ 如果成功写入字符，返回写入的字符值。
+ 如果发生错误，返回`EOF`。

以下是一个使用`fputc`的范例：

```plain
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main() {
    FILE *fp;
    char filename[] = "output.txt";

    // 以写入模式打开文件
    fp = fopen(filename, "w");
    if (fp == NULL) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("无法打开文件 %s，错误信息：%s\n", filename, errorMsg);
        return 1;
    }

    int ch = fputc('H', fp);
    if (ch == EOF) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("写入字符时发生错误：%s\n", errorMsg);
        fclose(fp);
        return 1;
    }

    ch = fputc('e', fp);
    if (ch == EOF) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("写入字符时发生错误：%s\n", errorMsg);
        fclose(fp);
        return 1;
    }

    // 继续写入其他字符

    fclose(fp);

    printf("成功写入字符到文件 %s。\n", filename);

    return 0;
}
```



这个例子会把字符p 写到stream 中，而且stream 必须以可写模式打开。



## 8.2 写入字符串
函数 fputs()用于向指定流中写入整个字符串。

**函数原型**：

```c
#include <stdio.h>

int fputs(const char *s, FILE *stream);
```

fputs() 调用会把str 指向的所有字符串都写入 stream 指向的流中，不会写入结束标 记符。成功时，返回一个非负整数；失败时，返回EOF。

**参数解析**

+ const char *s：
    - 这是一个指向要写入的字符串的指针。该字符串必须以终止符（'\0'）结尾。
    - 你可以传入一个字符串常量，也可以传入一个字符数组的首地址。
+ FILE *stream：
    - 这是一个指向文件流的指针，指定了要写入的目标文件。这个文件流可以是通过`fopen`函数打开的文件，或者是标准输入 / 输出 / 错误流（`stdin`、`stdout`、`stderr`）。

**返回值解析**

返回值为`int`类型：

+ 如果成功写入，函数将返回一个非负值，通常情况下返回写入的字符数量（不包括 null 终止符）。
+ 如果发生错误，函数将返回`EOF`。



以下是一个在 Linux 下使用 `fputs` 函数的范例

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main() {
   FILE *fp;
   char filename[] = "output.txt";
   char str[] = "Hello, world!";

   // 以写入模式打开文件
   fp = fopen(filename, "w");
   if (fp == NULL) {
       char errorMsg[100];
       strerror_r(errno, errorMsg, sizeof(errorMsg));
       printf("无法打开文件 %s，错误信息：%s\n", filename, errorMsg);
       return 1;
   }

   int result = fputs(str, fp);
   if (result == EOF) {
       char errorMsg[100];
       strerror_r(errno, errorMsg, sizeof(errorMsg));
       printf("写入字符串时发生错误：%s\n", errorMsg);
       fclose(fp);
       return 1;
   }

   fclose(fp);

   printf("成功写入字符串到文件 %s。\n", filename);

   return 0;
}
```





## 8.3 写入二进制数据
如果程序需要写入复杂的数据，单字符或行可能会截断数据。为了直接存储如 C 变量这样的二进制数据，标准I/O 提供了fwrite()函数：

**函数原型：**

```c
#include <stdio.h>

 size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
```

调用fwrite() 会把ptr指向的size个数据项写入到stream 中，每个数据项长为size。 文件指针会向前移动写入的所有字节的长度。

 **参数解析**

+ const void *ptr：
    - 这是一个指向要写入数据的指针。可以是任何数据类型的指针，因为它被声明为`const void *`，表示可以指向任意类型的数据。
    - 通常情况下，你会传入一个指向数组、结构体或其他数据结构的指针。
+ size_t size：
    - 表示每个数据项的大小，以字节为单位。
    - 例如，如果要写入整数数组，`size`可以设置为`sizeof(int)`；如果要写入字符数组，`size`可以设置为`sizeof(char)`。
+ size_t nmemb：
    - 表示要写入的数据项的数量。
    - 例如，如果要写入一个包含 10 个整数的数组，`nmemb`可以设置为 10。
+ FILE *stream：
    - 这是一个指向文件流的指针，指定了要写入的目标文件。这个文件流可以是通过`fopen`函数打开的文件，或者是标准输入 / 输出 / 错误流（`stdin`、`stdout`、`stderr`）

**返回值解析**：

返回值为`size_t`类型，表示成功写入的**元素数量**（注意不是字节大小）。如果返回值小于`nmemb`，则表示发生了错误或者部分写入。

例如，如果要写入一个包含 10 个整数的数组，并且成功写入了所有整数，`fwrite`将返回 10。如果只写入了 5 个整数，可能是由于文件空间不足或其他错误，返回值将为 5。

在使用`fwrite`函数时，需要注意以下几点：

1. 确保文件流是有效的，并且有足够的权限进行写入操作。
2. 检查返回值以确定是否成功写入了所有数据。







# 9 定位流
通常，控制当前流的位置是很有用的。可能应用程序正在读取复杂的、基于记录的 文件，而且需要跳跃式读取。有时，需要把流重新设置成指向初始位置。

## 9.1 fseek()函数定位到某个位置
在任何情 况下，标准I/O 提供了一系列功能等价于系统调用lseek()的函数。fseek()函数是标准I/O 最常用的定位函数，控制 stream 指向文件中由参数offset 和whence 确定的位置：

```c
#include <stdio.h>

int  fseek(FILE   *stream,long  offset,int   whence);
```



下是一个在 Linux 下使用 `fwrite` 和 `fread` 函数的范例

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

#define BUFFER_SIZE 100

int main() {
    FILE *fp;
    char filename[] = "data.dat";
    char writeData[] = "This is some data to write.";
    char readData[BUFFER_SIZE];
    size_t size = strlen(writeData);

    // 以二进制写入模式打开文件
    fp = fopen(filename, "wb");
    if (fp == NULL) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("无法打开文件进行写入：%s，错误信息：%s\n", filename, errorMsg);
        return 1;
    }

    size_t bytesWritten = fwrite(writeData, 1, size, fp);
    if (bytesWritten!= size) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("写入文件时发生错误，只写入了 %ld 字节，期望写入 %ld 字节。错误信息：%s\n", bytesWritten, size, errorMsg);
        fclose(fp);
        return 1;
    }

    fclose(fp);

    // 以二进制读取模式打开文件
    fp = fopen(filename, "rb");
    if (fp == NULL) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("无法打开文件进行读取：%s，错误信息：%s\n", filename, errorMsg);
        return 1;
    }

    size_t bytesRead = fread(readData, 1, BUFFER_SIZE - 1, fp);
    if (bytesRead < size) {
        if (feof(fp)) {
            char errorMsg[100];
            strerror_r(errno, errorMsg, sizeof(errorMsg));
            printf("提前到达文件末尾，只读取了 %ld 字节，期望读取 %ld 字节。错误信息：%s\n", bytesRead, size, errorMsg);
        } else if (ferror(fp)) {
            char errorMsg[100];
            strerror_r(errno, errorMsg, sizeof(errorMsg));
            printf("读取文件时发生错误。错误信息：%s\n", errorMsg);
        }
        fclose(fp);
        return 1;
    }

    readData[bytesRead] = '\0';

    fclose(fp);

    printf("写入的数据：%s\n读取的数据：%s\n", writeData, readData);

    return 0;
}
```



## 9.2 rewind()函数定位到文件开头
标准I/O 也提供了rewind()函数，可以很方便使用： 

**函数原型：**

```c
#include <stdio.h>

void rewind(FILE *stream); 
```

**参数解析**：

+ `stream`：指向要操作的文件流的`FILE`结构体指针。这通常是通过`fopen`等函数打开的文件流，或者是标准输入流`stdin`等。

**返回值解析**：

+ 这个函数没有返回值。它的作用是直接修改文件流的内部状态，将文件指针设置为文件的开头。如果文件流是可随机访问的（例如通过`fopen`以适当的模式打开），那么后续的文件操作将从文件的开头开始进行。



注意 rewind()没有返回值，因此无法直接提供出错信息。调用函数如果希望获取错 误，需要在调用之前清空errno值，并且检查该变量值在调用之后是否非零。举个例子：

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main() {
   FILE *fp = fopen("test.txt", "r+");
   if (fp == NULL) {
       fprintf(stderr, "Error opening file: %s\n", strerror(errno));
       return 1;
   }

   // 读取一些内容并输出
   char buffer[100];
   if (fgets(buffer, sizeof(buffer), fp)!= NULL) {
       printf("Before rewind: %s", buffer);
   }

   // 使用 rewind 函数将文件指针重置到文件开头
   if (rewind(fp)!= 0) {
       fprintf(stderr, "Error rewinding file: %s\n", strerror(errno));
       fclose(fp);
       return 1;
   }

   // 再次读取内容并输出
   if (fgets(buffer, sizeof(buffer), fp)!= NULL) {
       printf("After rewind: %s", buffer);
   }

   fclose(fp);
   return 0;
}
```









## 9.3 ftell()函数获得当前流位置
和 Iseek()不 同 ，fseek()不会返回更新后的流位置。另一个接口提供了该功能。ftell() 函数返回stream的当前流位置：

**函数原型**

```c
#include <stdio.h>

long ftell(FILE *stream);
```



**参数解析**

+ `stream`为指向文件流的指针。

**返回值解析**

出错时，该函数会返回-1,并相应设置errno 值。 

以下是一些关于`ftell`函数的使用示例：

### **示例一：获取文件大小**
```c
#include <stdio.h>

int main() {
    FILE *fp;
    char filename[] = "test.txt";

    fp = fopen(filename, "r");
    if (fp == NULL) {
        perror("无法打开文件");
        return 1;
    }

    fseek(fp, 0, SEEK_END);
    long int fileSize = ftell(fp);
    fclose(fp);

    printf("文件 %s 的大小为 %ld 字节。\n", filename, fileSize);

    return 0;
}
```

### **示例二：记录读取位置并恢复**
```c
#include <stdio.h>

int main() {
    FILE *fp;
    char filename[] = "test.txt";

    fp = fopen(filename, "r");
    if (fp == NULL) {
        perror("无法打开文件");
        return 1;
    }

    long int initialPosition = ftell(fp);

    char buffer[100];
    fgets(buffer, sizeof(buffer), fp);
    printf("读取的内容：%s", buffer);

    long int currentPosition = ftell(fp);
    printf("当前位置：%ld\n", currentPosition);

    // 假设进行一些其他操作后想回到初始位置读取
    fseek(fp, initialPosition, SEEK_SET);
    fgets(buffer, sizeof(buffer), fp);
    printf("回到初始位置后读取的内容：%s", buffer);

    fclose(fp);

    return 0;
}
```





# 10 Flush刷新输出流
标准I/O库提供了一个接口，可以将用户缓冲区写入内核，并且保证写到流中的所 有数据都通过write()函数flush 输出。fflush()函数提供了这一功能：

```c
#include <stdio.h>

int fflush(FILE *stream);
```

**参数说明：**

调用该函数时，stream 指向的流中所有未写入的数据会被 flush 到内核中。如果 stream是空的(NULL),  进程中所有打开的流会被flush。

**返回值解析：**

成功时，fflush()返回0。 失败时，返回EOF, 并相应设置errno 值。



为了更好地理解 fflush()函数的功能，**需要理解C 函数库维持的缓冲区和内核本身的缓冲区之间的区别**。本章提到的所有调用所需要的缓冲区都是由 C  函数库来维 护的，它是在用户空间，而不是内核空间。也就是说，这些调用的性能提升空间来自于用户空间，运行的是用户代码，而不是系统调用。只有当需要访问磁盘或其他 某些介质时，才会发起系统调用。



**fflush()函数的功能只是把用户缓冲的数据写入到内核缓冲区**。其执行结果是看起来似乎没有用户缓冲区，而是直接调用 write()函数。fflush() 函数并不保证数据最终 会写到物理介质上——如果需要这个功能，应该使用fsync()  这类函数。 



为了确保数据最终会写到备份存储 (backing  store) 中，可以如下来完成：在调用fflush()后，立即调用fsync()；也就是说，先保证用户缓冲区被写入到内核，然后保证内核缓冲区被写入到磁盘。



应用范例：

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

#define BUFFER_SIZE 100

int main() {
    FILE *fp;
    char filename[] = "data.txt";
    char writeData[] = "This is some data to write.";
    char readData[BUFFER_SIZE];

    // 以读写模式打开文件，如果文件不存在则创建
    fp = fopen(filename, "w+");
    if (fp == NULL) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("无法打开文件 %s，错误信息：%s\n", filename, errorMsg);
        return 1;
    }

    // 使用 fwrite 写入数据
    size_t bytesWritten = fwrite(writeData, 1, strlen(writeData), fp);
    if (bytesWritten!= strlen(writeData)) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("写入文件时发生错误，只写入了 %ld 字节，期望写入 %ld 字节。错误信息：%s\n", bytesWritten, strlen(writeData), errorMsg);
        fclose(fp);
        return 1;
    }

    // 使用 fflush 刷新缓冲区
    int flushResult = fflush(fp);
    if (flushResult!= 0) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("刷新文件缓冲区时发生错误：%s\n", errorMsg);
        fclose(fp);
        return 1;
    }

    // 将文件指针移动到文件开头
    fseek(fp, 0, SEEK_SET);

    // 使用 fread 读取数据
    size_t bytesRead = fread(readData, 1, BUFFER_SIZE - 1, fp);
    if (bytesRead < strlen(writeData)) {
        if (feof(fp)) {
            char errorMsg[100];
            strerror_r(errno, errorMsg, sizeof(errorMsg));
            printf("提前到达文件末尾，只读取了 %ld 字节，期望读取 %ld 字节。错误信息：%s\n", bytesRead, strlen(writeData), errorMsg);
        } else if (ferror(fp)) {
            char errorMsg[100];
            strerror_r(errno, errorMsg, sizeof(errorMsg));
            printf("读取文件时发生错误。错误信息：%s\n", errorMsg);
        }
        fclose(fp);
        return 1;
    }

    readData[bytesRead] = '\0';

    fclose(fp);

    printf("写入的数据：%s\n读取的数据：%s\n", writeData, readData);

    return 0;
}
```





# 11 错误和文件结束
对于某些标准I/O 接口，如fread()函数，把失败信息返回给调用方做得很不友好， **因为它们没有提供机制可以区分错误和文件结束(EOF)**。 对于这些调用，在某些场合需要检查流的状态，从而区分是出现错误还是达到文件结尾。标准I/O 为此提 供了两个接口。

函数 ferror()用于判断给定的stream 是否有错误标志：

```c
#include <stdio.h>

int ferror(FILE *stream);
```

错误标志由其他标准I/O接口在响应某种错误情况时设置。**如果错误标志被设置，ferror()函数返回非0值，否则返回0。**



函数feof()用于判断指定 stream 是否设置了文件结束标志： 

```c
#include <stdio.h>

int feof(FILE *stream);
```

 当到达文件结尾时，其他标准I/O 函数会设置EOF 标志。**如果设置了EOF标志， feof()函数会返回非0值，否则返回0。**

范例代码：

```c
if (ferror(fp)) {
   char errorMsg[100];
   strerror_r(errno, errorMsg, sizeof(errorMsg));
   printf("读取文件时发生错误：%s\n", errorMsg);
} else if (feof(fp)) {
   printf("已到达文件末尾，没有更多内容可读。\n");
}
```





clearerr()函数会清空指定stream 的错误和EOF 标志： 

```c
#include  <stdio.h>

void  clearerr(FILE  *stream);
```

clearer()函数没有返回值，而且不会失败(因此无法判断是否提供的是一个非法的 流)。只有检查了错误标志和 EOF 标志后，才可以调用clearerr(),  因为清空操作是不可恢复的。 





# 12 获取关联的文件描述符（了解）
在某些情况下，获得指定流的文件描述符是很方便的。例如，当不存在和流关联的 标准I/O 函数时，可以通过其文件描述符对该流执行系统调用。为了获得和流关联 的文件描述符，可以使用fileno()函数：

```c
#include <stdio.h>

int fileno(FILE *stream);
```

成功时，fileno()返回和指定stream 关联的文件描述符。失败时，返回-1。只有当指 定流非法时，才会失败，这时fileno()函数会将errno 值设置为EBADF。



通常，**不建议混合使用标准I/O调用和系统调用**。当使用fileno()函数时，编程人员 必须非常谨慎，确保基于文件描述符的操作和用户缓冲没有冲突。尤其值得注意的 是，在操作和流关联的文件描述符之前，最好先对流进行刷新 (flush) 。 记住，最好永远都不要混合使用文件描述符和基于流的I/O 操作。



# 13 控制缓冲
标准I/O 实现了三种类型的用户缓冲，并为开发者提供了接口，可以控制缓冲区类 型和大小。以下不同类型的用户缓冲提供不同功能，并适用于不同的场景：

**无缓冲 (Unbuffered)**

不执行用户缓冲。数据直接提交给内核。因为这种无缓冲模式不支持用户缓冲(而 用户缓冲一般会带来很多好处),通常很少使用，只有一个例外：标准错误默认是 采用无缓冲模式。



**行缓冲 (Line-buffered)**

缓冲是以行为单位执行。每遇到换行符，缓冲区就会被提交到内核。行缓冲对把流 输出到屏幕时很有用，因为输出到屏幕的消息也是通过换行符分隔的。因此，行缓 冲是终端的默认缓冲模式，比如标准输出。



**块缓冲 (Block-buffered)**

缓冲以块为单位执行，每个块是固定的字节数。本章一开始讨论的缓冲模式即块缓冲，它很适用于处理文件。默认情况下，和文件相关的所有流都是块缓冲模式。标准 I/O称块缓冲为“完全缓冲 (full   buffering)”。



大部分情况下，默认的缓冲模式即对于特定场景是最高效的。但是，标准I/O 还提 供了一个接口，可以修改使用的缓冲模式：

```c
#include <stdio.h>

int setvbuf(FILE *stream,char *buf,int mode,size_t size);
```



setbuf()函数把指定 stream 的缓冲模式设置成mode 模式，mode 值必须是以下之一： 

+ IONBF  无缓冲。
+ IOLBF   行缓冲。 
+ IOFBF  块缓冲。

在 IONBF无缓冲模式下，会忽略参数buf 和size; 对于其他模式，buf可以指向一个size 字节大小的缓冲空间，标准I/O 会用它来执行对给定流的缓冲。如果buf 为空 ，glibc 会自动分配指定size 的缓冲区。



setvbuf()函数必须在打开流后，并在执行任何操作之前被调用。成功时，返回0;  出错时，返回非0值。



总体而言，开发者不需要关心如何在流上使用缓冲。对于标准出错，终端是采用行缓冲模式；对于文件，采用块缓冲模式。默认的块缓冲区大小是 BUFSIZ,  在<stdio.h> 中定义，而且通常情况下该大小设置是最优的(一个较大值，是标准块大小的整数 倍 ) 。



范例：

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main() {
    FILE *fp;
    char filename[] = "output.txt";

    // 以写入模式打开文件
    fp = fopen(filename, "w");
    if (fp == NULL) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("无法打开文件 %s，错误信息：%s\n", filename, errorMsg);
        return 1;
    }

    // 设置缓冲区
    int result = setvbuf(fp, NULL, _IOFBF, 1024);
    if (result!= 0) {
        char errorMsg[100];
        strerror_r(errno, errorMsg, sizeof(errorMsg));
        printf("设置缓冲区时发生错误：%s\n", errorMsg);
        fclose(fp);
        return 1;
    }

    // 写入数据
    fprintf(fp, "Hello, world!");

    fclose(fp);

    printf("成功写入数据到文件 %s。\n", filename);

    return 0;
}
```





# 14 线程安全（先了解）
线程是指在一个进程中的执行单元。绝大多数进程只有一个线程。不过，一个进程也可以持有多个线程，每个线程执行自己的代码逻辑。我们称这种进程为“多线程 (multi threaded)” 。 可以把多线程的进程理解成有多个进程共享同一地址空间，如 果没有显式协调，线程会在任意时刻、以任意方式运行。在多处理器系统中，同一 个进程的两个或多个线程可能会并发执行。在访问共享数据时，有两种方式可以避 免修改它： 一是采取数据同步访问 (synchronized access) 机制(通过加锁实现), 二是把数据存储在线程的局部变量中 (thread-local,  也称为线程封闭 thread confinement)。

支持线程的操作系统提供了锁机制(锁是指可以保证相互排斥的程序结构), 从而 保证线程之间不会互相干扰。标准I/O 使用了这些机制，保证单个进程的多个线程 可以同时发起标准I/O调用——甚至可以对于同一个流发起多个调用，不会由于并 发操作而彼此干扰。但是，这些机制通常还不能满足需求。举个例子，在某些情况 下，你希望给一组调用加锁，把一个I/O操作的临界区 (critical region,是指一段 独立运行的代码，不受其他线程干扰)扩大为几个I/O 操作。而在另外一些情况下， 你可能希望取消全部锁，以提高程序效率。本节中，我们将会讨论如何处理这两 种情况。



**标准I/O 函数在本质上是线程安全的**。在每个函数的内部实现中，都关联了一把锁、 一个锁计数器，以及持有该锁并打开一个流的线程。每个线程在执行任何I/O 请求 之前，必须首先获得锁而且持有该锁。两个或多个运行在同一个流上的线程不会交 叉执行标准I/O 操作，因此，在单个函数调用中，标准I/O 操作是原子操作。



当然，在实际应用中，很多应用程序需要比独立函数调用级别更强的原子性。举个 例子，假设一个进程中有多个线程发起写请求，由于标准I/O 函数是线程安全的， 各个写请求不会交叉执行导致输出混乱。也就是说，即使两个线程同时发起请求操 作，加锁可以保证执行一个写请求后再执行另一个写请求。但是，如果进程连续发 起很多写请求，希望不但不同线程的写请求不会交叉，同一线程的写请求也不会交 叉，该怎么做呢?为了支持该功能，标准I/O 提供了一系列函数，每个函数可以操 纵和流关联的锁。



### 14.1 手动文件加锁(了解)
#### flockfile
函数 flockfile()会等待指定stream 被解锁，然后增加自己的锁计数，获得锁，该函 数的执行线程成为流的持有者，并返回：

```c
#include <stdio.h>

void flockfile(FILE *stream);
```

函数 funlockfile() 会减少和指定stream 关联的锁计数： 

```c
#include <stdio.h>

void funlockfile(FILE *stream);
```

如果锁计数值为0,当前线程会放弃对该流的持有权，另一个线程可以获得该锁。



通常，取消锁会带来各种问题。但有些程序可能会通过把所有I/O 委托给单个线程的方式(即一种 线程封闭方式),来实现线程安全策略。这样，就不必增加锁的开销。

这些调用可以嵌套。也就是说，单个线程可以执行多次 flockfile() 调用，而直到该 进程执行相同数量的funlockfile() 调用后，该流才会被解锁。



ftrylockfile()函数是flockfile()函数的非阻塞版： 

```c
#include <stdio.h>

int ftrylockfile(FILE *stream);
```



如果指定 stream 流当前已经加锁，ftrylockfile()函数不做任何处理，会立即返回一 个非0值。如果指定 stream 当前并没有加锁，执行 frylockfile()的线程会获得锁， 增加锁计数值，成为该stream 的持有者，并返回0。



我们来看下面这个例子。假设需要向一个文件中写多行数据，保证在写数据时多个 线程的写操作不会交织在一起：

```c
flockfile(stream);

fputs("List  of  treasure:\n",stream);

fputs("  (1)500 gold coins\n",stream);

fputs("  (2)Wonderfully ornate dishware\n",stream); 

funlockfile(stream);
```



尽管各个fputs()操作不会和其他I/O 操作造成竞争——比如 “List of treasure” 的输 出中间不会夹杂任何东西——另一个线程对该流的其他标准I/O 操作线程可能会夹 杂在当前线程的两个 fputs()语句之间。理想情况下，应用在设计上应该避免多个线 程向同一个流提交 I/O 操作。但是，如果应用确实需要这么做，并且需要一个比单 个函数调用更大的原子操作区域，flockfile()  及其相关函数可以解决这个问题。



### 14.2 对流操作解锁(了解)
手动给流加锁还有另外一个原因。只有应用开发者才能提供更精细、更准确的锁控 制，这样才可以将加锁的代价降到最小，并提高效率。为此，Linux 提供了一系列 函数，类似于常见的标准I/O接口，但是不执行任何锁操作。实际上，这些锁是不加锁的标准I/O:

```c
#define _GNU_SOURCE
#include <stdio.h>
int fgetc_unlocked (FILE *stream)
char *fgets_unlocked(char *str , int size , FILE *stream);
size_t fread_unlocked(void *buf , size_t size , size_t nr , FILE *stream);
int fputc_unlocked(int c , FILE *stream);
int fputs_unlocked(const char *str , FILE *stream);
size_t fwrite_unlocked(void *buf , size_t size , size_t nr , FILE *stream);
int fflush_unlocked (FILE *stream);
int feof_unlocked (FILE *stream);
int ferror_unlocked (FILE *stream);
int fileno_unlocked (FILE *stream);
int clearerr_unlocked (FILE *stream);
```



除了不检查或获取指定 stream 上的锁以外，这些函数和其对应的加锁函数功能完全相同。

如需加锁，程序员必须确保手工获得并释放锁。



# 15 结束语
标准I/O 是C 标准库提供的一个用户缓冲库。虽然有些不足，它是一个功能强大且 非常流行的解决方案。实际上，许多C 程序员，除了采用标准I/O, 对其他解决方 案一无所知。当然，对于终端I/O, 行缓冲是最理想的，而且标准 I/O 是它唯一的 解决方式。谁会直接调用write()向标准输出写数据呢?



标准I/O  (一般来说也包括用户缓冲)适用于下列情况：

● 很可能会发起多个系统调用，而你希望合并这些调用从而减少开销。

● 性能至关重要，你希望保证所有I/O操作都是以块大小执行，从而保证块对齐。

● 访问模式是基于字符或行，你希望通过接口可以简单地实现这种访问，但又不 希望产生额外的系统调用。

● 和底层的Linux系统调用相比，更喜欢调用高层的接口。

然而，最大的灵活在于直接使用Linux 系统调用。下一章，我们将学习高级I/O 及 其关联的系统调用。



# 16 拓展
## 16.1 linux fdopen应用场景
在 Linux 中，`fdopen`函数有以下一些主要应用场景**（这些都是高级应用，目前阶段了解下就行）**：

**一、与文件描述符相关的系统调用结合使用**

1. 与管道（pipe）配合
    - 在多进程编程中，常常使用管道进行进程间通信。创建管道后得到两个文件描述符，可以使用`fdopen`将其中一个文件描述符转换为`FILE*`类型的文件流指针，以便使用标准的文件输入输出函数进行操作。
    - 例如，一个父进程和子进程通过管道通信，子进程向管道写入数据，父进程从管道读取数据。父进程可以使用`fdopen`将读取端的文件描述符转换为文件流，然后使用`fgets`等函数读取数据，这样的代码更加简洁和可读。
2. 与套接字（socket）配合
    - 在网络编程中，当创建一个套接字后，得到的文件描述符也可以通过`fdopen`转换为文件流指针。这样可以使用熟悉的文件操作函数来处理网络通信，比如使用`fprintf`向套接字写入数据，使用`fscanf`从套接字读取数据。
    - 这在处理网络通信时可以提高代码的一致性和可读性，尤其是对于习惯使用标准文件输入输出函数的开发者来说非常方便。

**二、封装底层文件描述符**

有时候需要对底层的文件描述符进行更高级的抽象和封装。通过`fdopen`将文件描述符转换为文件流后，可以利用文件流的缓冲机制、格式化输入输出等功能，而无需直接操作底层的文件描述符。

例如，一个库函数可能接受一个文件描述符作为参数，但为了在内部使用更方便的文件操作方式，可以使用`fdopen`将文件描述符转换为文件流，然后进行各种文件操作，最后再将结果返回给调用者。这样可以在不改变底层文件描述符的情况下，提供更高级的功能和更方便的接口。

范例：

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>

int main() {
    // 打开一个文件用于写入，如果不存在则创建
    int fileDescriptor = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR);
    if (fileDescriptor == -1) {
        perror("open");
        return 1;
    }

    // 使用 fdopen 将文件描述符转换为 FILE 流指针
    FILE *fileStream = fdopen(fileDescriptor, "w");
    if (!fileStream) {
        perror("fdopen");
        close(fileDescriptor);
        return 1;
    }

    // 向文件中写入数据
    fprintf(fileStream, "This is written using fdopen.\n");
    fputs("Another line of text.\n", fileStream);

    // 关闭文件流
    fclose(fileStream);

    return 0;
}
```





## 16.2 文件I/O与标准I/O区别
| 标准I/O | 文件I/O |
| :--- | :--- |
| ANSIC | POSIX |
| 带缓冲（减少系统调用次数） | 无缓冲（读写文件需要进行系统调用） |
| 流（FILE结构体）打开文件 | 文件描述符表示一个打开的文件 |


[Linux系统编程入门学习笔记5-文件IO - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/341914732)



C库标准IO函数是在调用Linux系统IO函数的基础上实现的。C库标准IO函数具有缓冲区，读写效率比起直接调用系统IO函数往往更高。

![](C:\Users\lqf\AppData\Roaming\Typora\typora-user-images\image-20250205172306228.png)

![](C:\Users\lqf\AppData\Roaming\Typora\typora-user-images\image-20250205172333988.png)



> 更新: 2025-04-18 16:50:14  
> 原文: <https://www.yuque.com/linuxer/gscfv1/rin50tnmdociy54g>