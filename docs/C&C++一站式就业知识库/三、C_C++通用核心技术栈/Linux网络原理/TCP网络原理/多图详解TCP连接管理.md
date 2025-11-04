# 多图详解 TCP 连接管理

原文链接：[多图详解 TCP 连接管理，太全了！！！ - 程序员cxuan - 博客园](https://www.cnblogs.com/cxuanBlog/p/14679563.html)



<font style="color:rgb(51, 51, 51);">TCP 是一种面向连接的</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">单播</font><font style="color:rgb(51, 51, 51);">协议，在 TCP 中，并不存在多播、广播的这种行为，因为 TCP 报文段中能明确发送方和接受方的 IP 地址。</font>

<font style="color:rgb(51, 51, 51);">在发送数据前，相互通信的双方（即发送方和接受方）需要建立一条</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">连接</font><font style="color:rgb(51, 51, 51);">，在发送数据后，通信双方需要断开连接，这就是 TCP 连接的建立和终止。</font>

## <font style="color:rgb(51, 51, 51);">TCP 连接的建立和终止</font>
<font style="color:rgb(51, 51, 51);">如果你看过我之前写的关于网络层的一篇文章，你应该知道 TCP 的基本元素有四个：即</font>**<font style="color:rgb(51, 51, 51);">发送方的 IP 地址、发送方的端口号、接收方的 IP 地址、接收方的端口号</font>**<font style="color:rgb(51, 51, 51);">。而每一方的 IP + 端口号都可以看作是一个</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">套接字</font><font style="color:rgb(51, 51, 51);">，套接字能够被唯一标示。套接字就相当于是门，出了这个门，就要进行数据传输了。</font>

<font style="color:rgb(51, 51, 51);">TCP 的连接建立 -> 终止总共分为三个阶段</font>

![1714988763214-190930cd-e579-4960-886b-aae5759a963e.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763214-190930cd-e579-4960-886b-aae5759a963e-046868.png)

<font style="color:rgb(51, 51, 51);">下面我们所讨论的重点也是集中在这三个层面。</font>

<font style="color:rgb(51, 51, 51);">下图是一个非常典型的 TCP 连接的建立和关闭过程，其中不包括数据传输的部分。</font>

### <font style="color:rgb(51, 51, 51);">TCP 建立连接 - 三次握手</font>
![1714988763264-df28bd42-6109-4a08-ac48-f474c2a99051.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763264-df28bd42-6109-4a08-ac48-f474c2a99051-182388.png)

1. <font style="color:rgb(51, 51, 51);">服务端进程准备好接收来自外部的 TCP 连接，一般情况下是调用 bind、listen、socket 三个函数完成。这种打开方式被认为是</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">被动打开(passive open)</font><font style="color:rgb(51, 51, 51);">。然后服务端进程处于</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">LISTEN</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态，等待客户端连接请求。</font>
2. <font style="color:rgb(51, 51, 51);">客户端通过</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">connect</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">发起</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">主动打开(active open)</font><font style="color:rgb(51, 51, 51);">，向服务器发出连接请求，请求中首部同步位 SYN = 1，同时选择一个初始序号 sequence ，简写 seq = x。SYN 报文段不允许携带数据，只消耗一个序号。此时，客户端进入</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">SYN-SEND</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态。</font>
3. <font style="color:rgb(51, 51, 51);">服务器收到客户端连接后，，需要确认客户端的报文段。在确认报文段中，把 SYN 和 ACK 位都置为 1 。确认号是 ack = x + 1，同时也为自己选择一个初始序号 seq = y。这个报文段也不能携带数据，但同样要消耗掉一个序号。此时，TCP 服务器进入</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">SYN-RECEIVED(同步收到)</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态。</font>
4. <font style="color:rgb(51, 51, 51);">客户端在收到服务器发出的响应后，还需要给出确认连接。确认连接中的 ACK 置为 1 ，序号为 seq = x + 1，确认号为 ack = y + 1。TCP 规定，这个报文段可以携带数据也可以不携带数据，如果不携带数据，那么下一个数据报文段的序号仍是 seq = x + 1。这时，客户端进入</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">ESTABLISHED (已连接)</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态</font>
5. <font style="color:rgb(51, 51, 51);">服务器收到客户的确认后，也进入</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">ESTABLISHED</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态。</font>

<font style="color:rgb(51, 51, 51);">这是一个典型的三次握手过程，通过上面 3 个报文段就能够完成一个 TCP 连接的建立。三次握手的的目的不仅仅在于让通信双方知晓正在建立一个连接，</font>**<font style="color:rgb(51, 51, 51);">也在于利用数据包中的选项字段来交换一些特殊信息，交换初始序列号</font>**<font style="color:rgb(51, 51, 51);">。</font>

<font style="color:rgb(119, 119, 119);">一般首个发送 SYN 报文的一方被认为是主动打开一个连接，而这一方通常也被称为</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">客户端</font><font style="color:rgb(119, 119, 119);">。而 SYN 的接收方通常被称为</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">服务端</font><font style="color:rgb(119, 119, 119);">，它用于接收这个 SYN，并发送下面的 SYN，因此这种打开方式是被动打开。</font>

<font style="color:rgb(51, 51, 51);">TCP 建立一个连接需要三个报文段，释放一个连接却需要四个报文段。</font>

### <font style="color:rgb(51, 51, 51);">TCP 断开连接 - 四次挥手</font>
<font style="color:rgb(51, 51, 51);">数据传输结束后，通信的双方可以释放连接。数据传输结束后的客户端主机和服务端主机都处于 ESTABLISHED 状态，然后进入释放连接的过程。</font>

![1714988763258-960a4cb9-ca06-4f35-a33f-6cbb847f1a56.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763258-960a4cb9-ca06-4f35-a33f-6cbb847f1a56-931317.png)

<font style="color:rgb(51, 51, 51);">TCP 断开连接需要历经的过程如下</font>

1. <font style="color:rgb(51, 51, 51);">客户端应用程序发出释放连接的报文段，并停止发送数据，主动关闭 TCP 连接。客户端主机发送释放连接的报文段，报文段中首部 FIN 位置为 1 ，不包含数据，序列号位 seq = u，此时客户端主机进入</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">FIN-WAIT-1(终止等待 1)</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">阶段。</font>
2. <font style="color:rgb(51, 51, 51);">服务器主机接受到客户端发出的报文段后，即发出确认应答报文，确认应答报文中 ACK = 1，生成自己的序号位 seq = v，ack = u + 1，然后服务器主机就进入</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">CLOSE-WAIT(关闭等待)</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态。</font>
3. <font style="color:rgb(51, 51, 51);">客户端主机收到服务端主机的确认应答后，即进入</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">FIN-WAIT-2(终止等待2)</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的状态。等待客户端发出连接释放的报文段。</font>
4. <font style="color:rgb(51, 51, 51);">这时服务端主机会发出断开连接的报文段，报文段中 ACK = 1，序列号 seq = v，ack = u + 1，在发送完断开请求的报文后，服务端主机就进入了</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">LAST-ACK(最后确认)</font><font style="color:rgb(51, 51, 51);">的阶段。</font>
5. <font style="color:rgb(51, 51, 51);">客户端收到服务端的断开连接请求后，客户端需要作出响应，客户端发出断开连接的报文段，在报文段中，ACK = 1, 序列号 seq = u + 1，因为客户端从连接开始断开后就没有再发送数据，ack = v + 1，然后进入到</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">TIME-WAIT(时间等待)</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态，请注意，这个时候 TCP 连接还没有释放。必须经过时间等待的设置，也就是</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">2MSL</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">后，客户端才会进入</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">CLOSED</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态，时间 MSL 叫做</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">最长报文段寿命（Maximum Segment Lifetime）</font><font style="color:rgb(51, 51, 51);">。</font>
6. <font style="color:rgb(51, 51, 51);">服务端主要收到了客户端的断开连接确认后，就会进入 CLOSED 状态。因为服务端结束 TCP 连接时间要比客户端早，而整个连接断开过程需要发送四个报文段，因此释放连接的过程也被称为四次挥手。</font>

<font style="color:rgb(51, 51, 51);">TCP 连接的任意一方都可以发起关闭操作，只不过通常情况下发起关闭连接操作一般都是客户端。然而，一些服务器比如 Web 服务器在对请求作出相应后也会发起关闭连接的操作。TCP 协议规定通过发送一个 FIN 报文来发起关闭操作。</font>

<font style="color:rgb(51, 51, 51);">所以综上所述，建立一个 TCP 连接需要三个报文段，而关闭一个 TCP 连接需要四个报文段。TCP 协议还支持一种</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">半开启(half-open)</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态，虽然这种情况并不多见。</font>

### <font style="color:rgb(51, 51, 51);">TCP 半开启</font>
<font style="color:rgb(51, 51, 51);">TCP 连接处于半开启的这种状态是因为连接的一方关闭或者终止了这个 TCP 连接却没有通知另一方，也就是说两个人正在微信聊天，cxuan 你下线了你不告诉我，我还在跟你侃八卦呢。此时就认为这条连接处于</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">半开启</font><font style="color:rgb(51, 51, 51);">状态。这种情况发生在通信中的一方处于主机崩溃的情况下，你 xxx 的，我电脑死机了我咋告诉你？只要处于半连接状态的一方不传输数据的话，那么是无法检测出来对方主机已经下线的。</font>

<font style="color:rgb(51, 51, 51);">另外一种处于半开启状态的原因是通信的一方关闭了</font>**<font style="color:rgb(51, 51, 51);">主机电源</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">而不是正常关机。这种情况下会导致服务器上有很多半开启的 TCP 连接。</font>

### <font style="color:rgb(51, 51, 51);">TCP 半关闭</font>
<font style="color:rgb(51, 51, 51);">既然 TCP 支持半开启操作，那么我们可以设想 TCP 也支持半关闭操作。同样的，TCP 半关闭也并不常见。TCP 的半关闭操作是指</font>**<font style="color:rgb(51, 51, 51);">仅仅关闭数据流的一个传输方向</font>**<font style="color:rgb(51, 51, 51);">。两个半关闭操作合在一起就能够关闭整个连接。在一般情况下，通信双方会通过应用程序互相发送 FIN 报文段来结束连接，但是在 TCP 半关闭的情况下，应用程序会表明</font>_<font style="color:rgb(51, 51, 51);">自己的想法</font>_<font style="color:rgb(51, 51, 51);">："我已经完成了数据的发送发送，并发送了一个 FIN 报文段给对方，但是我依然希望接收来自对方的数据直到它发送一个 FIN 报文段给我"。 下面是一个 TCP 半关闭的示意图。</font>

![1714988763219-3d107a32-9170-46a6-bf10-63f70659cbfa.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763219-3d107a32-9170-46a6-bf10-63f70659cbfa-002100.png)

<font style="color:rgb(51, 51, 51);">解释一下这个过程：</font>

<font style="color:rgb(51, 51, 51);">首先客户端主机和服务器主机一直在进行数据传输，一段时间后，客户端发起了 FIN 报文，要求主动断开连接，服务器收到 FIN 后，回应 ACK ，由于此时发起半关闭的一方也就是客户端仍然希望服务器发送数据，所以服务器会继续发送数据，一段时间后服务器发送另外一条 FIN 报文，在客户端收到 FIN 报文回应 ACK 给服务器后，断开连接。</font>

<font style="color:rgb(119, 119, 119);">TCP 的半关闭操作中，连接的一个方向被关闭，而另一个方向仍在传输数据直到它被关闭为止。只不过很少有应用程序使用这一特性。</font>

### <font style="color:rgb(51, 51, 51);">同时打开和同时关闭</font>
<font style="color:rgb(51, 51, 51);">还有一种比较非常规的操作，这就是两个应用程序同时主动打开连接。虽然这种情况看起来不太可能，但是在特定的安排下却是有可能发生的。我们主要讲述这个过程。</font>

<font style="color:rgb(51, 51, 51);">通信双方在接收到来自对方的 SYN 之前会首先发送一个 SYN，这个场景还要求通信双方都知道对方的</font><font style="color:rgb(51, 51, 51);"> </font>**<font style="color:rgb(51, 51, 51);">IP 地址 + 端口号</font>**<font style="color:rgb(51, 51, 51);">。</font>

<font style="color:rgb(51, 51, 51);">比如恋爱中的一对男女，他俩都同时说出了我爱你这个神圣的誓言，然后他俩对彼此的响应进行么么哒，这就是同时打开。</font>

<font style="color:rgb(51, 51, 51);">下面是同时打开的例子</font>

![1714988763342-65b5cd47-76ec-4535-8108-9198c4388acc.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763342-65b5cd47-76ec-4535-8108-9198c4388acc-590779.png)

<font style="color:rgb(51, 51, 51);">如上图所示，通信双方都在收到对方报文前主动发送了 SYN 报文，都在收到彼此的报文后回复了一个 ACK 报文。</font>

<font style="color:rgb(51, 51, 51);">一个同时打开过程需要交换四个报文段，比普通的三次握手增加了一个，由于同时打开没有客户端和服务器一说，所以这里我用了通信双方来称呼。</font>

<font style="color:rgb(51, 51, 51);">像同时打开一样，同时关闭也是通信双方同时提出主动关闭请求，发送 FIN 报文，下图显示了一个同时关闭的过程。</font>

![1714988763653-a87ec57a-f059-4af7-a52f-5a8ca9402854.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763653-a87ec57a-f059-4af7-a52f-5a8ca9402854-947922.png)

<font style="color:rgb(51, 51, 51);">同时关闭过程中需要交换和正常关闭相同数量的报文段，只不过同时关闭不像四次挥手那样顺序进行，而是交叉进行的。</font>

### <font style="color:rgb(51, 51, 51);">聊一聊初始序列号</font>
<font style="color:rgb(51, 51, 51);">也许是我上面图示或者文字描述的不专业，初始序列号它是有专业术语表示的，初始序列号的英文名称是</font>**<font style="color:rgb(51, 51, 51);">Initial sequence numbers (ISN)</font>**<font style="color:rgb(51, 51, 51);">，所以我们上面表示的 seq = v，其实就表示的 ISN。</font>

<font style="color:rgb(51, 51, 51);">在发送 SYN 之前，通信双方会选择一个初始序列号。初始序列号是随机生成的，每一个 TCP 连接都会有一个不同的初始序列号。RFC 文档指出初始序列号是一个 32 位的计数器，每 4 us（微秒） + 1。因为每个 TCP 连接都是一个不同的实例，这么安排的目的就是为了防止出现序列号重叠的情况。</font>

<font style="color:rgb(51, 51, 51);">当一个 TCP 连接建立的过程中，只有正确的 TCP 四元组和正确的序列号才会被对方接收。这也反应了 TCP 报文段容易被</font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">伪造</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的脆弱性，因为只要我伪造了一个相同的四元组和初始序列号就能够伪造 TCP 连接，从而打断 TCP 的正常连接，所以抵御这种攻击的一种方式就是使用初始序列号，另外一种方法就是加密序列号。</font>

## <font style="color:rgb(51, 51, 51);">TCP 状态转换</font>
<font style="color:rgb(51, 51, 51);">我们上面聊到了三次握手和四次挥手，提到了一些关于 TCP 连接之间的状态转换，那么下面我就从头开始和你好好梳理一下这些状态之间的转换。</font>

<font style="color:rgb(51, 51, 51);">首先第一步，刚开始时服务器和客户端都处于 CLOSED 状态，这时需要判断是主动打开还是被动打开，如果是主动打开，那么客户端向服务器发送</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">SYN</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">报文，此时客户端处于</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">SYN-SEND</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态，SYN-SEND 表示发送连接请求后等待匹配的连接请求，服务器被动打开会处于</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">LISTEN</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态，用于监听 SYN 报文。如果客户端调用了 close 方法或者经过一段时间没有操作，就会重新变为 CLOSED 状态，这一步转换图如下</font>

![1714988763720-01b8f3d7-e230-4c3f-8f39-aee71297b28c.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763720-01b8f3d7-e230-4c3f-8f39-aee71297b28c-817654.png)

<font style="color:rgb(119, 119, 119);">这里有个疑问，为什么处于 LISTEN 状态下的客户端还会发送 SYN 变为 SYN_SENT 状态呢？</font>

<font style="color:rgb(51, 51, 51);">知乎看到了车小胖大佬的回答，这种情况可能出现在 FTP 中，LISTEN -> SYN_SENT 是因为这个连接可能是由于服务器端的应用有数据发送给客户端所触发的，客户端被动接受连接，连接建立后，开始传输文件。也就是说，处于 LISTEN 状态的服务器也是有可能发送 SYN 报文的，只不过这种情况非常少见。</font>

<font style="color:rgb(51, 51, 51);">处于 SYN_SEND 状态的服务器会接收 SYN 并发送 SYN 和 ACK 转换成为</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">SYN_RCVD</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态，同样的，处于 LISTEN 状态的客户端也会接收 SYN 并发送 SYN 和 ACK 转换为 SYN_RCVD 状态。如果处于 SYN_RCVD 状态的客户端收到</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">RST</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">就会变为 LISTEN 状态。</font>

![1714988763674-6b0c28a0-2e24-454b-886c-804710b18422.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763674-6b0c28a0-2e24-454b-886c-804710b18422-301709.png)

<font style="color:rgb(51, 51, 51);">这两张图一起看会比较好一些。</font>

<font style="color:rgb(119, 119, 119);">这里需要解释下什么是 RST</font>

<font style="color:rgb(51, 51, 51);">这里有一种情况是当主机收到 TCP 报文段后，其 IP 和端口号不匹配的情况。假设客户端主机发送一个请求，而服务器主机经过 IP 和端口号的判断后发现不是给这个服务器的，那么服务器就会发出一个</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">RST</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">特殊报文段给客户端。</font>

![1714988763724-14647bc1-56d6-4feb-9346-ab8484a7d9a8.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763724-14647bc1-56d6-4feb-9346-ab8484a7d9a8-328533.png)

<font style="color:rgb(51, 51, 51);">因此，当服务端发送一个 RST 特殊报文段给客户端的时候，它就会告诉客户端</font>_<font style="color:rgb(51, 51, 51);">没有匹配的套接字连接，请不要再继续发送了</font>_<font style="color:rgb(51, 51, 51);">。</font>

<font style="color:rgb(51, 51, 51);">RST：（Reset the connection）</font>**<font style="color:rgb(51, 51, 51);">用于复位因某种原因引起出现的错误连接，也用来拒绝非法数据和请求</font>**<font style="color:rgb(51, 51, 51);">。如果接收到 RST 位时候，通常发生了某些错误。</font>

<font style="color:rgb(51, 51, 51);">上面没有识别正确的 IP 端口是一种导致 RST 出现的情况，除此之外，RST 还可能由于请求超时、取消一个已存在的连接等出现。</font>

<font style="color:rgb(51, 51, 51);">位于 SYN_RCVD 的服务器会接收 ACK 报文，SYN_SEND 的客户端会接收 SYN 和 ACK 报文，并发送 ACK 报文，由此，客户端和服务器之间的连接就建立了。</font>

![1714988763734-2b691175-8397-4f77-a71d-7ee49d6ba166.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988763734-2b691175-8397-4f77-a71d-7ee49d6ba166-160614.png)

<font style="color:rgb(51, 51, 51);">这里还要注意一点，同时打开的状态我在上面没有刻意表示出来，实际上，在同时打开的情况下，它的状态变化是这样的。</font>

![1714988764184-6b0fa3f9-8e62-4090-a3ac-49dce66096e0.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988764184-6b0fa3f9-8e62-4090-a3ac-49dce66096e0-946091.png)

<font style="color:rgb(51, 51, 51);">为什么会是这样呢？因为你想，在同时打开的情况下，两端主机都发起 SYN 报文，而主动发起 SYN 的主机会处于 SYN-SEND 状态，发送完成后，会等待接收 SYN 和 ACK ， 在双方主机都发送了 SYN + ACK 后，双方都处于 SYN-RECEIVED(SYN-RCVD) 状态，然后等待 SYN + ACK 的报文到达后，双方就会处于 ESTABLISHED 状态，开始传输数据。</font>

<font style="color:rgb(51, 51, 51);">好了，到现在为止，我给你叙述了一下 TCP 连接建立过程中的状态转换，现在你可以泡一壶茶喝点水，等着数据传输了。</font>

<font style="color:rgb(51, 51, 51);">好了，现在水喝够了，这时候数据也传输完成了，数据传输完成后，这条 TCP 连接就可以断开了。</font>

<font style="color:rgb(51, 51, 51);">现在我们把时钟往前拨一下，调整到服务端处于 SYN_RCVD 状态的时刻，因为刚收到了 SYN 包并发送了 SYN + ACK 包，此时服务端很开心，但是这时，服务端应用进程关闭了，然后应用进程发了一个</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">FIN</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">包，就会让服务器从 SYN_RCVD -></font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">FIN_WAIT_1</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态。</font>

![1714988764129-80ebc9ed-f395-4494-bbab-ea68580bf0f7.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988764129-80ebc9ed-f395-4494-bbab-ea68580bf0f7-454905.png)

<font style="color:rgb(51, 51, 51);">然后把时钟调到现在，客户端和服务器现在已经传输完数据了 ，此时客户端发送了一条 FIN 报文希望断开连接，此时客户端也会变为</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">FIN_WAIT_1</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态，对于服务器来说，它接收到了 FIN 报文段并回复了 ACK 报文，就会从 ESTABLISHED -></font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">CLOSE_WAIT</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态。</font>

![1714988764156-8f1e3c5b-3073-440f-a896-00d932e99d54.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988764156-8f1e3c5b-3073-440f-a896-00d932e99d54-079326.png)

<font style="color:rgb(51, 51, 51);">位于 CLOSE_WAIT 状态的服务端会发送 FIN 报文，然后把自己置于 LAST_ACK 状态。处于 FIN_WAIT_1 的客户端接收 ACK 消息就会变为 FIN_WAIT_2 状态。</font>

<font style="color:rgb(119, 119, 119);">这里需要先解释一下 CLOSING 这个状态，FIN_WAIT_1 -> CLOSING 的转换比较特殊</font>

<font style="color:rgb(51, 51, 51);">CLOSING 这种状态比较特殊，实际情况中应该是很少见，属于一种比较罕见的例外状态。正常情况下，当你发送FIN 报文后，按理来说是应该先收到（或同时收到）对方的 ACK 报文，再收到对方的 FIN 报文。但是 CLOSING 状态表示你发送 FIN 报文后，并没有收到对方的 ACK 报文，反而却也收到了对方的 FIN 报文。</font>

<font style="color:rgb(51, 51, 51);">什么情况下会出现此种情况呢？其实细想一下，也不难得出结论：那就是如果双方在同时关闭一个链接的话，那么就出现了同时发送 FIN 报文的情况，也即会出现 CLOSING 状态，表示双方都正在关闭连接。</font>

<font style="color:rgb(51, 51, 51);">FIN_WAIT_2 状态的客户端接收服务端主机发送的 FIN + ACK 消息，并发送 ACK 响应后，会变为</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">TIME_WAIT</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">状态。处于 CLOSE_WAIT 的客户端发送 FIN 会处于 LAST_ACK 状态。</font>

<font style="color:rgb(119, 119, 119);">这里不少图和博客虽然在图上画的是 FIN + ACK 报文后才会处于 LAST_ACK 状态，但是描述的时候，一般通常只对于 FIN 进行描述。也就是说 CLOSE_WAIT 发送 FIN 才会处于 LAST_ACK 状态。</font>

![1714988764248-3ff6714a-9a52-4bc3-959b-8179f3e9653e.png](./img/44549646_yy4WdMb_Jb7Da1tl/1714988764248-3ff6714a-9a52-4bc3-959b-8179f3e9653e-979929.png)

<font style="color:rgb(51, 51, 51);">所以这里 FIN_WAIT_1 -> TIME_WAIT 的状态也就是接收 FIN 和 ACK 并发送 ACK 之后，客户端处于的状态。</font>

<font style="color:rgb(51, 51, 51);">然后位于 CLOSINIG 状态的客户端这时候还有 ACK 接收的话，会继续处于 TIME_WAIT 状态，可以看到，TIME_WAIT 状态相当于是客户端在关闭前的最后一个状态，它是一种主动关闭的状态；而 LAST_ACK 是服务端在关闭前的最后一个状态，它是一种被动打开的状态。</font>

<font style="color:rgb(51, 51, 51);">上面有几个状态比较特殊，这里我们向西解释下。</font>

### <font style="color:rgb(51, 51, 51);">TIME_WAIT 状态</font>
<font style="color:rgb(51, 51, 51);">通信双方建立 TCP 连接后，主动关闭连接的一方就会进入 TIME_WAIT 状态。TIME_WAIT 状态也称为</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">2MSL</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的等待状态。在这个状态下，TCP 将会等待</font>**<font style="color:rgb(51, 51, 51);">最大段生存期(Maximum Segment Lifetime, MSL)</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">时间的两倍。</font>

<font style="color:rgb(119, 119, 119);">这里需要解释下 MSL</font>

<font style="color:rgb(51, 51, 51);">MSL 是 TCP 段期望的最大生存时间，也就是在网络中存在的最长时间。这个时间是有限制的，因为我们知道 TCP 是依靠 IP 数据段来进行传输的，IP 数据报中有 TTL 和跳数的字段，这两个字段决定了 IP 的生存时间，一般情况下，TCP 的最大生存时间是 2 分钟，不过这个数值是可以修改的，根据不同操作系统可以修改此值。</font>

<font style="color:rgb(51, 51, 51);">基于此，我们来探讨 TIME_WAIT 的状态。</font>

<font style="color:rgb(51, 51, 51);">当 TCP 执行一个主动关闭并发送最终的 ACK 时，TIME_WAIT 应该以 2 * 最大生存时间存在，这样就能够让 TCP 重新发送最终的 ACK 以避免出现丢失的情况。重新发送最终的 ACK 并不是因为 TCP 重传了 ACK，而是因为通信另一方重传了 FIN，客户端经常回发送 FIN，因为它需要 ACK 的响应才能够关闭连接，如果生存时间超过了 2MSL 的话，客户端就会发送 RST，使服务端出错。</font>

 



> 更新: 2024-05-06 17:46:27  
> 原文: <https://www.yuque.com/linuxer/gscfv1/ln7scvou7g0gs01a>