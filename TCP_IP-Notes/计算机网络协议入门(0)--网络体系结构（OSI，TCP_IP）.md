## OSI七层协议体系结构
OSI（开放系统互连）七层网络模型，被称为开放式网络互连参考模型，上三层称为高层，用于定义应用程序之间的通信和人机界面；下四层称为底层，用于定义数据如何进行端到端的传输，物理范围以及数据与光电信号间的转换。
如图：
![](http://i.imgur.com/XBtpoga.png)
5） 会话层：主要负责在网络中的两个结点之间建立、维护、控制会话，区分不同的会话，以及提供单工（Simplex)、半双工（Half duplex)、全双工（Full duplex)3种通信模式的服务。

4） 传输层：OSI模型中最重要的一层，主要负责分割、组合数据，实现端到端的逻辑连接。数据在上三层是整体的，到了这一层开始被分割，分割后的数据被称为段。三次握手，面向连接或非面向连接的服务、流量控制等都发生在这一层。

3） 网络层：网络层将网络（IP）地址翻译为物理（MAC）地址，并决定将数据从发送方路由到接收方，主要负责管理网络地址、定位设备、决定路由，（路由器就工作在这一层）。上层的数据段在这一层被分割，封装后叫做包，包有两种：一种为用户数据包（Data packets），是上层传下来的用户数据；另一种为路由更新包(Route update packets)，是直接由路由器发出来的，用来和其他路由器进行路由信息的交换。

2） 数据链路层： 控制物理层与网络层之间的通信，主要负责物理传输和准备，包括物理地址寻址、CRC校验、错误通知、网络拓扑、流量控制、重发等。MAC地址和交换机都工作在这一层。上层传下来的包在这一层被分割封装后叫做帧。

1) 物理层：物理层是实实在在地物理链路，它规定了激活、维持、关闭通信端点之间的机械特性、电气特性、功能特性以及过程特性。它为上层协议提供了一个传输数据的物理媒介，负责将数据以比特流的方式发送、接收。常见的物理媒介有双绞线、同轴电缆等。属于物理层相关的规范有EIA/TIA RS-232、EIA/TIA RS-449、RJ-45等。

*********
## TCP/IP体系结构
TCP/IP是最基本的Internet协议，由网络层的IP和传输层的TCP构成。常说的TCP/IP指的是TCP/IP协议簇
。由下而上分别为网络接口层、网络层、运输层、应用层。
### TCP/IP四层和OSI七层模型对应图
![](http://i.imgur.com/DGlCf8M.png)

### TCP/IP协议簇中的常见协议
![](http://i.imgur.com/LqHrell.jpg)

TCP/IP协议实际上就是在物理网上的一组完整的网络协议。其中TCP是提供传输层服务，而IP则是提供网络层服务。下面是各个层的协议说明：
　　
IP： 网际协议(Internet Protocol) 负责主机间数据的路由和网络上数据的存储。同时为ICMP，TCP，UDP提供分组发送服务。用户进程通常不需要涉及这一层。

ARP： 地址解析协议(Address Resolution Protocol)
此协议将IP地址映射到MAC地址。

RARP： 反向地址解析协议(Reverse Address Resolution Protocol)
此协议将MAC地址映射到IP地址。

ICMP： 网际报文控制协议(Internet Control Message Protocol)
此协议处理信关和主机的差错和传送控制。(ping程序就是直接使用网络层的ICMP协议工作的)

TCP： 传送控制协议(Transmission Control Protocol)
这是一种提供给用户进程的可靠的全双工字节流面向连接的协议。它要为用户进程提供虚电路服务，并为数据可靠传输建立检查。（注：大多数网络用户程序使用TCP）

UDP： 用户数据报协议(User Datagram Protocol)
这是提供给用户进程的无连接协议，用于传送数据而不执行正确性检查。

FTP： 文件传输协议(File Transfer Protocol)
允许用户以文件操作的方式（文件的增、删、改、查、传送等）与另一主机相互通信。

SMTP： 简单邮件传送协议(Simple Mail Transfer Protocol)
SMTP协议为系统之间传送电子邮件。

TELNET：终端协议(Telnet Terminal Procotol)
允许用户以虚终端方式访问远程主机

HTTP： 超文本传输协议(Hypertext Transfer Procotol)

TFTP: 简单文件传输协议(Trivial File Transfer Protocol)