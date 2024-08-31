
-   点对点（point-to-point）：一个发送方、一个接收方
-   可靠的、有序的字节流
    -   没有消息的边界
-   流水线机制
    -   窗口大小由拥塞和流量控制
-   全双工
    -   同时双向数据流
    -   MSS（maximum segment size）：最大报文段长度
-   面向连接
    -   握手：交换控制信息，初始化发送方和接收方的状态
-   流量控制

## TCP报文段结构

![在这里插入图片描述|600](https://img-blog.csdnimg.cn/85d56e5e987e4ec2a7eac098ccb7403f.png)  
1、端口号：用来标识同一台计算机的不同的应用进程。

-   源端口：源端口和IP地址的作用是标识报文的返回地址。
-   目的端口：端口指明接收方计算机上的应用程序接口。  
    TCP报头中的源端口号和目的端口号同IP数据报中的源IP与目的IP唯一确定一条TCP连接。

2、序号和确认号：是TCP可靠传输的关键部分。序号是本报文段发送的数据组的第一个字节的序号。在TCP传送的流中，每一个字节一个序号。e.g.一个报文段的序号为300，此报文段数据部分共有100字节，则下一个报文段的序号为400。所以序号确保了TCP传输的有序性。确认号，即ACK，指明下一个期待收到的字节序号，表明该序号之前的所有数据已经正确无误的收到。确认号只有当ACK标志为1时才有效。比如建立连接时，SYN报文的ACK标志位为0。

3、数据偏移／首部长度：4bits。由于首部可能含有可选项内容，因此TCP报头的长度是不确定的，报头不包含任何任选字段则长度为20字节，4位首部长度字段所能表示的最大值为1111，转化为10进制为15，15\*32/8 = 60，故报头最大长度为60字节。首部长度也叫数据偏移，是因为首部长度实际上指示了数据区在报文段中的起始偏移值。

4、保留：为将来定义新的用途保留，现在一般置0。

5、控制位：URG ACK PSH RST SYN FIN，共6个，每一个标志位表示一个控制功能。

-   URG：紧急指针标志，为1时表示紧急指针有效，为0则忽略紧急指针。
-   ACK：确认序号标志，为1时表示确认号有效，为0表示报文中不含确认信息，忽略确认号字段。
-   PSH：push标志，为1表示是带有push标志的数据，指示接收方在接收到该报文段以后，应尽快将这个报文段交给应用程序，而不是在缓冲区排队。
-   RST：重置连接标志，用于重置由于主机崩溃或其他原因而出现错误的连接。或者用于拒绝非法的报文段和拒绝连接请求。
-   SYN：同步序号，用于建立连接过程，在连接请求中，SYN=1和ACK=0表示该数据段没有使用捎带的确认域，而连接应答捎带一个确认，即SYN=1和ACK=1。
-   FIN：finish标志，用于释放连接，为1时表示发送方已经没有数据发送了，即关闭本方数据流。

6、窗口：滑动窗口大小，用来告知发送端接受端的缓存大小，以此控制发送端发送数据的速率，从而达到流量控制。窗口大小时一个16bit字段，因而窗口大小最大为65535。

7、校验和：奇偶校验，此校验和是对整个的 TCP 报文段，包括 TCP 头部和 TCP 数据，以 16 位字进行计算所得。由发送端计算和存储，并由接收端进行验证。

8、紧急指针：只有当 URG 标志置 1 时紧急指针才有效。紧急指针是一个正的偏移量，和顺序号字段中的值相加表示紧急数据最后一个字节的序号。 TCP 的紧急方式是发送端向另一端发送紧急数据的一种方式。

9、选项和填充：最常见的可选字段是最长报文大小，又称为MSS（Maximum Segment Size），每个连接方通常都在通信的第一个报文段（为建立连接而设置SYN标志为1的那个段）中指明这个选项，它表示本端所能接受的最大报文段的长度。选项长度不一定是32位的整数倍，所以要加填充位，即在这个字段中加入额外的零，以保证TCP头是32的整数倍。

10、数据部分： TCP 报文段中的数据部分是可选的。在一个连接建立和一个连接终止时，双方交换的报文段仅有 TCP 首部。如果一方没有数据要发送，也使用没有任何数据的首部来确认收到的数据。在处理超时的许多情况中，也会发送不带任何数据的报文段。

##### TCP序号

报文段（segment）的序号：字节流第一个字节的序号

例题：下面文件的前3个报文段的序号分别是？

![TCP segment sequenceNum.png|575](https://img-blog.csdnimg.cn/img_convert/0a509f919f7f85115b806bda06185661.png)  
第一个报文段：0；第二个报文段：1000；第三个报文段：2000

##### TCP ACK

-   TCP报文的ACK填写：期望从另一方收到的下一个字节序号 （确认号）
    -   主机A接收到从主机B传来的字节0～535，A下一个期望接到的字节为536，所以主机A发送的报文段的ACK中填536。
-   累计ACK（cumulative ACK）:与GBN相似
    -   主机A接收到从主机B传来的字节 0～535 和 900～1000，A下一个期望接到的字节依旧为536，所以主机A下一个发送的报文段的ACK中填536。

##### TCP 序号和ACK传输

![TCP segment eg.png|500](https://img-blog.csdnimg.cn/img_convert/2c32a6b904cac6fe53cb6fd99487d5f5.png)

##### TCP计时

Q： 怎样设置TCP 超时？  
 比RTT要长 ：但RTT是变化的  
 太短：太早超时 ： 不必要的重传  
 太长：对报文段丢失 反应太慢，消极

如何EstimateRTT（估计RTT）？  
 SampleRTT：测量从报文段发出到 收到确认的时间 : 如果有重传，忽略此次测量  
 SampleRTT会变化，因此估计的 RTT应该比较平滑 : 对几个最近的测量值求平均，而不是仅用当前的SampleRTT

设置的时间间隔 （数学 看不懂 就是套公式） ： 推荐值： & = 0.25 概率问题  
![在这里插入图片描述](https://img-blog.csdnimg.cn/9bd1e056d0b843c2b7369790e64eaa57.png)

### TCP可靠数据传输

-   TCP在IP不可靠服务的基础上 建立了rdt
-   管道化的报文段 • GBN or SR
-   累积确认（像GBN）
-   单个重传定时器（像GBN）
-   是否可以接受乱序的，没有规范  
     通过以下事件触发重传  
    超时（只重发那个最早的未确认段：SR）  
    重复的确认  
    例子：收到了ACK50,之后又收到3 个ACK50
-   首先考虑简化的`TCP`发送方： 忽略重复的确认 ， 忽略流量控制和拥塞控 制

##### TCP简化

TCP简化版：无重复ACK、拥塞控制和流量控制。

###### TCP sender

TCP sender 的3种事件

-   从上一层收到数据
    -   分段，创建seq#（报文中字节流第一个字节的序号）
    -   开始timer
        -   只给最早一个未ACK的segment timer
        -   用 TimeoutInterval 作为timeout时间
-   超时
    -   重传segment
    -   重启timer
-   收到ACK
    -   如果收到未ACK的segment的ACK
        -   更新被ACK的segment标记
        -   从最近的一个未ACK的segment开新的timer

`TCP sender简化版 :`  
![TCP sender.png|500](https://img-blog.csdnimg.cn/img_convert/318c6a85020d0dd6c6c3bf781100e4c3.png)

###### TCP重传情况

图 ACK丢包  
![在这里插入图片描述|500](https://img-blog.csdnimg.cn/83c02e7a5c98468785c696a125f12d90.png)

##### TCP快速重传

![在这里插入图片描述](https://img-blog.csdnimg.cn/8e14587761cc4f7ba8b108a77e34dca4.png)

-   timeout时间长，造成长时延
-   通过重复的ACK检测丢包
    -   发送方会发送很多个segment
    -   如果有segment丢失，则可能会收到很多个重复的ACK

TCP快速重传机制：

-   如果发送方收到对同一数据收到3个重复的ACK（实际收到4次该ACK），则认为此时未ACK的segment丢失，不需再等待timeout，重发未ACK的最小seq#

### 流量控制 rwnd

接收方要控制发送方，使发送方不会发送得太快导致接收方的缓存（`buffer`）溢出。

如果sender接收到rwnd=0，则说明此时没有剩余buffer，再发送数据会造成receiver buffer溢出，但是有要防止锁住。  
  所以sender向receiver发送一个`1 byte data`的报文，以更新`rwnd`。

### 连接管理

#### TCP建立连接：3次握手

建立连接之前，先握手

-   双方同意建立连接
-   同意连接的参数

**为什么两次握手行不通？**

-   各种delay
-   消息丢失导致重传
-   消息乱序

##### TCP关闭连接：4次挥手

-   client、server两边都可关闭连接
    -   发送TCP segment的 FIN bit = 1
-   用ACK回应FIN（ACK可以和FIN一起发）
-   同时收到FIN也可以处理（双工）  
    图 TCP关闭连接  
    ![TCP closing connection.png](https://img-blog.csdnimg.cn/img_convert/a3593494be6d1f5634509c4bbcbd1dd0.png)

### 拥塞控制（cwnd）原理

![在这里插入图片描述|500](https://img-blog.csdnimg.cn/a4152492ba624c679416b847ae553529.png)

-   MTU - >  
    ![在这里插入图片描述|500](https://img-blog.csdnimg.cn/9a7aed3f04ca418d8037461fe36a5094.png)
    
    -   初次的序号不可能为 `0`![ |500](https://img-blog.csdnimg.cn/7b82b21b7b754d05b0cc16655f9e5e36.png)
-   虽然网卡的传输时间可以设置为固定的， 但是应用进程建立TCP的时间是不固定的，动态的 ，其分布非常大，所以需要**定期测量 ！**
    

### TCP拥塞控制

#### 概述

拥塞:

-    非正式的定义: “太多的数据需要网络传输，超过了网络的处理能力”
-    与流量控制不同
-    拥塞的表现:
    -   ```
        分组丢失 (路由器缓冲区溢出) 
        ```
        
    -   分组经历比较长的延迟(在路由器的队列中排队)  
         网络中前10位的问题!  
        ![在这里插入图片描述](https://img-blog.csdnimg.cn/f856dad6a71c4ad2ad9df34ece928c2d.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/6bb0e3f42f5545bda1fac2db2be492bf.png)

##### 拥塞控制策略:

慢启动 AIMD：线性增、乘性减少 超时事件后的保守策略

###### TCP 拥塞控制：AIMD

sender逐渐增加发送速率（window size），从而探查可用bandwidth，直到丢包

-   加性增（additive increase）：cwnd每次每个RTT增加 1 MSS 直到检测到丢包（MSS 最大报文段长度）
-   乘性减（multiplicative decrease）：如果发生丢包，cwnd减半

![TCP AIMD.png](https://img-blog.csdnimg.cn/img_convert/247987a964a1d75a761dbd2ff7e580d4.png)

-   sender传输限制
    

-   **cwnd**是动态的随着网络拥塞程度变化的函数
    
-   结合之前的rwnd，实际的窗口大小为 minrwnd,cwnd  
    ![TCP cwnd.png](https://img-blog.csdnimg.cn/img_convert/ce9c17b7e67e2f9956a2ff461082b3c3.png)
    

**TCP发送速率（TCP sending rate）**

发送cwnd bytes，等待1个RTT接收ACK，然后再发送后续的bytes。

rate≈cwndRTT bytes/sec����≈������� �����/���

#### TCP慢启动（TCP slow start）

-   连接开始时，先指数级增长发送速率，直到出现
    
    丢包
    
    -   初始，cwnd=1 MSS����=1 ���
    -   每经过一个RTT，翻倍cwnd（实际上，每收到一个ACK，cwnd+1）

![TCP slow start.png](https://img-blog.csdnimg.cn/img_convert/3a37224e7130fd6c86fdfe5ca7ce8c9b.png)

当出现**丢包**时，

-   timeout情况
    -   cwnd重新设为 1MSS
    -   重新开始慢启动，直到到达一个threshold
-   3个重复的ACK情况（TCP RENO版本）
    -   重复的ACK既然能收到，那么网络还是有一定的传输能力，不需要像timeout一样重开。
    -   cwnd减半（乘性减）
-   TCP Tahoe版本中，timeout和3个重复的ACK都将cwnd设为 1MSS

#### 从 slow start 到 CA （快速恢复）的转换

当cwnd达到上次`timeout`时的1/2（即sstresh）时，从指数级增长变成线形增长。

**sstresh** —— 出现丢包时，将sstresh设置为此时`cwnd`的`1/2`

图 TCP Tahoe/Reno下cwnd的变化图【注：中间有一次3次ACK的丢包】  
![TCP congestion window.png](https://img-blog.csdnimg.cn/img_convert/34cb4f778b9a4af95d3f51d75bd1b7e8.png)

#### TCP吞吐量

![在这里插入图片描述](https://img-blog.csdnimg.cn/a06d02a4ecfe4610947f795b5a358b9c.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/55eacd629b864fbf9c4b2f948c74733f.png)

#### TCP公平性

-   目标：K条TCP连接，经过R bps的瓶颈，每条TCP连接分 R/Kbps，则公平。
    
-   以两条TCP连接为例，从A出发，经过加性增、乘性减，会逐渐趋向公平线。所以TCP可以实现公平性。  
    ![TCP Fairness.png](https://img-blog.csdnimg.cn/img_convert/01b1e3f96a39197315093ae841d9c0cc.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/9d63efa25123448fb57a8e4f202fdda7.png)
    
-   不同应用分别使用的协议:  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/69dd8656ad5a44fea7c6606dfbe6ab1d.png)