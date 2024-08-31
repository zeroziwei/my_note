---
tags: 
progress: 
creation date: 2024-07-24 12:26
modification date: 星期三 24日 七月 2024 12:26:45
---
#### 文章目录

-   [CPU的时间观](https://blog.csdn.net/qq_43380180/article/details/111573642#CPU_2)
-   [I/O（异步的瓶颈）](https://blog.csdn.net/qq_43380180/article/details/111573642#IO_13)
-   [基础概念](https://blog.csdn.net/qq_43380180/article/details/111573642#_29)
-   -   [进程/线程](https://blog.csdn.net/qq_43380180/article/details/111573642#_31)
    -   [阻塞/非阻塞](https://blog.csdn.net/qq_43380180/article/details/111573642#_37)
    -   [并发/并行](https://blog.csdn.net/qq_43380180/article/details/111573642#_41)
    -   -   [CPU调度策略](https://blog.csdn.net/qq_43380180/article/details/111573642#CPU_45)
    -   [同步/异步](https://blog.csdn.net/qq_43380180/article/details/111573642#_53)
    -   [事件循环+回调](https://blog.csdn.net/qq_43380180/article/details/111573642#_59)
-   [协程（异步）](https://blog.csdn.net/qq_43380180/article/details/111573642#_68)
-   [async/await](https://blog.csdn.net/qq_43380180/article/details/111573642#asyncawait_78)
-   -   [asyncio事件循环（python3.6）](https://blog.csdn.net/qq_43380180/article/details/111573642#asynciopython36_79)
    -   [asyncio事件循环（python3.7）](https://blog.csdn.net/qq_43380180/article/details/111573642#asynciopython37_105)
    -   [async](https://blog.csdn.net/qq_43380180/article/details/111573642#async_121)
    -   [await](https://blog.csdn.net/qq_43380180/article/details/111573642#await_145)
    -   [asyncio.create\_task()](https://blog.csdn.net/qq_43380180/article/details/111573642#asynciocreate_task_176)
    -   [asyncio.futures对象](https://blog.csdn.net/qq_43380180/article/details/111573642#asynciofutures_207)
-   [实例](https://blog.csdn.net/qq_43380180/article/details/111573642#_224)
-   [参考](https://blog.csdn.net/qq_43380180/article/details/111573642#_337)

介绍异步前，先简述几个计算中有意思的概念。

## [CPU](https://so.csdn.net/so/search?q=CPU&spm=1001.2101.3001.7020)的时间观

> 如果电脑的主频为2.6G，也就是每秒可以执行2.6\*10^9个指令，每个指令只需要0.38ns。  
> 假设一个指令时间比作人类能够感知的一秒钟来对比数据，如下图：  
> （借用一张网络download的CPU时间观的对比图）

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/49b60be59c58d7f32645a5f4e171490d.jpeg)

> 举个常见的例子  
> `1Gbps网络上传输2KB数据，真实延迟20微秒，CPU感觉过了14.4小时！！！`
> 
> -   CPU是计算机的核心，如果利用率低，不仅浪费资源，而且程序执行效率低下，需要耗费更长的时间（这是最应该关注的点）
> -   如果不提高程序执行效率，等同于“**谋财害命**”

## I/O（异步的瓶颈）

> -   **CPU的时间观**中可以看出除了`执行指令`和`一级缓存`，对CPU来说是可接受的，其他操作都**很慢很慢很慢很慢很慢很慢**(特别是从`上下文切换`开始)！！！
> -   回顾一下大学期间，被老师强调无数次的的`冯·诺依曼结构`
> -   数学家冯·诺依曼提出了计算机制造的三个基本原则，即采用二进制逻辑、程序存储执行以及计算机由五个部分组成（运算器、控制器、存储器、输入设备、输出设备），这套理论被称为**冯·诺依曼体系结构**。

> -   从**冯·诺依曼体系结构**得出，**运算器**和**控制器**主要集成在CPU上的，其余全是I/O，包括磁盘读写，网卡读写等等。
> -   异步可以提高程序效率，但是I/O依旧是最大的瓶颈！
> -   **因此I/O是程序效率的最大瓶颈！！！！！**
> -   **因此I/O是程序效率的最大瓶颈！！！！！**
> -   **因此I/O是程序效率的最大瓶颈！！！！！**  
>     （没有卡，重要的事情说三遍！）
> -   世界上最大的计算机程序，莫过于因特网，因此网络I/O（CPU的时间观）成为了I/O的最大瓶颈！
> -   除了宕机，网络I/O是**最慢的 最慢的 最慢的**！因此诸多异步框架基本都是处理网络I/O的。

-   后面介绍`使用协程去处理异步程序，尽可能的利用I/0操作时的等待时间去执行其他动作，提高程序执行效率。`

## 基础概念

`如果基础概念已经了解，请直接跳过本节。`

### 进程/线程

> -   **进程**（Process）是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的**基本单位**，是操作系统结构的基础。在早期面向进程设计的计算机结构中，进程是程序的基本执行实体；在当代面向线程设计的计算机结构中，**进程是线程的容器。程序是指令、数据及其组织形式的描述，进程是程序的实体。**
> -   **线程**（thread）是操作系统能够进行运算调度的**最小单位**。它被包含在进程之中，是进程中的实际运作单位。一条线程指的是进程中一个**单一顺序的控制流**，一个进程中可以并发多个线程，每条线程并行执行不同的任务。在Unix System V及SunOS中也被称为轻量进程（lightweight processes），但轻量进程更多指内核线程（kernel thread），而把用户线程（user thread）称为线程。
> -   **一个进程可以有很多线程，每条线程并行执行不同的任务。**

### 阻塞/非阻塞

> -   **阻塞和非阻塞**指的是调用者（程序）在等待返回结果（或输入）时的状态。
> -   **阻塞时**，在调用结果返回前，当前线程会被挂起，并在得到结果之后返回。
> -   **非阻塞时**，如果不能立刻得到结果，则该调用者不会阻塞当前线程。因此对应非阻塞的情况，调用者需要定时轮询查看处理状态。

### 并发/并行

> -   **并发**：在操作系统中，是指一个时间段中有几个程序都处于已启动运行到运行完毕之间，且这几个程序都是在同一个处理机上运行，但任一个时刻点上只有一个程序在处理机上运行。
> -   **并行**：在操作系统中是指，`一组程序按独立异步的速度执行`，无论从微观还是宏观，程序都是一起执行的。
> -   对比地讲，并发是：`在同一个时间段内，两个或多个程序执行，有时间上的重叠(宏观上是同时,微观上仍是顺序执行)`。

#### CPU调度策略

-   在并发运行中，CPU需要在多个程序之间来回切换，因此会有一些切换的调度策略。

> -   先来先服务 - 时间片轮转调度
> -   优先级调度
> -   最短作业优先
> -   最高响应比优先
> -   多级反馈队列调度

### 同步/异步

-   同步：在发出一个同步调用时，在没有得到结果之前，该调用就不返回。
-   异步：在发出一个异步调用后，调用者不会立刻得到结果，该调用就返回了。

> -   同步和阻塞的定义很像，但却是两个概念，同步不一定阻塞，在结果没有返回前，阻塞是指线程被挂起，**这段程序不再执行**，而同步调用是在运行状态，**CPU还在执行这段程序**。
> -   异步和非阻塞定义也很像，但也是两个概念，异步指在调用时不会立即得到结果，调用就返回了，线程可能阻塞，也可能不阻塞。而非阻塞是指调用的时候，线程一定不会进入非阻塞状态。

### 事件循环+回调


回调就是传一个地址

> -   所谓的事件循环，并非是一个真正意义上的循环，可以理解为一种定义，可以理解为是主线程不断的从**事件队列**里面取值/函数的过程，因为这一过程是不断发生的所以我们为了方便把这个过程叫**事件循环**。
> -   事件本身并没有循环的，循环的只是主线程取事件的动作。  
>     软件模块之间总是存在着一定的接口，从调用方式上，可以把他们分为三类：同步调用、回调和异步调用。
> -   同步调用是一种阻塞式调用，调用方要等待对方执行完毕才返回，它是一种单向调用；
> -   回调是一种双向调用模式，也就是说，被调用方在接口被调用时也会调用对方的接口;
> -   异步调用是一种类似消息或事件的机制，不过它的调用方向刚好相反，接口的服务在收到某种讯息或发生某种事件时，会主动通知客户方（即调用客户方的接口）。
> -   回调和异步调用的关系非常紧密，通常我们使用回调来实现异步消息的注册，通过异步调用来实现消息的通知。

## 协程（异步）

`python中为了提高I/O效率，使用协程去处理异步程序，协程自动完善了上述的各种调度任务！`

至于如何完善的，在之后的文章中会详述。（也可能会鸽了）

-   进程和线程是计算机提供的，协程是程序员创造的，不存在于计算机中。
    
-   协程（Co-routine），也可称为微线程，或非抢占式的多任务子例程，一种用户态的上下文切换技术（通过一个线程实现代码块间的相互切换执行）
    
-   意义：在一个线程（协程）中，遇到io等待时间，线程可以利用这个等待时间去做其他事情。
    

## async/await

### asyncio事件循环（python3.6）

-   事件循环：去检索一个任务列表的所有任务，并执行所有未执行任务，直至所有任务执行完成。
-   **执行协程函数，必须使用事件循环。**

```python
# python 源码 import asyncio async def func1(): print('协程1') async def func2(): print('协程2') # task可为列表,即任务列表 # task = func1() task = [func1(), func2()] # 创建事件循环 loop = asyncio.get_event_loop() # 添加任务，直至所有任务执行完成 loop.run_until_complete(asyncio.wait(task)) #关闭事件循环 loop.close() # 事件循环关闭后，再次调用loop，将不会再次执行。
```

### asyncio事件循环（python3.7）

-   python3.7省略的手动创建事件循环，可直接用`asyncio.run()`去执行协程任务。

```python
import asyncio async def func1(): print('协程1') async def func2(): print('协程2') task = [func1(), func2()] # python3.7引入的新特性，不用手动创建事件循环 asyncio.run(task)
```

### async

-   协程函数：定义函数时加上async修饰，即`async def func()`
-   协程对象：执行`协程函数`得到的对象

> 注：执行[协程函数](https://so.csdn.net/so/search?q=%E5%8D%8F%E7%A8%8B%E5%87%BD%E6%95%B0&spm=1001.2101.3001.7020)得到协程对象，函数内部代码不会执行

```python
# python 源码 >>> import asyncio >>> async def main(): ... print('hello') ... await asyncio.sleep(1) ... print('world') >>> main() <coroutine object main at 0x1053bb7c8> >>> asyncio.run(main()) hello world
```

-   执行协程函数内部代码，必须把协程对象交给`事件循环`处理

### await

-   await + 可等待对象（协程对象，Future，Task对象（IO等待））
-   `等待到对象的返回结果`，才会继续执行后续代码

```python
# python 源码 import asyncio import time async def say_after(delay, what): await asyncio.sleep(delay) print(what) async def main(): print(f"started at {time.strftime('%X')}") await say_after(1, 'hello') #await say_after(1, 'hello')执行完之后，才继续向下执行 await say_after(2, 'world') print(f"finished at {time.strftime('%X')}") asyncio.run(main())
```

output：

> started at 17:13:52  
> hello  
> world  
> finished at 17:13:55

### asyncio.create\_task()

> -   `asyncio.create_task()`作为异步并发运行协程的函数Tasks。
> -   将协程添加到`asyncio.create_task()`中，则该协程将很快的自动计划运行

```python
# python 源码 async def main(): task1 = asyncio.create_task( say_after(1, 'hello')) task2 = asyncio.create_task( say_after(2, 'world')) print(f"started at {time.strftime('%X')}") # Wait until both tasks are completed (should take around 2 seconds.) # 两个任务同时执行，直到到所有任务执行完成。 await task1 await task2 print(f"finished at {time.strftime('%X')}")
```

output:

> started at 17:14:32  
> hello  
> world  
> finished at 17:14:34

> 注：比不使用`asyncio.create_task()`的结果快了一秒，也即两个任务同时执行了。

### asyncio.futures对象

-   官方文档对Future的介绍如下

> Futures  
> A Future is a special low-level awaitable object that represents an eventual result of an asynchronous operation.  
> When a Future object is awaited it means that the coroutine will wait until the Future is resolved in some other place.  
> Future objects in asyncio are needed to allow callback-based code to be used with async/await.  
> Normally there is no need to create Future objects at the application level code.  
> Future objects, sometimes exposed by libraries and some asyncio APIs, can be awaited:

> 大概意思是：  
> Future是特殊的低级等待对象，代表异步操作的最终结果。  
> 当等待Future对象时，它意味着协程将等待，直到在其他地方解析Future。  
> 需要在asyncio中使用将来的对象，以允许将基于回调的代码与async / await一起使用。  
> 通常，不需要在应用程序级别的代码中创建Future对象。

-   使用async/await时 会自动创建Future对象。（在此作为了解即可）

## 实例

-   正常的网络请求

```python
# python 源码 import requests import time def result(url): res = request_url(url) print(url,res) def request_url(url): res = requests.get(url) print("\n",res) time.sleep(2) print("execute_time:",time.time()-start) return res url_list = ["https://www.csdn.net/", "https://blog.csdn.net/qq_43380180/article/details/111573642", "https://www.baidu.com/", ] start = time.time() print("start_time:",start) task = [result(url) for url in url_list] endtime = time.time()-start print("\nendtime:",time.time()) print("all_execute_time:",endtime)
```

output:

> start\_time: 1608888064.4879584
> 
> <Response \[200\]>  
> execute\_time: 4.062316656112671  
> https://www.csdn.net/ <Response \[200\]>
> 
> <Response \[200\]>  
> execute\_time: 6.691046237945557  
> https://blog.csdn.net/qq\_43380180/article/details/111573642 <Response \[200\]>
> 
> <Response \[200\]>  
> execute\_time: 9.305423259735107  
> https://www.baidu.com/ <Response \[200\]>
> 
> endtime: 1608888073.7933817  
> all\_execute\_time: 9.305423259735107

> 3个网络请求耗时9.3秒

-   使用协程

```python
# python 源码 import asyncio import requests import time async def result(url): res = await request_url(url) print(url, res) async def request_url(url): res = requests.get(url) print(url) await asyncio.sleep(2) print("execute_time:", time.time() - start) return res url_list = ["https://www.csdn.net/", "https://blog.csdn.net/qq_43380180/article/details/111573642", "https://www.baidu.com/", ] start = time.time() print(f"start_time:{start}\n") task = [result(url) for url in url_list] loop = asyncio.get_event_loop() loop.run_until_complete(asyncio.wait(task)) endtime = time.time() - start print("\nendtime:", time.time()) print("all_execute_time:", endtime)
```

output:

> start\_time:1608887916.4435036
> 
> https://blog.csdn.net/qq\_43380180/article/details/111573642  
> https://www.baidu.com/  
> https://www.csdn.net/  
> execute\_time: 2.6272454261779785  
> https://blog.csdn.net/qq\_43380180/article/details/111573642 <Response \[200\]>  
> execute\_time: 3.0642037391662598  
> https://www.baidu.com/ <Response \[200\]>  
> execute\_time: 3.9802708625793457  
> https://www.csdn.net/ <Response \[200\]>
> 
> endtime: 1608887920.4237745  
> all\_execute\_time: 3.9802708625793457

> 使用协程耗时3.9秒

> 对比结果可以看出，协程大大提高了程序的执行效率，程序数量越多，越能体现出协程对程序的执行效率越高！！！！

**注：使用协程时，需要其底层方法实现时就是协程，才会生效，否则协程不生效！**

-   此处使用的`requests`底层实现并不是异步，因此使用了`time.sleep()` 和 `asyncio.sleep()`模拟放大网络IO时间。
-   异步模块举例：aiohttp-requests、aiofiles、grequests等

文章中有不足或错误的地方，欢迎留言指正!

## 参考

-   协程和任务:  
    [https://docs.python.org/3.7/library/asyncio-task.html](https://docs.python.org/3.7/library/asyncio-task.html)
    
-   深入理解python异步  
    [https://mp.weixin.qq.com/s?\_\_biz=MzIxMjY5NTE0MA==&mid=2247483720&idx=1&sn=f016c06ddd17765fd50b705fed64429c](https://mp.weixin.qq.com/s?__biz=MzIxMjY5NTE0MA==&mid=2247483720&idx=1&sn=f016c06ddd17765fd50b705fed64429c)
    
-   Python异步编程 asyncio小白速通  
    [https://www.bilibili.com/video/BV1dD4y127bD?from=search&seid=15383041802813101586](https://www.bilibili.com/video/BV1dD4y127bD?from=search&seid=15383041802813101586)
