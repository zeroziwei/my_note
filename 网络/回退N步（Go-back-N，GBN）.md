[Go-Back Protocol](https://media.pearsoncmg.com/ph/esm/ecs_kurose_compnetwork_8/cw/content/interactiveanimations/go-back-n-protocol/index.html)
Go-back-N: 发送端最多在流水线 中有N个未确认的分组  
1. 接收端只是发送累计型确认cumulative ack 
2. 接收端如果发现gap， 不确认新到来的分组  
3. 发送端拥有对最老的 未确认分组的定时器 
4. 只需设置一个定时器 
5. 当定时器到时时，重传所有未确认分组

##### GBN发送方

![在这里插入图片描述](https://img-blog.csdnimg.cn/6106f3601e564482b3899b62c3e32c2f.png)

-   packet头部有`k bit`的序号，则可以表示 2k2� 个序号。
    
-   “窗口”大小为`N`，这一段是允许的未`ACK`的`packet`
    
-   ACK(n)累计确认：ACK在发送的时候要带上序号#n，即#n及之前的packet都收到了。接收方发送ACK n，则证明#n及之前的packet都收到了。否则接收方还是发送之前的ACK（重复）。
    
-   计时器只给最早的未ACK的packet保留
    
-   如果timeout（n），重传#n以及比#n更大的未ACK的packet
    

\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-4RYdcgcM-1682166861126)(https://raw.githubusercontent.com/GY23333/imgs/master/Network\_GBN-sender(1)\].png)

##### GBN接收方

![在这里插入图片描述](https://img-blog.csdnimg.cn/89e4bf20ef3643e0ac981df9d76a102f.png)

-   发送的ACK是顺序接收到的packet里面最大的序列号#
    -   可能会产生重复的ACK
    -   只需要记住期望的序列号（expextedseqnum）
-   乱序到达的packet
    -   直接丢弃，不缓存（缓存会造成数据重复）
    -   重新发送顺序最大序列号#

##### GBN运行

![在这里插入图片描述](https://img-blog.csdnimg.cn/e5a3a04d094d44e5ae27abdd71e1863c.png)  
  重发#2 ~ #5 的 packet。