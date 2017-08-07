算法数据结构 socket编程 epoll多路复用

# java基础方面

# 算法



# 数据结构



# socket编程

应用层：用户进程

——这个位置中间软件抽象层（socket抽象层）

传输层：协议+端口  TCP/UDP

网络层：ip地址

链路层：硬件接口

三元组（ip地址，协议，端口）可以标识网络进程了，网络中的进程通信就是利用这个标志和其他进程进行交互。这就是TCP/IP协议组。

使用tcp/ip协议的应用程序通常采用的应用编程接口：（过去淘汰的bsd的socket和unix system v的TLI）现在都是采用socket编程接口。

TCP/IP协议存在与OS中，网络服务通过OS提供。

在OS中增加支持TCP/IP的系统调用——Berkeley套接字，如Socket，Connect，Send，Recv等。

UDP（user data protocol）与TCP（transmission control protocol）相对应

socket是应用层和TCP/IP协议组（传输层加网络层）通信的中间软件抽象层，就是一组接口。

socket就是一个门面模式，把复杂的tcp/ip协议组隐藏在socket接口后面，我们在网络编程中大量用的是通过socket实现的。

socket套接字描述符其实就是个整数，系统为每个运行的进程维护一张单独的文件描述符表（一个进程一张表）。进程打开一个文件，系统返回一个指向此文件内部数据结构的指针写入这张表中，并且把索引值返回给调用者，这个索引值就是描述符。

socket函数

创建socket可以指定不同参数

protofamily：协议域/协议组（family）AF_INET,AF_INET6,AF_LOCAL,AF_ROUTE

type：socket类型：

protocol：指定协议：IPPROTO_TCP,IPPROTO_UDP,IPPROTO_STCP,IPPROTO_TIPC(tcp,udp,stcp,tipc)

bind函数不调用那么系统会随机分配一个端口

客户端不需要bind，服务端需要bind指定端口

### 网络字节序和主机字节序

主机字节序就是平常说的大端和小端，也就是**整数在内存中的保存顺序**

大端（java通信统一大端）

网络字节序：UDP/TCP/IP协议规定把接收到的第一个字节当作高位字节看待,这就要求发送端发送的第一个字节是高位字节，所以说,网络字节序是大端字节序。所以有时我们也会把big endian方式称之为网络字节序。

在将一个地址绑定到socket的时候，请先将主机字节序转换成为网络字节序

总结：

一台机器上如果小端序，先转化成大端序，网络传输都是大端序，（低地址放高位，由于起始从内存低位往高位传输数据），所有接受端机器接受的首先为字节的高位，如果机器本身为大端则无须转换，如果为小端那就要转换了。

socket()创建套接字

bind()

listen()

accept():区分

​	监听套接字：socket函数产生

​	连接套接字：accept函数产生，返回connect_fd

read()

write()

socket中tcp的建立（三次握手）

第一次：客户端发送SYN包（syn=j），进入**SYN_SEND**状态

第二次：服务器接受SYN包，自己也发送SYN（syn=k）+ACK(ack=j+1)包，进入**SYN_RECV**

第三次：客户端接受到SYN（syn=k）+ACK(ack=j+1)包，向服务器发送ACK（ack=k+1），客户端和服务端进入**ESTABLISHED**，完成三次握手

tcp连接的终止（四次握手释放）

四次握手的原因：客户端发送fin给服务端，只是说明客户端没数据传输过来了，但是服务端可能还在传输数据给客户端，所以，服务端先确认客户端发送的fin，然后再次发送fin给客户端，然后客户端发送确认双方关闭

php下的fsockopen，pfsockopen

多进程和多线程（出现晚一点）同步阻塞

一个进程处理所有外来连接创建子进程，开销很大

Leader-Follower模型

创建多个进程处理外来连接分别创建子进程，apache和php-fpm

但是缺点也很明显：

1. 严重依赖进程数量，一个客户端就要占用一个进程，并发能力有限。


2. 启动这么多进程，那么会带来额外上下文的进程调度消耗

下面就是在一个进程处理所有并发IO

# IO多路复用之epoll



