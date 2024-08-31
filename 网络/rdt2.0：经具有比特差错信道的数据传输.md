引入差错检测、控制信号和重传机制，解决下层信道不可靠问题。

## rdt2.0概述

-   packet在下层信道传输中会出现**比特翻转**：可以引入**校验和**（checksum)来检测比特错误。
-   错误恢复——如果检验到错误如何恢复？
    -   **ACKs（acknowledgements）**：接收方告诉发送方收到的pkt是正确的
    -   **NAKs（negative acknowledgements）**：接收方告诉发送方收到的pkt是错误的
    -   发送方收到NAK则**重传**那个pkt
-   rdt2.0引入的新机制
    -   差错检测：checksum
    -   接收方反馈：控制信号（control msg），即ACK和NAK
    -   重传

## rdt2.0 状态机

![在这里插入图片描述|600](https://img-blog.csdnimg.cn/20ccc9ce1c9640ebad93079e28120c97.png)

### 发送方

rdt2.0的发送方有2个状态——**等待上级调用**、**等待ACK或NAK**。发送方最初处于“**等待上级调用**”的状态，`rdt_send(data)`上级调用rdt，从上级接收到data，`sndpkt = make_pkt(data, checksum)`将data装到packet里，再用`udt_send(sndpkt)`将packet发送出去。此时，发送方变为“**等待ACK或NAK**”的状态。`rdt_rcv(rcvpkt)`接收反馈，如果`isNAK(rcvpkt)`即接收到NAK，则重传`udt_send(sndpkt)`，并保持“**等待ACK或NAK**”的状态；如果`isACK(rcvpkt)`即接收到ACK，则回到“**等待上级调用**”的状态。

-   停等机制（`stop and wait`）
-   发送方发送一个`packet`，然后等待接收方的响应。

> 一直要等想想就很慢 

### 接收方

接收方还是只有一个状态——**等待下级调用**。接收方首先在“**等待下级调用**”的状态，`rdt_rcv(rcvpkt)`接收方接收packet，如果`corrupt(rcvpkt)`，即检测到错误，则`udt_send(NAK)`反馈NAK；如果`notcorrupt(rcvpkt)`，即未检测到错误，则`extract(packet, data)`将packet重新恢复成data，`deliver_data(data)`将data传送给上级，最后`udt_send(ACK)`反馈ACK。完成后接收方再回到“等待下级调用”的状态。

### 问题

ACK/NAK分组受损怎么办
