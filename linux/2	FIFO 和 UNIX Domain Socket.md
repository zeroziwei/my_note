## 2	FIFO 和 UNIX Domain Socket

> [!hint]
> 如果要互相通信的几个进程没有从公共祖先那里继承文件描述符，它们怎么通信呢？(任意进程如何通信)
> 
> 可以转换为下面的问题
> 
> 内核提供一条通道不成问题，问题是如何标识这条通道才能使各进程都可以访问它？(任意进程如何连接上内核给的通道)



文件系统中的路径名是全局的，各进程都可以访问，因此可以用文件系统中的路径名来标识一个 IPC 通道。

**FIFO** 和 **UNIX Domain Socket** 这两种 I**PC 机制**都是利用文件系统中的**特殊文件**来标识的。可以用`mkfifo`命令创建一个 FIFO 文件：

```
$ mkfifo hello
$ ls -l hello
prw-r--r-- 1 akaedu akaedu 0 2008-10-30 10:44 hello

```

FIFO 文件在**磁盘**上没有**数据块**，仅用来标识**内核**中的一条通道，==各进程可以打开这个文件进行`read`/`write`，实际上是在读写内核通道==（根本原因在于这个`file`结构体所指向的`read`、`write`函数和常规文件不一样），这样就实现了**进程间通信**。UNIX Domain Socket 和 FIFO 的原理类似，也需要一个特殊的 socket 文件来标识内核中的通道，例如`/var/run`目录下有很多系统服务的 socket 文件：

```
$ ls -l /var/run/
total 52
srw-rw-rw- 1 root        root           0 2008-10-30 00:24 acpid.socket
...
srw-rw-rw- 1 root        root           0 2008-10-30 00:25 gdm_socket
...
srw-rw-rw- 1 root        root           0 2008-10-30 00:24 sdp
...
srwxr-xr-x 1 root        root           0 2008-10-30 00:42 synaptic.socket

```

文件类型 s 表示 socket，这些文件在磁盘上也没有数据块。UNIX Domain Socket 是目前最广泛使用的 IPC 机制，到后面讲 **socket 编程**时再详细介绍。

现在把进程之间传递信息的各种途径（包括各种 IPC 机制）总结如下：

*   父进程通过`fork`可以将打开文件的描述符传递给子进程
    
*   子进程结束时，父进程调用`wait`可以得到子进程的终止信息
    
*   几个进程可以在文件系统中读写某个共享文件，也可以通过给文件加锁来实现进程间同步
    
*   进程之间互发信号，一般使用`SIGUSR1`和`SIGUSR2`实现用户自定义功能
    
*   管道
    
*   FIFO
    
*   mmap 函数，几个进程可以映射同一内存区
    
*   SYS V IPC，以前的 SYS V UNIX 系统实现的 IPC 机制，包括消息队列、信号量和共享内存，现在已经基本废弃
    
*   UNIX Domain Socket，目前最广泛使用的 IPC 机制