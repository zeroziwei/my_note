## Socket编程

##### socket俩个重要的数据结构

```c
// IP地址和port捆绑关系的数据结构（标示进程的端节点） struct sockaddr_in { short sin_family; //地址族 u_short sin_port;// port端口 struct in_addr ,sin_addr; //IP 地址 char sin_zero[8]; // align 对齐 }; //数据结构 hostent 域名和IP地址的数据结构 struct hostent { char *h_name;//主机域名 char **h_aliases; //数组存储 主机别名 int h_addrtype; int h_length; /*地址长度*/ char **h_addr_list; // 数组存储 IP地址 #define h_addr h_addr_list[0]; };
```

#### UDPsocket 编程

> UDP提供的是一种不可靠的数据流传输，传输过程中可能会丢包，接收的时候顺序也可能被打乱。

UDP：客户机与服务器之间没有连接

-   发送数据前不需要握手
-   发送数据包附加IP地址+端口号
-   接收方从数据包中提取处IP地址+端口号  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/dfebf2c84dbd4704a478b8942dc3dba4.png)UDP中的socket编程示例
-   下面俩个程序的作用是 在`用户端`发送字符串给`服务端`, 然后`服务端`把字符串修改为大写在发送给`客户端`
-   这里`jupyter notebook`中进行编程，安装好`jupyter notebook`后，在命令行执行 （我用的vscode）`UDPServer.ipynb`

```cpp
from socket import * #引入socket库 serverPort = 12000 #服务器端口号 serverSocket = socket(AF_INET, SOCK_DGRAM) #创建服务器套接字 serverSocket.bind(('', serverPort)) #给套接字绑定端口号 print('The server is ready to receive.') while True: #服务器要一直在线等待，所以给一个死循环 message, clientAddress = serverSocket.recvfrom(2048) #从服务器套接字中读取信息（发送的消息和客户机IP地址+端口号） modifiedMessage = message.decode().upper() #对消息进行处理（此处是改大写） serverSocket.sendto(modifiedMessage.encode(), clientAddress) #将处理后的消息发回给客户机
```

`UDPClient.ipynb`

```cpp
from socket import * #引入socket库 serverName = gethostname() #由于没得服务器，服务器主机用本机来当 serverPort = 12000 #服务器端口号 clientSocket = socket(AF_INET, SOCK_DGRAM) #创建客户机套接字 message = input('Input lowercase sentence:') #得到输入字符串 clientSocket.sendto(message.encode(),(serverName, serverPort)) #发送数据到相应主机名+端口号的服务器进程 modifiedMessage, serverAddress = clientSocket.recvfrom(2048) #接收服务器发回的消息 print(modifiedMessage.decode()) #显示接收的字符串 clientSocket.close() #关闭客户机socket
```

##### 运行过程

-   必须先运行`服务器端`

![在这里插入图片描述](https://img-blog.csdnimg.cn/e396ffb7f5f54dc8b3bb42b7d1e07a21.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/8543310ee44540f8918f673efd8da32d.png)

#### TCPsocket 编程

-   服务器的先行准备
    -   服务器必须先运行
    -   服务器需要创建socket来连接客户机
-   客户机连接服务器
    -   客户机需要创建自己的socket，明确服务器进程的IP地址和端口号
    -   客户机创建socket时，客户机和服务器之间需建立TCP连接
-   服务器接收客户机消息
    -   服务器需创建一个新的socket，为了服务器进程能够和客户机进行通信
        -   要运行服务器与多个客户机进行通信
        -   用源的端口号来区分不同的客户机

> TCP提供的是一种可靠的字节流（byte-stream）传输（pipe）。

![在这里插入图片描述](https://img-blog.csdnimg.cn/7f8113e37a384592a13579fb24b242dc.png)

##### TCP中的socket编程示例

-   TCP服务器代码  
    `TCPServer.ipynb`

```cpp
from socket import * #引入socket库 serverPort = 12000 #服务器端口号 serverSocket = socket(AF_INET, SOCK_STREAM) #创建服务器套接字（前台） serverSocket.bind(('', serverPort)) #给套接字绑定端口号 serverSocket.listen(1) print('The server is ready to receive.') while True: #服务器要一直在线等待，所以给一个死循环 connectionSocket, addr = serverSocket.accept() #前台套接字接收到请求后，创建一个新的套接字（窗口） sentence = connectionSocket.recv(1024).decode() #窗口套接字读取信息 capitalizedSentence = sentence.upper() #对消息进行处理（此处是改大写） connectionSocket.send(capitalizedSentence.encode()) #将处理后的信息发回给客户机 connectionSocket.close() #关闭窗口套接字，前台套接字保持开放
```

-   TCP客户机代码  
    `TCPClient.ipynb`

```cpp
from socket import * #引入socket库 serverName = gethostname() #由于没得服务器，服务器主机用本机来当 serverPort = 12000 #服务器端口号 clientSocket = socket(AF_INET, SOCK_STREAM) #创建客户机套接字(类型为字节流SOCK_STREAM) clientSocket.connect((serverName, serverPort)) #TCP连接 sentence = input('Input lowercase sentence:') #得到输入字符串 clientSocket.send(sentence.encode()) #发送数据到服务器 modifiedSentence = clientSocket.recv(1024) #接收服务器发回的消息 print('From server:', modifiedSentence.decode()) #显示接收的字符串 clientSocket.close() #关闭客户机socket
```

-   有BUG 待解决 ！！
    
-   待补充C++版本
    
-   [py版本](https://blog.csdn.net/weixin_45811693/article/details/115716351)