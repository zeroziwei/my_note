## CDN

#### 视频流化服务和CDN：上下文

-   流量大、用户多 、异构性  不同用户拥有不同的能力（例如：有线接入和移 动用户；带宽丰富和受限用户 . — > **解决方案: 分布式的，应用层面的基础设施**
    
-   ![在这里插入图片描述](https://img-blog.csdnimg.cn/bfc2764461924acf96eaeaad4894efc9.png)
    
-   由于存储下载视频 是一对一的关系，则下载未免有些慢了，于是就有了存储视频的流化服务.
    
-   多媒体流化服务：DASH  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/c7677e1eeecc4a749c2cd5af21ab525d.png)  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ddc59cd105534b8c8ccdb79789d0b52d.png)  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ae5d16fc0f034bcd8851551aa728539e.png)
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/8873e7b9920b40538b91b015df9be61e.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/248bfa6d230248aa96781656288b247b.png)

-   互联网络主机-主机之间的通信作为一种服务向用户提供
-   OTT 挑战: 在拥塞的互联网上复制内容 “over the top”  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/38fa32de2eff44dfa6e9398abf8cdae9.png)  
    1 ： 流量服务器  
    2、向本地DNS服务器解析  
    3、返回URL  
    4、 告诉他离他最近的客户端的ip地址  
    6、再由kingCDN 向客户提供流化服务