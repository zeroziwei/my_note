## HTTP

![在这里插入图片描述](https://img-blog.csdnimg.cn/86eef70b85c4470da75ce8907eb7a1e8.png)



### HTTP连接

![在这里插入图片描述](https://img-blog.csdnimg.cn/7eb3464130ad4051bce9fd04fe77d109.png)

### HTTP请求报文

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cf0ad4352134c679936a4f76f53ab66.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/f18f82711bfd4ed7ba29e7a8f5939669.png)

### 用户-服务器状态 ： cookies

![在这里插入图片描述](https://img-blog.csdnimg.cn/da6a29404caa49b693432d990bf549cf.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/06ea0f4af6b64bae8342779d3aa95ee9.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/f8fdb7b6593b4521825e0b6ec5ca2a41.png)

### Web缓存 (代理(proxy server)服务器)

![在这里插入图片描述](https://img-blog.csdnimg.cn/8b80b7fdf9c94a9a9c19417f4aaa2686.png)

-   缓存既是客户端又是 服务器
-   通常缓存是由ISP安 装 (大学、公司、居 民区`ISP`)  
    **为什么要使用Web缓存 ？**  
    降低客户端的请求响应时 间  
    可以大大减少一个机构内 部网络与**Internent**接入 链路上的流量  
    互联网大量采用了缓存： 可以使较弱的**ICP**也能够 有效提供内容  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/a3af50d43a7140febec258f96eb10a96.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/bbce430c403f4161a9e3c4d86e95fee0.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/cc0009b2da744b5794406a963fe38e26.png)

**改进**

缓存例子：安装本地缓存  
![在这里插入图片描述](https://img-blog.csdnimg.cn/7da6ed917ebf46409c5955ff95f76500.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/416cf03b5fab409eb76aaa403b30075e.png)

-   总体延迟：  = 0.6 \* (从原始服务器获取对象的 延迟) +0.4 \* (从缓存获取对象的延 迟)  
     = 0.6 (2.01) + 0.4 (~msecs)  = ~ 1.2 secs  比安装154Mbps链路还来得小 (而且 比较便宜!)

#### 条件GET方法

![在这里插入图片描述](https://img-blog.csdnimg.cn/f4923bbd588848319769b8639445dd71.png)