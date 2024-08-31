---
aliases:
  - UDP
  - udp
---
- “no frills,” “bare bones” Internet transport protocol (青春版( )
	- connectionless
		- no handshaking between UDP sender, receiver
		- each UDP segment handled independently of others

 - “best effort” service, UDP segments may be: (我尽力而为---领导语))
	 - lost
	 - delivered out-of-order to app

- Why is there a UDP?
	- no connection establishment (which can add RTT delay)
	- simple: no connection state at sender, receiver
	- small header size
	- no congestion control
		- UDP can blast away as fast as desired! (那岂不是会爆炸)
		- can function in the face of congestion (能用,但有没有用就不知道了)

- UDP use:
	- streaming multimedia apps (loss tolerant, rate sensitive)
	- DNS
	- SNMP
	- HTTP/3


- if reliable transfer needed over UDP (e.g., HTTP/3): 
	- add needed reliability at application layer
	- acc congestion control at application layer
(那不就成了tcp了)


- UDP: User Datagram Protocol [RFC 768]

![[Pasted image 20240703030743.png]]
ip的事该ip做!!!

![[Pasted image 20240703031422.png]]

- UDP: Transport Layer Actions
	1. is passed an application-layer message
	2. **determines UDP segment header fields values** (how!!!) #question 
	3. creates UDP segment
	4. passes segment to IP

- UDP receiver actions:
	1. receives segment from IP
	2. checks UDP checksum header value
	3. extracts application-layer message
	4. demultiplexes message up to application via socket








![在这里插入图片描述|500](https://img-blog.csdnimg.cn/f55ba183b5194bc88f7e175c722f8621.png)  


UDP除了实现了复用/分用功能和简单的错误校验外，几乎没有对 IP 增加别的东西；UDP 提供尽力而为交付服务服务，UDP段可能丢失、非按序到达；UDP是无连接的，发送方和接收方之间不需要握手，每个UDP段的处理独立于其他段。

TCP提供可靠数据传输和拥塞控制，为什么还需要UDP呢？UDP有以下好处：

-   应用可更好地控制发送时间和速率
-   无需建立连接 (减少延迟减少延迟)。这也可能是DNS使用UDP而不是TCP的主要原因，如果使用TCP的话，DNS服务将会慢很多。
-   无需维护连接状态：`TCP`为了实现可靠数据传输和拥塞控制需要在端系统中维护一些参数，这些参数包括：接收和发送的缓存、拥塞控制参数、确认号和序号；这些参数信息都是必须的；而`UDP`因为不建立连接，所以自然也就不需要维护这些状态，这就减少了时空开销；
-   分组首部更小：`TCP`有`20`字节的首部开销，而`UDP`只有`8`字节；

## udp报文结构

![在这里插入图片描述|500](https://img-blog.csdnimg.cn/ed21f2f5dde34747b3e4c1c03bb953cc.png)  
UDP首部只有4个字段，每个字段占用两个字节，分别是：**源端口号、目的端口号、长度和校验和**；长度字段指示了在 UDP 报文段中的字节数（首部加数据）。接收方使用检验和来检查在该报文段中是否出现了差错。

## UDP校验和

![在这里插入图片描述|500](https://img-blog.csdnimg.cn/d8e5e9f45a2148c0b28e8a44b196febf.png)

-   `出错就丢弃`  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/7f6b8c4c4a064369a3024c8b3977c1bb.png)