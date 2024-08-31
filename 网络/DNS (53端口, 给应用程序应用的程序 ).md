## DNS (53端口, 给应用程序应用的程序 )

![在这里插入图片描述](https://img-blog.csdnimg.cn/a5b15e1c21294fddb7d5246b90315008.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/16f22ee011eb4957a38e491b9f094af9.png)

-   负载均衡 ： DNS也用于冗余的服务器之间进行负载分配 ， 繁忙的站点被冗余分布在多台服务器上，每台服务器均运行在不同的端系统上，每个都有不同的IP地址。

#### DNS系统需要解决的问题

![在这里插入图片描述](https://img-blog.csdnimg.cn/d5d1a4815225417c91064ccf9767c7c2.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/0cd77815a5844f3b8f5abb0eb5329509.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/f4630b2fdeaa4f9caa8c74c249f76cdd.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/815484b62cec4224b97df0a26101ae37.png)

#### 问题2：解析问题-名字服务器(NameServer)

![在这里插入图片描述](https://img-blog.csdnimg.cn/aa92eae3de3b47889e52e7d34343f703.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/cff2dec1c0574bbda31a3f7c4459b571.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/acde354c6f144654a2ae979cb8ba9a88.png)

-   Value : IP
    
-   T ： type  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/5692b991d660448eb2203c6519b115b7.png)
    
-   **区域名字服务器维护资源记录**
    

-   资源记录(resource records)
    
    -   作用：维护 域名-IP地址(其它)的映射关系
    -   位置：Name Server的分布式数据库中
-   RR格式: (domain\_name, ttl, type,class,Value)
    
    -   `Domain_name`: 域名
    -   `Ttl: time to live` : 生存时间(权威，缓冲记录)
    -   `Class` 类别 ：对于Internet，值为IN
    -   `Value` 值：可以是数字，域名或ASCII串
    -   `Type` 类别：资源记录的类型—见下页

![在这里插入图片描述](https://img-blog.csdnimg.cn/5dfc1db599dc42dd8b113e591e91cb79.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/9f475d1b748841cbb3491f700c862c83.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/5d575e6dfacc400588c2a89d1de319d0.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/0d9eb8b9aa7f46e8bb29f7eb5f066a84.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/32ac5ab1bc08495cac8d5a20200d2fe8.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/390d9c825a04469388b682d0b333e175.png)

#### DNS协议、报文

![在这里插入图片描述](https://img-blog.csdnimg.cn/3c097fac77f24afba47184fe51c48f02.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2278fb9f83104c43b4c81e75c72a1b80.png)

#### 提高性能 – > 缓存

一旦名字服务器学到了一个映射，就将该映射 缓存起来  
 根服务器通常都在本地服务器中缓存着 使得根服务器不用经常被访问  
目的：提高效率 可能存在的问题：如果情况变化，缓存结果和 权威资源记录不一致  
解决方案：TTL（默认2天）

#### 问题3：维护问题：新增一个域

![在这里插入图片描述](https://img-blog.csdnimg.cn/ceb96b2d0dd648fab2abe54c7c5ddf54.png)

#### 攻击DNS

![在这里插入图片描述](https://img-blog.csdnimg.cn/c7e0e759c11d4c978b42fcf0550bc183.png)