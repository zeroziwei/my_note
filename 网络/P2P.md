## P2P

-   纯P2P架构 :  没有（或极少）一直运行的 服务器 任意端系统都可以直接通信  利用peer的服务能力  Peer节点间歇上网，每次IP 地址都有可能变化
-   例子: 文件分发 (BitTorrent) 流媒体(KanKan)  VoIP (Skype)
-   非结构化P2P ： 集中化目录 ， 完全分布式，混合
-   结构化p2p ：DHT（分布式散列表）维护环or树的有序拓扑结构( 不断深入的过程)

![在这里插入图片描述](https://img-blog.csdnimg.cn/6e7748c69689457a90982a6b8db2ae2e.png)

-   分布式命名
-   哈希值对应唯一的ID

#### 文件分发

![在这里插入图片描述](https://img-blog.csdnimg.cn/fbcd2d4346f24000bab6b31079dc7a47.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/be28397262394dd9bea09ba51e26b609.png)

##### BitTorrent

![在这里插入图片描述](https://img-blog.csdnimg.cn/9f537a316cc04d81a3e6bcb5f504e592.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/75f16c78dedc4bf09e0c79f07c50ea57.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/4c05a008eb394b5cb4ef05d2b68e5402.png)

#### p2p文件共享

![在这里插入图片描述](https://img-blog.csdnimg.cn/b5f0a9c0fef64144add81e8da7ba5f76.png)

-   两大问题： 如何定位所需资源 如何处理对等方的 加入与离开
-   可能的方案 :集中 分散 半分散

##### 集中式目录

![在这里插入图片描述](https://img-blog.csdnimg.cn/f629fa35f6cc4290845360eaaa0fcd51.png)

##### 泛洪 (完全分布式)

![在这里插入图片描述](https://img-blog.csdnimg.cn/373559f134d947ff8ad647ae57adcad9.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/ed63b76ec8eb435c83bedce8eed0d1ee.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/a960feee453740e1aca182e621dac7da.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/b8d8a093bf894e268db085f705e1970e.png)

#### 利用不匀称性：KaZaA

![在这里插入图片描述](https://img-blog.csdnimg.cn/5571380d19764558a61649c947437fcd.png)