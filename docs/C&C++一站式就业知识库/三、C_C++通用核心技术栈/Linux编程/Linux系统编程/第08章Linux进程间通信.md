# 第08章 Linux进程间通信

# Linux进程间通信（IPC）文档
# 1. 概述
在Linux系统中，进程间通信（Inter-Process Communication, IPC）是指多个进程之间进行数据交换和同步的机制。Linux提供了多种IPC机制，每种机制都有其特定的应用场景和优缺点。本文将介绍Linux中常见的IPC机制，包括它们的原理、函数原型、范例代码、流程图，以及在实际工程中的应用案例。

# 2. Linux进程间通信的种类
Linux系统中常见的进程间通信机制包括：

1. **管道（Pipe）**
2. **命名管道（FIFO）**
3. **消息队列（Message Queue）**
4. **共享内存（Shared Memory）**
5. **信号量（Semaphore）**
6. **信号（Signal）**
7. **套接字（Socket）**

# 3. 各种IPC机制的原理与函数原型
## 3.1 管道（Pipe）
**原理**：管道是一种半双工的通信方式，数据只能单向流动。管道通常用于父子进程之间的通信。

**函数原型**：

```c
#include <unistd.h>
int pipe(int pipefd[2]);
```

+ `pipefd[0]`：用于读取数据的文件描述符。
+ `pipefd[1]`：用于写入数据的文件描述符。

**范例**：

```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

int main() {
    int pipefd[2];
    pid_t pid;
    char buf[256];

    if (pipe(pipefd) == -1) {
        perror("pipe");
        return 1;
    }

    pid = fork();
    if (pid == 0) { // 子进程
        close(pipefd[1]); // 关闭写端
        read(pipefd[0], buf, sizeof(buf));
        printf("Child received: %s\n", buf);
        close(pipefd[0]);
    } else { // 父进程
        close(pipefd[0]); // 关闭读端
        const char *msg = "Hello from parent";
        write(pipefd[1], msg, strlen(msg) + 1);
        close(pipefd[1]);
    }

    return 0;
}
```

**流程图**：

```plain
父进程
  |
  | pipe()
  v
创建管道
  |
  | fork()
  v
子进程
  |
  | write()
  v
父进程写入数据
  |
  | read()
  v
子进程读取数据
```

## 3.2 命名管道（FIFO）
**原理**：命名管道是一种特殊的文件，允许无亲缘关系的进程通过文件系统进行通信。

**函数原型**：

```c
#include <sys/types.h>
#include <sys/stat.h>
int mkfifo(const char *pathname, mode_t mode);
```

**范例**：

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

int main() {
    const char *fifo_path = "/tmp/my_fifo";
    mkfifo(fifo_path, 0666);

    pid_t pid = fork();
    if (pid == 0) { // 子进程
        int fd = open(fifo_path, O_RDONLY);
        char buf[256];
        read(fd, buf, sizeof(buf));
        printf("Child received: %s\n", buf);
        close(fd);
    } else { // 父进程
        int fd = open(fifo_path, O_WRONLY);
        const char *msg = "Hello from parent";
        write(fd, msg, strlen(msg) + 1);
        close(fd);
    }

    return 0;
}
```

**流程图**：

```plain
父进程
  |
  | mkfifo()
  v
创建FIFO文件
  |
  | fork()
  v
子进程
  |
  | open() + write()
  v
父进程写入数据
  |
  | open() + read()
  v
子进程读取数据
```

## 3.3 消息队列（Message Queue）
**原理**：消息队列允许进程通过消息进行通信，消息可以带有类型，接收方可以选择性地接收特定类型的消息。

**函数原型**：

```c
#include <sys/msg.h>
int msgget(key_t key, int msgflg);
int msgsnd(int msqid, const void *msgp, size_t msgsz, int msgflg);
ssize_t msgrcv(int msqid, void *msgp, size_t msgsz, long msgtyp, int msgflg);
```

**范例**：

```c
#include <stdio.h>
#include <sys/msg.h>
#include <string.h>

struct msgbuf {
    long mtype;
    char mtext[256];
};

int main() {
    key_t key = ftok("msgqfile", 65);
    int msgid = msgget(key, 0666 | IPC_CREAT);

    pid_t pid = fork();
    if (pid == 0) { // 子进程
        struct msgbuf msg;
        msgrcv(msgid, &msg, sizeof(msg.mtext), 1, 0);
        printf("Child received: %s\n", msg.mtext);
    } else { // 父进程
        struct msgbuf msg;
        msg.mtype = 1;
        strcpy(msg.mtext, "Hello from parent");
        msgsnd(msgid, &msg, sizeof(msg.mtext), 0);
    }

    return 0;
}
```

**流程图**：

```plain
父进程
  |
  | msgget()
  v
创建消息队列
  |
  | fork()
  v
子进程
  |
  | msgsnd()
  v
父进程发送消息
  |
  | msgrcv()
  v
子进程接收消息
```

## 3.4 共享内存（Shared Memory）
**原理**：共享内存允许多个进程共享同一块内存区域，是最快的IPC机制。

**函数原型**：

```c
#include <sys/shm.h>
int shmget(key_t key, size_t size, int shmflg);
void *shmat(int shmid, const void *shmaddr, int shmflg);
int shmdt(const void *shmaddr);
```

**范例**：

```c
#include <stdio.h>
#include <sys/shm.h>
#include <string.h>

int main() {
    key_t key = ftok("shmfile", 65);
    int shmid = shmget(key, 1024, 0666 | IPC_CREAT);
    char *str = (char*) shmat(shmid, (void*)0, 0);

    pid_t pid = fork();
    if (pid == 0) { // 子进程
        printf("Child read: %s\n", str);
        shmdt(str);
    } else { // 父进程
        strcpy(str, "Hello from parent");
        shmdt(str);
    }

    return 0;
}
```

**流程图**：

```plain
父进程
  |
  | shmget()
  v
创建共享内存
  |
  | fork()
  v
子进程
  |
  | shmat() + write()
  v
父进程写入数据
  |
  | shmat() + read()
  v
子进程读取数据
```

## 3.5 信号量（Semaphore）
**原理**：信号量用于进程间的同步，通常用于控制对共享资源的访问。

**函数原型**：

```c
#include <sys/sem.h>
int semget(key_t key, int nsems, int semflg);
int semop(int semid, struct sembuf *sops, size_t nsops);
```

**范例**：

```c
#include <stdio.h>
#include <sys/sem.h>
#include <unistd.h>

int main() {
    key_t key = ftok("semfile", 65);
    int semid = semget(key, 1, 0666 | IPC_CREAT);

    pid_t pid = fork();
    if (pid == 0) { // 子进程
        struct sembuf sb = {0, -1, 0};
        semop(semid, &sb, 1);
        printf("Child process\n");
    } else { // 父进程
        sleep(2); // 模拟父进程延迟
        struct sembuf sb = {0, 1, 0};
        semop(semid, &sb, 1);
        printf("Parent process\n");
    }

    return 0;
}
```

**流程图**：

```plain
父进程
  |
  | semget()
  v
创建信号量
  |
  | fork()
  v
子进程
  |
  | semop() (等待)
  v
父进程
  |
  | semop() (释放)
  v
子进程继续执行
```

## 3.6 信号（Signal）
**原理**：信号是一种异步通信机制，用于通知进程发生了某个事件。

**函数原型**：

```c
#include <signal.h>
void (*signal(int signum, void (*handler)(int)))(int);
int kill(pid_t pid, int sig);
```

**范例**：

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    printf("Received signal: %d\n", sig);
}

int main() {
    pid_t pid = fork();
    if (pid == 0) { // 子进程
        signal(SIGUSR1, handler);
        pause(); // 等待信号
    } else { // 父进程
        sleep(2); // 模拟父进程延迟
        kill(pid, SIGUSR1);
    }

    return 0;
}
```

**流程图**：

```plain
父进程
  |
  | fork()
  v
子进程
  |
  | signal() + pause()
  v
父进程
  |
  | kill()
  v
子进程处理信号
```

## 3.7 套接字（Socket）
**原理**：套接字是一种通用的IPC机制，可以用于不同主机之间的进程通信。

**函数原型**：

```c
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
int listen(int sockfd, int backlog);
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

**范例**：

```c
#include <stdio.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <string.h>
#include <unistd.h>

int main() {
    int server_fd, new_socket;
    struct sockaddr_in address;
    int addrlen = sizeof(address);
    char buffer[1024] = {0};
    const char *msg = "Hello from server";

    server_fd = socket(AF_INET, SOCK_STREAM, 0);
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(8080);

    bind(server_fd, (struct sockaddr *)&address, sizeof(address));
    listen(server_fd, 3);

    new_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen);
    read(new_socket, buffer, 1024);
    printf("Server received: %s\n", buffer);
    send(new_socket, msg, strlen(msg), 0);

    close(new_socket);
    close(server_fd);
    return 0;
}
```

**流程图**：

```plain
服务器
  |
  | socket() + bind() + listen()
  v
等待客户端连接
  |
  | accept()
  v
客户端连接
  |
  | read() + send()
  v
数据交换
```



使用 `iperf` 和 `lmbench` 工具测试进程间通信（IPC）性能是一种常见的方法。以下是如何使用这些工具进行测试的详细步骤，包括安装、配置和运行测试。

---

# 4 IPC性能测试
## 4.1 常用工具
### 4.1.1 **iperf**
+ **用途**：主要用于测试网络带宽和吞吐量。
+ **适用场景**：测试基于套接字（Socket）的进程间通信性能，尤其是TCP/UDP通信。
+ **特点**：支持客户端-服务器模式，可以测量带宽、延迟和丢包率。

### 4.12 **lmbench**
+ **用途**：用于测量系统性能，包括进程间通信、内存访问、文件系统操作等。
+ **适用场景**：测试共享内存、管道、消息队列等IPC机制的性能。
+ **特点**：提供多种微基准测试，适合低层次性能分析。

---

## 4.2 测试方法
### 4.2.1 使用 `iperf` 测试套接字通信性能
#### 安装 `iperf`
在Linux系统中，可以通过包管理器安装 `iperf`：

```bash
sudo apt-get install iperf3  # Ubuntu/Debian
sudo yum install iperf3      # CentOS/RHEL
```

#### 测试步骤
1. **启动服务器端**：  
在一台机器或一个终端中运行以下命令：

```bash
iperf3 -s
```

这将启动一个服务器，监听默认端口5201。

2. **启动客户端**：  
在另一台机器或另一个终端中运行以下命令：

```bash
iperf3 -c <服务器IP地址>
```

例如：

```bash
iperf3 -c 127.0.0.1  # 本地测试
```

3. **查看结果**：  
测试完成后，客户端和服务器端都会显示带宽、延迟等性能数据。

#### 示例输出
```plain
Connecting to host 127.0.0.1, port 5201
[  5] local 127.0.0.1 port 12345 connected to 127.0.0.1 port 5201
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-10.00  sec  12.3 GBytes  10.6 Gbits/sec  sender
[  5]   0.00-10.00  sec  12.3 GBytes  10.6 Gbits/sec  receiver
```

---

### 4.2.2 使用 `lmbench` 测试IPC性能
#### 安装 `lmbench`
1. 下载 `lmbench` 源码：

```bash
wget http://www.bitmover.com/lmbench/lmbench3.tar.gz
tar -xzf lmbench3.tar.gz
cd lmbench3
```

2. 编译和安装：

```bash
make
```

#### 测试步骤
1. **运行测试**：  
在终端中运行以下命令：

```bash
make results
```

这将运行一系列性能测试，包括IPC性能。

2. **查看结果**：  
测试完成后，结果会保存在 `results/` 目录下的文件中。可以通过以下命令查看：

```bash
cat results/<主机名>.0
```

#### 测试内容
`lmbench` 提供了多种IPC性能测试，包括：

+ **管道（Pipe）**：测试管道通信的延迟和吞吐量。
+ **Unix Domain Socket**：测试本地套接字通信性能。
+ **共享内存（Shared Memory）**：测试共享内存的读写性能。
+ **消息队列（Message Queue）**：测试消息队列的通信性能。

#### 示例输出
```plain
Pipe latency: 5.00 microseconds
AF_UNIX sock stream latency: 3.20 microseconds
Process fork+exit: 1200 microseconds
Process fork+execve: 1500 microseconds
Process fork+/bin/sh -c: 3000 microseconds
```

---

## 4.3 自定义测试脚本
如果需要更精细地测试特定IPC机制（如共享内存、消息队列等），可以编写自定义测试程序。以下是一个简单的共享内存测试示例：

### 4.3.1 **共享内存性能测试**
#### 代码示例
```c
#include <stdio.h>
#include <sys/shm.h>
#include <sys/ipc.h>
#include <sys/time.h>
#include <string.h>

#define SHM_SIZE 1024 * 1024  // 1MB

int main() {
    key_t key = ftok("shmfile", 65);
    int shmid = shmget(key, SHM_SIZE, 0666 | IPC_CREAT);
    char *str = (char*) shmat(shmid, (void*)0, 0);

    struct timeval start, end;
    gettimeofday(&start, NULL);

    // 写入数据
    memset(str, 'A', SHM_SIZE);

    gettimeofday(&end, NULL);
    long seconds = end.tv_sec - start.tv_sec;
    long microseconds = end.tv_usec - start.tv_usec;
    double elapsed = seconds + microseconds * 1e-6;

    printf("Time taken: %f seconds\n", elapsed);

    shmdt(str);
    shmctl(shmid, IPC_RMID, NULL);
    return 0;
}
```

#### 运行测试
1. 编译代码：

```bash
gcc -o shm_test shm_test.c
```

2. 运行程序：

```bash
./shm_test
```

3. 查看结果：

```plain
Time taken: 0.001234 seconds
```

---

## 4.4 总结
### **1. 工具选择**
+ `iperf`：适合测试套接字通信性能，尤其是网络带宽和延迟。
+ `lmbench`：适合测试多种IPC机制的性能，包括管道、共享内存、消息队列等。

### **2. 测试建议**
+ 对于本地进程通信，优先使用 `lmbench` 进行综合性能测试。
+ 对于网络通信，使用 `iperf` 测试带宽和延迟。
+ 对于特定IPC机制（如共享内存），可以编写自定义测试程序。

### **3. 实际应用**
+ **共享内存**：适合高频数据交换场景（如数据库缓存）。
+ **Unix Domain Socket**：适合本地进程通信（如容器间通信）。
+ **消息队列**：适合异步任务调度（如任务队列）。

通过以上工具和方法，可以全面评估不同IPC机制的性能，为实际工程中的选择提供数据支持。



# 5 总结
在 Linux 系统中，选择合适的进程间通信（IPC）方式取决于多个因素，下面从不同的维度为你详细分析如何做出合适的选择：

## 5.1 通信需求方面
### 数据量大小
+ **少量数据**：
    - 对于只需传递少量数据（如标志位、简单指令）的场景，信号和管道是不错的选择。
    - 信号机制简单，适合发送紧急且数据量极少的通知，例如通知进程终止、暂停等。例如，当用户按下 `Ctrl + C` 时，系统会向当前进程发送 `SIGINT` 信号，进程收到该信号后可执行相应的清理操作。
    - 无名管道适合在具有亲缘关系的进程间快速传递少量数据，实现简单的同步和数据交互。比如在一个父进程和子进程的程序中，父进程可以通过无名管道向子进程传递简单的配置参数。
+ **大量数据**：
    - 共享内存是处理大量数据传输的最佳方式。由于多个进程可以直接访问同一块物理内存，避免了数据的多次复制，显著提高了数据传输效率。例如，在图像或视频处理应用中，多个进程可能需要同时访问和处理大量的图像或视频数据，使用共享内存可以快速地在这些进程间共享数据。
    - 消息队列也可以处理较大数据量，但相比共享内存，它在数据传输时会有一定的复制开销，不过它提供了更灵活的消息管理机制。

### 数据传输方向
+ **单向传输**：
    - 管道（无名管道和命名管道）非常适合单向数据传输。无名管道常用于父子进程间的单向数据传递，而命名管道可用于不同进程间的单向通信。例如，在一个日志记录系统中，日志生成进程可以通过命名管道将日志信息单向传递给日志存储进程。
+ **双向传输**：
    - 消息队列和套接字支持双向数据传输。消息队列允许进程在不同时间点以消息为单位进行双向通信，每个消息可以有不同的类型和内容。套接字则提供了更灵活的双向通信机制，无论是面向连接的 TCP 套接字还是无连接的 UDP 套接字，都能满足进程间双向数据交换的需求。例如，在一个客户端 - 服务器架构的应用中，客户端和服务器可以通过套接字进行双向通信，客户端发送请求，服务器返回响应。

### 同步需求
+ **异步通信**：
    - 信号是典型的异步通信方式，进程不需要等待信号的到来，可以继续执行其他任务。当信号发生时，进程会中断当前操作，执行信号处理函数。例如，在一个长时间运行的后台进程中，可以使用信号来处理系统资源耗尽等异常情况，而不影响进程的正常运行。
+ **同步通信**：
    - 信号量常用于实现进程间的同步和互斥。当多个进程需要访问共享资源时，信号量可以确保同一时间只有一个进程能够访问该资源，从而避免数据竞争。例如，在多个进程同时访问共享文件的场景中，可以使用信号量来控制对文件的读写操作，保证数据的一致性。
    - 消息队列也可以用于同步通信，进程可以通过发送和接收消息来协调操作的执行顺序。

## 5.2 进程关系方面
### 亲缘关系进程
+ 对于具有亲缘关系（如父子进程、兄弟进程）的进程，无名管道是一种简单且高效的通信方式。由于其创建和使用相对简单，不需要额外的文件系统操作，适合在进程创建过程中快速建立通信通道。例如，在一个 shell 脚本中启动多个子进程进行数据处理时，可以使用无名管道在子进程和父进程间传递数据。

### 非亲缘关系进程
+ 命名管道、消息队列、共享内存和套接字适用于非亲缘关系的进程间通信。
    - 命名管道以文件的形式存在于文件系统中，不同进程可以通过该文件进行通信，无需进程间有特定的亲缘关系。
    - 消息队列和共享内存通过内核提供的机制实现通信，只要进程能够获取相应的标识符，就可以进行通信。
    - 套接字则可以实现不同主机上进程间的通信，也可用于同一主机上非亲缘关系进程间的通信，具有很强的通用性。

## 5.3 系统资源和性能方面
### 资源开销
+ 信号的资源开销最小，它只是一个简单的通知机制，不涉及大量的数据传输和存储。
+ 管道和消息队列会占用一定的内核缓冲区资源，随着数据量的增加，资源消耗也会相应增加。
+ 共享内存虽然在数据传输效率上很高，但需要分配物理内存



## 5.4 大神建议
陈硕是知名的技术专家，他建议进程间通信尽量使用套接字（Socket），主要基于以下几个方面的考量：

### 通用性与跨平台性
+ **跨主机通信**：套接字最大的优势之一是能够轻松实现不同主机之间的进程通信。在分布式系统中，各个节点可能分布在不同的物理机器上，使用套接字可以方便地构建起跨主机的通信网络。例如，在云计算环境下，多个虚拟机上的进程可以通过套接字进行数据交互，实现资源共享和协同工作。这种跨主机通信能力是其他一些进程间通信方式（如管道、共享内存等）所不具备的。
+ **跨操作系统支持**：套接字是基于网络协议的，遵循标准的网络通信规范，几乎所有的操作系统都提供了对套接字编程的支持。无论是 Linux、Windows 还是 macOS 等，都可以使用套接字进行进程间通信。这使得开发的应用程序具有更好的可移植性，能够在不同的操作系统环境下无缝运行。

### 可靠性与稳定性
+ **面向连接的通信**：TCP 套接字提供了面向连接的通信方式，通过三次握手建立可靠的连接，能够保证数据的可靠传输。在数据传输过程中，TCP 协议会进行数据确认、重传等操作，确保数据的完整性和顺序性。对于一些对数据准确性要求较高的应用场景，如金融交易系统、数据库同步等，使用 TCP 套接字可以有效避免数据丢失和错误。
+ **错误处理与恢复机制**：套接字通信具备完善的错误处理和恢复机制。当出现网络故障、连接中断等异常情况时，应用程序可以通过套接字的状态信息进行相应的处理，如重新连接、重试数据传输等。这种健壮性使得基于套接字的通信系统在复杂的网络环境中能够稳定运行。

### 灵活性与可扩展性
+ **自定义协议**：套接字允许开发者自定义通信协议，根据具体的业务需求设计消息格式、数据编码方式和交互流程。这种灵活性使得套接字可以适应各种不同的应用场景。例如，在物联网领域，不同的设备可能有不同的通信需求，开发者可以基于套接字设计适合设备特点的通信协议，实现设备与服务器之间的高效通信。
+ **可扩展性**：随着业务的发展和系统的升级，基于套接字的通信系统可以很容易地进行扩展。可以通过增加服务器节点、调整端口号、优化协议等方式来提高系统的性能和处理能力。例如，在一个大型的电商系统中，随着用户数量的增加，可以通过增加服务器并使用套接字进行负载均衡，来满足高并发的业务需求。

### 异步与并发处理能力
+ **异步 I/O 支持**：现代操作系统和编程语言都提供了对套接字异步 I/O 的支持。通过异步 I/O，进程可以在等待数据传输的同时继续执行其他任务，提高了系统的并发处理能力。例如，在一个高性能的 Web 服务器中，使用异步套接字可以同时处理大量的客户端请求，避免了传统同步 I/O 方式下的阻塞问题，提高了服务器的响应速度和吞吐量。
+ **多线程与多进程处理**：套接字可以与多线程或多进程技术结合使用，进一步提高系统的并发性能。多个线程或进程可以同时监听不同的套接字，处理多个客户端的连接和请求。例如，在一个聊天服务器中，可以使用多线程来处理每个客户端的连接，实现多个用户之间的实时通信。

### 安全性
+ **网络安全机制**：套接字通信可以与各种网络安全机制相结合，如 SSL/TLS 加密、防火墙等，提供数据传输的安全性。在传输敏感数据时，使用 SSL/TLS 加密的套接字可以防止数据在传输过程中被窃取或篡改。例如，在网上银行系统中，客户端与服务器之间的通信通常使用 SSL/TLS 加密的套接字，确保用户的账户信息和交易数据的安全。



> 更新: 2025-04-18 16:50:34  
> 原文: <https://www.yuque.com/linuxer/gscfv1/et9b2nnf6regbwdg>