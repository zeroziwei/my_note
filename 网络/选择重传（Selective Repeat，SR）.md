## 选择重传（Selective Repeat，SR）

Selective Repeat:  
发送端最多在流水线中 有N个未确认的分组  
接收方对每个到来的分 组单独确认individual ack （非累计确认）  
发送方为每个未确认的 分组保持一个定时器 当超时定时器到时，只是 重发到时的未确认分组  
v![](https://img-blog.csdnimg.cn/88720582cdae4f6b8d9422ba4f174ee0.png)

![SR action.png](https://img-blog.csdnimg.cn/img_convert/35c49c280e7b7fb337e6a6efb3ca6164.png)

##### SR困境

比如下面序列号有：#0, #1, #2, #3，window大小为3的情况。SR会无法分清a、b两种情况，导致在b中误判重发的第一轮的`pkt0`，被当作后一轮的`pkt0`填入。

![在这里插入图片描述](https://img-blog.csdnimg.cn/9117226f01234ec5b836ac2d17db92c9.png)

#### 对比 GBN & SR

![在这里插入图片描述](https://img-blog.csdnimg.cn/857493827b9c4fdd9d1d345980e1c712.png)