
-   下层信道完全可靠：rdt1.0中假设下层的信道是一个完全可靠的信道（理想情况）
    
    -   没有bit的错误
    -   没有分组（packet）丢失
-   发送方和接收方的 有限[[状态机]]（FSM） （存在状态和操作）  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/fc0767e33aa445a8a62790746afdbb05.png)
    
    -   发送方发送数据给下层信道 ， 接收方接收下层信道传来的数据  
          发送方首先在“**等待上级调用**”的状态，`rdt_send(data)`上级调用rdt，从上级接收到data，`make_pkt(data)`将data装到packet里，再用`udt_send(packet)`将packet发送出去，完成后发送方再回到“等待上级调用”的状态。（发送方只有一个状态）

接收方首先在“**等待下级调用**”的状态，`rdt_rcv(packet)`下级调用rdt，从下级接收到packet，用`extract(packet, data)`将packet重新恢复成data，提取出来的data再通过`deliver_data(data)`传送给上级。完成后接收方再回到“等待下级调用”的状态。

-   下层信道不可能完全可靠 => 引入rdt2.0