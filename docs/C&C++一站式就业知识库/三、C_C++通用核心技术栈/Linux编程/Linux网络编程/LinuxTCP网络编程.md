# Linux TCP网络编程



## 目录
+ [概述](#概述)
+ [TCP协议基础](#tcp协议基础)
+ [套接字API](#套接字api)
+ [TCP服务器实现](#tcp服务器实现)
+ [TCP客户端实现](#tcp客户端实现)
+ [高级TCP编程技术](#高级tcp编程技术)
+ [常见问题与解决方案](#常见问题与解决方案)

## 概述
TCP（传输控制协议）是一种面向连接的、可靠的、基于字节流的传输层通信协议。它在IP协议之上，为应用程序提供可靠的数据传输服务。在Linux系统中，TCP网络编程主要通过套接字（Socket）API来实现。

原图片链接替换为以下TCP/IP协议栈的图表：

```plain
+---------------------+
|     应用层          |  HTTP, FTP, SMTP, SSH, DNS...
+---------------------+
|     传输层          |  TCP, UDP
+---------------------+
|     网络层          |  IP, ICMP, IGMP
+---------------------+
|   数据链路层        |  以太网, Wi-Fi, PPP...
+---------------------+
|     物理层          |  网线, 光纤, 无线电波...
+---------------------+
```

## TCP协议基础
### TCP的特点
+ **面向连接**：通信双方必须先建立连接，然后才能开始数据传输
+ **可靠传输**：保证数据无误地传送到目的地
+ **字节流服务**：把应用层传下来的数据看成一连串无结构的字节流
+ **全双工通信**：允许通信双方同时发送和接收数据
+ **具有拥塞控制**：防止网络过载

### TCP连接的三次握手
原图片链接替换为以下三次握手流程图：

```plain
客户端                                             服务器
   |                                                |
   |       SYN=1, seq=x                             |
   | ------------------------------------------>    |
   |                                                |
   |       SYN=1, ACK=1, seq=y, ack=x+1            |
   | <------------------------------------------    |
   |                                                |
   |       ACK=1, seq=x+1, ack=y+1                 |
   | ------------------------------------------>    |
   |                                                |
   |                    连接建立                     |
   |                                                |
```

1. **第一次握手**：客户端发送SYN=1，seq=x的包到服务器，进入SYN_SENT状态
2. **第二次握手**：服务器收到后，发送SYN=1，ACK=1，seq=y，ack=x+1的包，进入SYN_RCVD状态
3. **第三次握手**：客户端收到后，发送ACK=1，seq=x+1，ack=y+1的包，双方进入ESTABLISHED状态

### TCP连接的四次挥手
原图片链接替换为以下四次挥手流程图：

```plain
客户端                                             服务器
   |                                                |
   |            FIN=1, seq=u                        |
   | ------------------------------------------>    |
   |                                                |
   |            ACK=1, ack=u+1                      |
   | <------------------------------------------    |
   |                                                |
   |            FIN=1, ACK=1, seq=v, ack=u+1       |
   | <------------------------------------------    |
   |                                                |
   |            ACK=1, seq=u+1, ack=v+1            |
   | ------------------------------------------>    |
   |                                                |
   |                    连接关闭                     |
   |                                                |
```

1. **第一次挥手**：客户端发送FIN=1，seq=u的包，进入FIN_WAIT_1状态
2. **第二次挥手**：服务器收到后，发送ACK=1，ack=u+1的包，进入CLOSE_WAIT状态
3. **第三次挥手**：服务器发送FIN=1，ACK=1，seq=v，ack=u+1的包，进入LAST_ACK状态
4. **第四次挥手**：客户端收到后，发送ACK=1，seq=u+1，ack=v+1的包，进入TIME_WAIT状态

## 套接字API
Linux中的TCP网络编程主要使用以下套接字API：

### 创建套接字
```c
int socket(int domain, int type, int protocol);
```

**参数解析：**

+ `domain`：协议族，常用值为AF_INET（IPv4）或AF_INET6（IPv6）
+ `type`：套接字类型，TCP使用SOCK_STREAM
+ `protocol`：协议，通常为0，表示使用默认协议

**返回值解析：**

+ 成功：返回非负整数，表示套接字描述符
+ 失败：返回-1，errno被设置为相应的错误码

**函数说明：**

+ 创建一个新的套接字，返回套接字描述符
+ 套接字描述符类似于文件描述符，用于后续的网络操作
+ 创建后的套接字处于未连接状态，需要进一步绑定地址或连接

**示例：**

```c
int sockfd = socket(AF_INET, SOCK_STREAM, 0);
if (sockfd < 0) {
    perror("socket创建失败");
    exit(EXIT_FAILURE);
}
```

### 绑定地址
```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

**参数解析：**

+ `sockfd`：由socket()函数返回的套接字描述符
+ `addr`：指向要绑定的地址结构的指针
+ `addrlen`：地址结构的大小

**返回值解析：**

+ 成功：返回0
+ 失败：返回-1，errno被设置为相应的错误码

**函数说明：**

+ 将套接字与特定的IP地址和端口号绑定
+ 通常在服务器端使用，用于指定服务器监听的地址和端口
+ 客户端通常不需要显式调用bind()，系统会自动分配端口

**示例：**

```c
struct sockaddr_in server_addr;
memset(&server_addr, 0, sizeof(server_addr));
server_addr.sin_family = AF_INET;
server_addr.sin_addr.s_addr = htonl(INADDR_ANY); // 监听所有网络接口
server_addr.sin_port = htons(8080);              // 监听8080端口

if (bind(sockfd, (struct sockaddr*)&server_addr, sizeof(server_addr)) < 0) {
    perror("bind失败");
    exit(EXIT_FAILURE);
}
```

### 监听连接
```c
int listen(int sockfd, int backlog);
```

**参数解析：**

+ `sockfd`：要监听的套接字描述符
+ `backlog`：挂起连接队列的最大长度

**返回值解析：**

+ 成功：返回0
+ 失败：返回-1，errno被设置为相应的错误码

**函数说明：**

+ 将套接字标记为被动套接字，用于接受传入连接
+ 只有服务器需要调用listen()
+ backlog参数指定了系统允许排队的未完成连接请求数

**示例：**

```c
if (listen(sockfd, 5) < 0) {
    perror("listen失败");
    exit(EXIT_FAILURE);
}
```

### 接受连接
```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

**参数解析：**

+ `sockfd`：监听套接字描述符
+ `addr`：用于返回客户端地址信息的结构体指针
+ `addrlen`：输入参数时表示addr的大小，输出参数时表示实际地址大小

**返回值解析：**

+ 成功：返回一个新的套接字描述符，用于与客户端通信
+ 失败：返回-1，errno被设置为相应的错误码

**函数说明：**

+ 从已完成连接队列中提取第一个连接请求，创建一个新的套接字
+ 如果队列为空，accept()会阻塞等待连接到来
+ 原始的监听套接字保持打开状态，可以继续接受新的连接

**示例：**

```c
struct sockaddr_in client_addr;
socklen_t client_len = sizeof(client_addr);
int client_sockfd = accept(sockfd, (struct sockaddr*)&client_addr, &client_len);

if (client_sockfd < 0) {
    perror("accept失败");
    exit(EXIT_FAILURE);
}

// 获取客户端IP地址和端口
char client_ip[INET_ADDRSTRLEN];
inet_ntop(AF_INET, &(client_addr.sin_addr), client_ip, INET_ADDRSTRLEN);
int client_port = ntohs(client_addr.sin_port);
printf("接受来自 %s:%d 的连接\n", client_ip, client_port);
```

### 连接服务器
```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

**参数解析：**

+ `sockfd`：客户端套接字描述符
+ `addr`：包含服务器地址信息的结构体指针
+ `addrlen`：地址结构的大小

**返回值解析：**

+ 成功：返回0
+ 失败：返回-1，errno被设置为相应的错误码

**函数说明：**

+ 将未连接的套接字连接到指定地址的服务器
+ 仅由客户端调用，用于建立与服务器的连接
+ 调用connect()会触发TCP三次握手过程

**示例：**

```c
struct sockaddr_in server_addr;
memset(&server_addr, 0, sizeof(server_addr));
server_addr.sin_family = AF_INET;
server_addr.sin_port = htons(8080);

inet_pton(AF_INET, "127.0.0.1", &server_addr.sin_addr);

if (connect(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
    perror("连接失败");
    return -1;
}
```

### 数据传输
```c
ssize_t send(int sockfd, const void *buf, size_t len, int flags);
ssize_t recv(int sockfd, void *buf, size_t len, int flags);
```

**参数解析（send）：**

+ `sockfd`：已连接的套接字描述符
+ `buf`：要发送的数据缓冲区
+ `len`：要发送的数据长度
+ `flags`：控制发送行为的标志，通常为0

**参数解析（recv）：**

+ `sockfd`：已连接的套接字描述符
+ `buf`：接收数据的缓冲区
+ `len`：缓冲区的大小
+ `flags`：控制接收行为的标志，通常为0

**返回值解析：**

+ 成功：返回实际发送或接收的字节数
+ 失败：返回-1，errno被设置为相应的错误码
+ 对于recv()，如果连接已关闭，返回0

**函数说明：**

+ send()尝试将数据发送到已连接的套接字
+ recv()从已连接的套接字接收数据
+ 这些函数可能会发送或接收比请求的更少的字节
+ 可以使用循环来确保完全发送或接收所需的数据

**示例：**

```c
// 发送数据
char *message = "Hello from client";
ssize_t bytes_sent = send(sockfd, message, strlen(message), 0);
if (bytes_sent < 0) {
    perror("send失败");
} else {
    printf("成功发送 %ld 字节数据\n", bytes_sent);
}

// 接收数据
char buffer[1024] = {0};
ssize_t bytes_received = recv(sockfd, buffer, sizeof(buffer) - 1, 0);
if (bytes_received < 0) {
    perror("recv失败");
} else if (bytes_received == 0) {
    printf("连接已关闭\n");
} else {
    buffer[bytes_received] = '\0';  // 确保字符串以null结尾
    printf("接收到 %ld 字节数据: %s\n", bytes_received, buffer);
}
```

### 关闭连接
```c
int close(int fd);
```

**参数解析：**

+ `fd`：要关闭的套接字描述符

**返回值解析：**

+ 成功：返回0
+ 失败：返回-1，errno被设置为相应的错误码

**函数说明：**

+ 关闭套接字，释放相关资源
+ 调用close()会启动TCP的正常关闭序列（四次挥手）
+ 如果多个进程共享同一个套接字，close()只会减少引用计数

**示例：**

```c
if (close(sockfd) < 0) {
    perror("close失败");
}
```

## TCP服务器实现
### 基本流程
以下是TCP服务器的基本工作流程：

```plain
+----------------+     +----------------+     +----------------+
| 创建套接字      |     | 绑定地址和端口  |     | 监听连接       |
| socket()       |---->| bind()         |---->| listen()       |
+----------------+     +----------------+     +----------------+
        |                                            |
        |                                            v
+----------------+     +----------------+     +----------------+
| 关闭连接        |<----| 数据收发        |<----| 接受客户端连接  |
| close()        |     | send()/recv()  |     | accept()       |
+----------------+     +----------------+     +----------------+
```

1. 创建套接字（socket）
2. 绑定地址和端口（bind）
3. 监听连接（listen）
4. 接受客户端连接（accept）
5. 数据收发（send/recv）
6. 关闭连接（close）

### 实例代码
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int server_fd, new_socket;
    struct sockaddr_in address;
    int opt = 1;
    int addrlen = sizeof(address);
    char buffer[BUFFER_SIZE] = {0};
    
    // 创建套接字
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }
    
    // 设置套接字选项
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, 
                   &opt, sizeof(opt))) {
        perror("setsockopt");
        exit(EXIT_FAILURE);
    }
    
    // 配置地址
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);
    
    // 绑定套接字
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }
    
    // 监听
    if (listen(server_fd, 3) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }
    
    printf("Server listening on port %d\n", PORT);
    
    // 接受连接并处理
    while(1) {
        printf("Waiting for connection...\n");
        
        if ((new_socket = accept(server_fd, (struct sockaddr *)&address, 
                                 (socklen_t*)&addrlen)) < 0) {
            perror("accept");
            exit(EXIT_FAILURE);
        }
        
        char client_ip[INET_ADDRSTRLEN];
        inet_ntop(AF_INET, &(address.sin_addr), client_ip, INET_ADDRSTRLEN);
        printf("Connection accepted from %s:%d\n", client_ip, ntohs(address.sin_port));
        
        // 读取客户端消息
        read(new_socket, buffer, BUFFER_SIZE);
        printf("Message from client: %s\n", buffer);
        
        // 发送响应
        char *message = "Hello from server";
        send(new_socket, message, strlen(message), 0);
        printf("Response sent to client\n");
        
        close(new_socket);
    }
    
    return 0;
}
```

## TCP客户端实现
### 基本流程
以下是TCP客户端的基本工作流程：

```plain
+----------------+     +----------------+     +----------------+
| 创建套接字      |     | 连接服务器      |     | 数据收发       |
| socket()       |---->| connect()      |---->| send()/recv()  |
+----------------+     +----------------+     +----------------+
                                                      |
                                                      v
                                              +----------------+
                                              | 关闭连接        |
                                              | close()        |
                                              +----------------+
```

1. 创建套接字（socket）
2. 连接服务器（connect）
3. 数据收发（send/recv）
4. 关闭连接（close）

### 实例代码
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int sock = 0;
    struct sockaddr_in serv_addr;
    char buffer[BUFFER_SIZE] = {0};
    
    // 创建套接字
    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
        perror("Socket creation error");
        return -1;
    }
    
    // 配置服务器地址
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(PORT);
    
    // 将IP转换为网络地址结构
    if (inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr) <= 0) {
        perror("Invalid address / Address not supported");
        return -1;
    }
    
    // 连接服务器
    if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
        perror("Connection Failed");
        return -1;
    }
    
    // 发送消息
    char *message = "Hello from client";
    send(sock, message, strlen(message), 0);
    printf("Message sent to server\n");
    
    // 接收响应
    read(sock, buffer, BUFFER_SIZE);
    printf("Response from server: %s\n", buffer);
    
    // 关闭连接
    close(sock);
    
    return 0;
}
```

### TCP客户端服务器交互流程图
以下是TCP客户端和服务器完整的交互流程：

```plain
客户端                                             服务器
   |                                                |
   |  创建套接字 socket()                            |  创建套接字 socket()
   |                                                |
   |                                                |  绑定地址 bind()
   |                                                |
   |                                                |  监听连接 listen()
   |                                                |
   |  连接服务器 connect()        --------------->   |  接受连接 accept()
   |  (TCP三次握手)                                  |
   |                                                |
   |  发送数据 send()            --------------->   |  接收数据 recv()
   |                                                |
   |  接收数据 recv()            <---------------   |  发送数据 send()
   |                                                |
   |  关闭连接 close()           --------------->   |  关闭连接 close()
   |  (TCP四次挥手)                                  |
   |                                                |
```

## 高级TCP编程技术
### 非阻塞IO
通过设置套接字为非阻塞模式，使得调用read()、write()等函数时不会被阻塞，而是立即返回：

```c
int flags = fcntl(sockfd, F_GETFL, 0);
fcntl(sockfd, F_SETFL, flags | O_NONBLOCK);
```

### 多路复用
使用select()、poll()或epoll()实现一个进程同时处理多个连接：

原图片链接替换为以下IO多路复用模型图：

```plain
                   +-------------+  
                   |  应用程序    |  
                   +-------------+  
                         |
                         v
+----------+    +----------------+    +----------+
| 套接字1   | <- |                | -> | 套接字3   |
+----------+    |  多路复用器     |    +----------+
                |  select/poll/  |
+----------+    |  epoll         |    +----------+
| 套接字2   | <- |                | -> | 套接字4   |
+----------+    +----------------+    +----------+
                         |
                         v
                   +-------------+
                   |   内核空间   |
                   +-------------+
```

#### select示例
下面是一个完整的使用select实现的TCP服务器示例，可以同时处理多个客户端连接：

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#include <sys/time.h>
#include <sys/select.h>

#define PORT 8080
#define MAX_CLIENTS 30
#define BUFFER_SIZE 1024

int main() {
    int master_socket, addrlen, new_socket, client_sockets[MAX_CLIENTS],
        max_clients = MAX_CLIENTS, activity, i, valread, sd;
    int max_sd;
    struct sockaddr_in address;
    
    char buffer[BUFFER_SIZE];
    
    // 初始化客户端套接字数组
    for (i = 0; i < max_clients; i++) {
        client_sockets[i] = 0;
    }
    
    // 创建主套接字
    if ((master_socket = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }
    
    // 设置主套接字为可重用端口
    int opt = 1;
    if (setsockopt(master_socket, SOL_SOCKET, SO_REUSEADDR, (char *)&opt, sizeof(opt)) < 0) {
        perror("setsockopt");
        exit(EXIT_FAILURE);
    }
    
    // 设置地址
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);
    
    // 绑定套接字
    if (bind(master_socket, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }
    
    // 监听
    if (listen(master_socket, 3) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }
    
    // 接受传入连接
    addrlen = sizeof(address);
    printf("Waiting for connections on port %d...\n", PORT);
    
    while (1) {
        // 清空描述符集
        fd_set readfds;
        FD_ZERO(&readfds);
        
        // 添加主套接字到描述符集
        FD_SET(master_socket, &readfds);
        max_sd = master_socket;
        
        // 添加子套接字到描述符集
        for (i = 0; i < max_clients; i++) {
            // 如果套接字描述符有效，添加到集合
            sd = client_sockets[i];
            if (sd > 0) {
                FD_SET(sd, &readfds);
            }
            
            // 记录最大的套接字描述符
            if (sd > max_sd) {
                max_sd = sd;
            }
        }
        
        // 等待活动（无超时设置）
        activity = select(max_sd + 1, &readfds, NULL, NULL, NULL);
        
        if ((activity < 0) && (errno != EINTR)) {
            perror("select error");
        }
        
        // 检查主套接字是否有数据可读，意味着有传入连接
        if (FD_ISSET(master_socket, &readfds)) {
            if ((new_socket = accept(master_socket, (struct sockaddr *)&address, (socklen_t*)&addrlen)) < 0) {
                perror("accept");
                exit(EXIT_FAILURE);
            }
            
            // 打印新连接信息
            char client_ip[INET_ADDRSTRLEN];
            inet_ntop(AF_INET, &(address.sin_addr), client_ip, INET_ADDRSTRLEN);
            printf("New connection from %s:%d, socket fd is %d\n",
                   client_ip, ntohs(address.sin_port), new_socket);
            
            // 发送欢迎消息
            char *welcome_message = "Welcome to the TCP server\r\n";
            if (send(new_socket, welcome_message, strlen(welcome_message), 0) != strlen(welcome_message)) {
                perror("send");
            }
            
            // 将新socket添加到客户端套接字数组
            for (i = 0; i < max_clients; i++) {
                if (client_sockets[i] == 0) {
                    client_sockets[i] = new_socket;
                    printf("Adding to list of sockets as %d\n", i);
                    break;
                }
            }
        }
        
        // 检查是否有客户端套接字有数据可读
        for (i = 0; i < max_clients; i++) {
            sd = client_sockets[i];
            
            if (FD_ISSET(sd, &readfds)) {
                // 检查是否是关闭连接
                if ((valread = read(sd, buffer, BUFFER_SIZE)) == 0) {
                    // 获取断开连接的客户端信息
                    getpeername(sd, (struct sockaddr*)&address, (socklen_t*)&addrlen);
                    char client_ip[INET_ADDRSTRLEN];
                    inet_ntop(AF_INET, &(address.sin_addr), client_ip, INET_ADDRSTRLEN);
                    printf("Client disconnected: %s:%d\n",
                           client_ip, ntohs(address.sin_port));
                    
                    // 关闭套接字并从列表中删除
                    close(sd);
                    client_sockets[i] = 0;
                } else {
                    // 处理客户端数据
                    buffer[valread] = '\0';
                    
                    // 获取客户端信息
                    getpeername(sd, (struct sockaddr*)&address, (socklen_t*)&addrlen);
                    char client_ip[INET_ADDRSTRLEN];
                    inet_ntop(AF_INET, &(address.sin_addr), client_ip, INET_ADDRSTRLEN);
                    
                    printf("Received from %s:%d: %s\n", 
                           client_ip, ntohs(address.sin_port), buffer);
                    
                    // 回显消息给客户端
                    char response[BUFFER_SIZE + 32];
                    sprintf(response, "Server echo: %s", buffer);
                    send(sd, response, strlen(response), 0);
                }
            }
        }
    }
    
    return 0;
}
```

#### poll示例
以下是使用poll()函数实现的多路复用服务器示例：

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#include <poll.h>
#include <errno.h>

#define PORT 8080
#define MAX_CLIENTS 30
#define BUFFER_SIZE 1024

int main() {
    int master_socket, new_socket, addrlen, activity, i, valread;
    struct sockaddr_in address;
    char buffer[BUFFER_SIZE];
    
    // 创建poll结构数组
    struct pollfd fds[MAX_CLIENTS + 1];
    int nfds = 1;
    
    // 创建主socket
    if ((master_socket = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }
    
    // 设置socket选项
    int opt = 1;
    if (setsockopt(master_socket, SOL_SOCKET, SO_REUSEADDR, (char *)&opt, sizeof(opt)) < 0) {
        perror("setsockopt");
        exit(EXIT_FAILURE);
    }
    
    // 配置地址
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);
    
    // 绑定socket
    if (bind(master_socket, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }
    
    // 监听
    if (listen(master_socket, 3) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }
    
    // 初始化poll结构，添加主socket
    fds[0].fd = master_socket;
    fds[0].events = POLLIN;
    
    // 初始化其余数组位置
    for (i = 1; i <= MAX_CLIENTS; i++) {
        fds[i].fd = -1;
    }
    
    addrlen = sizeof(address);
    printf("TCP Server listening on port %d\n", PORT);
    
    while (1) {
        // 调用poll()
        activity = poll(fds, nfds, -1);
        
        if (activity < 0) {
            perror("poll error");
            exit(EXIT_FAILURE);
        }
        
        // 检查主socket是否有事件
        if (fds[0].revents & POLLIN) {
            if ((new_socket = accept(master_socket, (struct sockaddr *)&address, (socklen_t*)&addrlen)) < 0) {
                perror("accept");
                exit(EXIT_FAILURE);
            }
            
            // 获取客户端信息
            char client_ip[INET_ADDRSTRLEN];
            inet_ntop(AF_INET, &(address.sin_addr), client_ip, INET_ADDRSTRLEN);
            printf("New connection from %s:%d, socket fd is %d\n",
                   client_ip, ntohs(address.sin_port), new_socket);
            
            // 发送欢迎消息
            char *welcome_message = "Welcome to TCP server (poll)\r\n";
            if (send(new_socket, welcome_message, strlen(welcome_message), 0) != strlen(welcome_message)) {
                perror("send");
            }
            
            // 添加到poll数组
            for (i = 1; i <= MAX_CLIENTS; i++) {
                if (fds[i].fd < 0) {
                    fds[i].fd = new_socket;
                    fds[i].events = POLLIN;
                    printf("Adding to poll array at position %d\n", i);
                    
                    // 更新需要poll的最大索引
                    if (i + 1 > nfds) {
                        nfds = i + 1;
                    }
                    break;
                }
            }
            
            if (i > MAX_CLIENTS) {
                printf("Too many connections\n");
                close(new_socket);
            }
        }
        
        // 检查客户端套接字是否有数据
        for (i = 1; i < nfds; i++) {
            if (fds[i].fd >= 0 && (fds[i].revents & POLLIN)) {
                // 处理来自客户端的数据
                if ((valread = read(fds[i].fd, buffer, BUFFER_SIZE)) <= 0) {
                    // 连接关闭或错误
                    getpeername(fds[i].fd, (struct sockaddr*)&address, (socklen_t*)&addrlen);
                    char client_ip[INET_ADDRSTRLEN];
                    inet_ntop(AF_INET, &(address.sin_addr), client_ip, INET_ADDRSTRLEN);
                    
                    printf("Client disconnected: %s:%d\n",
                           client_ip, ntohs(address.sin_port));
                    
                    // 关闭socket并移除
                    close(fds[i].fd);
                    fds[i].fd = -1;
                } else {
                    // 收到数据
                    buffer[valread] = '\0';
                    
                    // 获取客户端信息
                    getpeername(fds[i].fd, (struct sockaddr*)&address, (socklen_t*)&addrlen);
                    char client_ip[INET_ADDRSTRLEN];
                    inet_ntop(AF_INET, &(address.sin_addr), client_ip, INET_ADDRSTRLEN);
                    
                    printf("Received from %s:%d: %s\n", 
                           client_ip, ntohs(address.sin_port), buffer);
                    
                    // 回显消息
                    char response[BUFFER_SIZE + 32];
                    sprintf(response, "Server echo (poll): %s", buffer);
                    send(fds[i].fd, response, strlen(response), 0);
                }
            }
        }
    }
    
    return 0;
}
```

#### epoll示例
以下是使用epoll实现的完整服务器示例，epoll在处理大量连接时比select和poll更高效：

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#include <sys/epoll.h>
#include <errno.h>
#include <fcntl.h>

#define PORT 8080
#define MAX_EVENTS 64
#define BUFFER_SIZE 1024

// 设置套接字为非阻塞模式
int set_nonblocking(int sockfd) {
    int flags = fcntl(sockfd, F_GETFL, 0);
    if (flags == -1) {
        perror("fcntl F_GETFL");
        return -1;
    }
    
    if (fcntl(sockfd, F_SETFL, flags | O_NONBLOCK) == -1) {
        perror("fcntl F_SETFL O_NONBLOCK");
        return -1;
    }
    
    return 0;
}

int main() {
    int server_fd, new_socket, epoll_fd, event_count, i;
    struct sockaddr_in address;
    int addrlen = sizeof(address);
    char buffer[BUFFER_SIZE];
    
    struct epoll_event ev, events[MAX_EVENTS];
    
    // 创建服务器套接字
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }
    
    // 设置套接字选项
    int opt = 1;
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, &opt, sizeof(opt))) {
        perror("setsockopt");
        exit(EXIT_FAILURE);
    }
    
    // 设置非阻塞
    set_nonblocking(server_fd);
    
    // 配置地址
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);
    
    // 绑定套接字
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }
    
    // 监听
    if (listen(server_fd, 10) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }
    
    // 创建epoll实例
    epoll_fd = epoll_create1(0);
    if (epoll_fd == -1) {
        perror("epoll_create1");
        exit(EXIT_FAILURE);
    }
    
    // 添加服务器套接字到epoll
    ev.events = EPOLLIN;
    ev.data.fd = server_fd;
    if (epoll_ctl(epoll_fd, EPOLL_CTL_ADD, server_fd, &ev) == -1) {
        perror("epoll_ctl: server_fd");
        exit(EXIT_FAILURE);
    }
    
    printf("TCP Server listening on port %d (epoll)\n", PORT);
    
    while (1) {
        // 等待事件
        event_count = epoll_wait(epoll_fd, events, MAX_EVENTS, -1);
        if (event_count == -1) {
            perror("epoll_wait");
            exit(EXIT_FAILURE);
        }
        
        // 处理就绪事件
        for (i = 0; i < event_count; i++) {
            // 检查是否有新连接
            if (events[i].data.fd == server_fd) {
                // 处理所有待处理的连接
                while (1) {
                    new_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen);
                    if (new_socket == -1) {
                        if (errno == EAGAIN || errno == EWOULDBLOCK) {
                            // 所有连接都已处理
                            break;
                        } else {
                            perror("accept");
                            break;
                        }
                    }
                    
                    // 获取客户端信息
                    char client_ip[INET_ADDRSTRLEN];
                    inet_ntop(AF_INET, &address.sin_addr, client_ip, INET_ADDRSTRLEN);
                    printf("New connection from %s:%d\n", client_ip, ntohs(address.sin_port));
                    
                    // 设置新套接字为非阻塞
                    set_nonblocking(new_socket);
                    
                    // 添加新连接到epoll
                    ev.events = EPOLLIN | EPOLLET; // 设置为边缘触发模式
                    ev.data.fd = new_socket;
                    if (epoll_ctl(epoll_fd, EPOLL_CTL_ADD, new_socket, &ev) == -1) {
                        perror("epoll_ctl: new_socket");
                        close(new_socket);
                        continue;
                    }
                    
                    // 发送欢迎消息
                    char *welcome_message = "Welcome to TCP server (epoll)\r\n";
                    if (send(new_socket, welcome_message, strlen(welcome_message), 0) != strlen(welcome_message)) {
                        perror("send");
                    }
                }
            } else {
                // 处理客户端数据
                int client_fd = events[i].data.fd;
                
                // 读取所有数据
                ssize_t bytes_read;
                while ((bytes_read = read(client_fd, buffer, BUFFER_SIZE - 1)) > 0) {
                    buffer[bytes_read] = '\0';
                    
                    // 获取客户端信息
                    struct sockaddr_in client_info;
                    socklen_t client_len = sizeof(client_info);
                    getpeername(client_fd, (struct sockaddr*)&client_info, &client_len);
                    
                    char client_ip[INET_ADDRSTRLEN];
                    inet_ntop(AF_INET, &client_info.sin_addr, client_ip, INET_ADDRSTRLEN);
                    printf("Received from %s:%d: %s\n", 
                           client_ip, ntohs(client_info.sin_port), buffer);
                    
                    // 回显消息
                    char response[BUFFER_SIZE + 32];
                    sprintf(response, "Server echo (epoll): %s", buffer);
                    send(client_fd, response, strlen(response), 0);
                }
                
                if (bytes_read == 0) {
                    // 客户端关闭连接
                    struct sockaddr_in client_info;
                    socklen_t client_len = sizeof(client_info);
                    getpeername(client_fd, (struct sockaddr*)&client_info, &client_len);
                    
                    char client_ip[INET_ADDRSTRLEN];
                    inet_ntop(AF_INET, &client_info.sin_addr, client_ip, INET_ADDRSTRLEN);
                    printf("Client disconnected: %s:%d\n", 
                           client_ip, ntohs(client_info.sin_port));
                    
                    // 从epoll中移除套接字
                    epoll_ctl(epoll_fd, EPOLL_CTL_DEL, client_fd, NULL);
                    close(client_fd);
                } else if (bytes_read < 0) {
                    if (errno != EAGAIN && errno != EWOULDBLOCK) {
                        perror("read");
                        // 清理错误的套接字
                        epoll_ctl(epoll_fd, EPOLL_CTL_DEL, client_fd, NULL);
                        close(client_fd);
                    }
                }
            }
        }
    }
    
    close(server_fd);
    close(epoll_fd);
    
    return 0;
}
```

### 多线程服务器
创建工作线程处理每个连接：

```c
pthread_t thread_id;
if (pthread_create(&thread_id, NULL, client_handler, (void*)&client_sock) < 0) {
    perror("could not create thread");
    return 1;
}
```

处理函数：

```c
void *client_handler(void *socket_desc) {
    int sock = *(int*)socket_desc;
    char message[2000], client_message[2000];
    
    // 接收客户端消息
    while ((read_size = recv(sock, client_message, 2000, 0)) > 0) {
        // 处理消息
        // 发送响应
        write(sock, message, strlen(message));
    }
    
    close(sock);
    return NULL;
}
```

#### 完整的多线程服务器示例
以下是一个完整的多线程TCP服务器示例，为每个客户端连接创建一个新线程：

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#include <pthread.h>

#define PORT 8080
#define BUFFER_SIZE 1024
#define MAX_CLIENTS 100

// 用于传递给线程的客户端信息结构
typedef struct {
    int socket;
    struct sockaddr_in address;
    int id;
} client_t;

// 活跃客户端数量
int client_count = 0;
pthread_mutex_t client_count_mutex = PTHREAD_MUTEX_INITIALIZER;

// 客户端处理函数
void *handle_client(void *arg) {
    // 获取客户端数据
    client_t *client = (client_t *)arg;
    int client_sock = client->socket;
    int client_id = client->id;
    
    // 获取客户端IP
    char client_ip[INET_ADDRSTRLEN];
    inet_ntop(AF_INET, &(client->address.sin_addr), client_ip, INET_ADDRSTRLEN);
    int client_port = ntohs(client->address.sin_port);
    
    printf("Client #%d connected from %s:%d\n", client_id, client_ip, client_port);
    
    // 发送欢迎消息
    char welcome[BUFFER_SIZE];
    sprintf(welcome, "Welcome to the server, client #%d! Type 'exit' to quit.\r\n", client_id);
    if (send(client_sock, welcome, strlen(welcome), 0) < 0) {
        perror("Failed to send welcome message");
    }
    
    // 接收和处理客户端消息
    char buffer[BUFFER_SIZE];
    int read_size;
    
    while ((read_size = recv(client_sock, buffer, BUFFER_SIZE - 1, 0)) > 0) {
        // 确保数据以null结尾
        buffer[read_size] = '\0';
        
        // 检查客户端是否要退出
        if (strncmp(buffer, "exit", 4) == 0) {
            printf("Client #%d requested to exit\n", client_id);
            break;
        }
        
        printf("Message from client #%d: %s\n", client_id, buffer);
        
        // 准备响应消息
        char response[BUFFER_SIZE];
        sprintf(response, "Server received your message: %s", buffer);
        
        // 发送响应
        if (send(client_sock, response, strlen(response), 0) < 0) {
            perror("Send failed");
            break;
        }
    }
    
    // 检查连接关闭或错误
    if (read_size == 0) {
        printf("Client #%d disconnected\n", client_id);
    } else if (read_size == -1) {
        perror("Receive failed");
    }
    
    // 关闭客户端套接字
    close(client_sock);
    
    // 更新活跃客户端计数
    pthread_mutex_lock(&client_count_mutex);
    client_count--;
    printf("Active clients: %d\n", client_count);
    pthread_mutex_unlock(&client_count_mutex);
    
    // 释放客户端结构体内存
    free(client);
    
    return NULL;
}

// 处理服务器终止的信号
void handle_sigint(int sig) {
    printf("\nShutting down server...\n");
    exit(0);
}

int main() {
    int server_fd;
    struct sockaddr_in address;
    int addrlen = sizeof(address);
    
    // 创建套接字
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("Socket creation failed");
        exit(EXIT_FAILURE);
    }
    
    // 设置套接字选项
    int opt = 1;
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, &opt, sizeof(opt))) {
        perror("setsockopt failed");
        exit(EXIT_FAILURE);
    }
    
    // 设置服务器地址结构
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);
    
    // 绑定套接字
    if (bind(server_fd, (struct sockaddr *)&address, addrlen) < 0) {
        perror("Bind failed");
        exit(EXIT_FAILURE);
    }
    
    // 监听
    if (listen(server_fd, 10) < 0) {
        perror("Listen failed");
        exit(EXIT_FAILURE);
    }
    
    printf("Server started on port %d\n", PORT);
    printf("Waiting for connections...\n");
    
    // 设置信号处理
    signal(SIGINT, handle_sigint);
    
    // 主循环，等待并处理客户端连接
    while (1) {
        // 客户端地址结构
        struct sockaddr_in client_addr;
        socklen_t client_len = sizeof(client_addr);
        
        // 接受客户端连接
        int client_sock = accept(server_fd, (struct sockaddr *)&client_addr, &client_len);
        if (client_sock < 0) {
            perror("Accept failed");
            continue;
        }
        
        // 检查最大客户端限制
        pthread_mutex_lock(&client_count_mutex);
        if (client_count >= MAX_CLIENTS) {
            pthread_mutex_unlock(&client_count_mutex);
            printf("Max clients reached. Connection rejected.\n");
            
            char *msg = "Server is full. Please try again later.\r\n";
            send(client_sock, msg, strlen(msg), 0);
            close(client_sock);
            continue;
        }
        
        // 更新客户端计数
        client_count++;
        int client_id = client_count;
        printf("Active clients: %d\n", client_count);
        pthread_mutex_unlock(&client_count_mutex);
        
        // 创建客户端结构，传递给线程
        client_t *client = (client_t *)malloc(sizeof(client_t));
        if (client == NULL) {
            perror("Failed to allocate memory for client");
            close(client_sock);
            continue;
        }
        
        client->socket = client_sock;
        client->address = client_addr;
        client->id = client_id;
        
        // 创建新线程处理客户端
        pthread_t thread_id;
        if (pthread_create(&thread_id, NULL, handle_client, (void *)client) < 0) {
            perror("Could not create thread");
            free(client);
            close(client_sock);
            
            // 更新计数
            pthread_mutex_lock(&client_count_mutex);
            client_count--;
            pthread_mutex_unlock(&client_count_mutex);
            
            continue;
        }
        
        // 分离线程，不需要等待它结束
        pthread_detach(thread_id);
        printf("New thread created for client #%d\n", client_id);
    }
    
    // 关闭服务器套接字
    close(server_fd);
    
    return 0;
}
```

这个多线程服务器示例具有以下特点：

1. **并发处理多个客户端**：每个连接都在独立的线程中处理
2. **线程分离**：使用`pthread_detach`使线程独立运行，避免资源泄露
3. **互斥锁**：使用互斥锁保护共享资源（活跃客户端计数）
4. **客户端限制**：实现了最大客户端数量限制
5. **优雅退出**：处理SIGINT信号实现优雅退出
6. **内存管理**：为每个客户端分配和释放资源

####  
 

## 常见问题与解决方案
### 1. 端口已被占用
**问题**：bind()失败，显示"Address already in use"

**解决方案**：

+ 等待端口释放（通常需要等待TIME_WAIT状态结束）
+ 使用setsockopt()设置SO_REUSEADDR选项：

```c
int opt = 1;
setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt));
```

### 2. 连接重置
**问题**：send()或recv()失败，显示"Connection reset by peer"

**解决方案**：

+ 检查对方是否关闭连接
+ 实现心跳机制检测连接状态
+ 适当处理网络异常情况

### 3. 套接字阻塞
**问题**：程序在read()或accept()调用处停止响应

**解决方案**：

+ 使用非阻塞IO
+ 使用多线程处理连接
+ 使用select()/poll()/epoll()进行IO多路复用

### 4. 粘包问题
**问题**：TCP是流协议，多个发送的数据包可能被接收方当作一个数据包接收

**解决方案**：

+ 固定长度包
+ 特殊分隔符标记包边界
+ 包头指定包长度
+ 应用层协议规定边界



> 更新: 2025-04-18 17:36:38  
> 原文: <https://www.yuque.com/linuxer/gscfv1/nmn4yqmng7xbxi18>