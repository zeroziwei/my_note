## 电子邮件（EMail）

![在这里插入图片描述](https://img-blog.csdnimg.cn/ad3ccdf7ae954cf89b63004af222d5e5.png)

-   邮件服务器
    -    邮箱中管理和维护发送给用户 的邮件
    -   输出报文队列保持待发送邮件 报文
    -    邮件服务器之间的SMTP协议 ：发送email报文
        -    客户：发送方邮件服务器
        -    服务器：接收端邮件服务 器
-   使用TCP在客户端和服务器之间传送报文，端口 号为25
-   直接传输：从发送方服务器到接收方服务器
-   传输的3个阶段 握手 传输报文 关闭
-   命令/响应交互 命令：ASCII文本 响应：状态码和状态信息
-   报文必须为7位ASCII码

#### eg

![在这里插入图片描述](https://img-blog.csdnimg.cn/2ea6e3e8740c4f40b3148e1ae8b6f650.png)

#### 简单的SMTP交互

-   telnet servername 25
-    enter HELO, MAIL FROM, RCPT TO, DATA, QUIT commands

#### 小结

-   HTPP ：拉协议 ，用户使用HTTP从服务器拉取这些信息
-   SMTP ：推协议,发送邮件服务器把文件推向接受邮件服务器。  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ff5c40996ca043aa9238c7dcebf8c2c9.png)

#### 邮件报文格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/010ba7c9ca03477080d6cbedd9e3a557.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/636b4037ef3b42e985ca8ff91b3944aa.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ee4cc782858641ac8009a85fd92a09ac.png)

#### POP3

![在这里插入图片描述](https://img-blog.csdnimg.cn/db30e17be06d49379c120ede9a2e599c.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/0c1dea48722942be88d2f1c29f81bc77.png)