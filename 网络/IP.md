**网际协议**（英语：Internet Protocol，缩写：**IP**），又称**互联网协议**，是互联网协议套件中的网络层通信协议，用于跨网络边界[封包交换](https://www.wikiwand.com/zh-hans/%E5%B0%81%E5%8C%85%E4%BA%A4%E6%8F%9B "封包交换")。它的路由功能实现了互联互通，并从本质上建立了互联网。 ^iw0bxrps

IP是在[TCP/IP协议族](https://www.wikiwand.com/zh-hans/TCP/IP%E5%8D%8F%E8%AE%AE%E6%97%8F "TCP/IP协议族")中[网络层](https://www.wikiwand.com/zh-hans/%E7%BD%91%E7%BB%9C%E5%B1%82 "网络层")的主要协议，任务是仅根据数据包标头中的[IP地址](https://www.wikiwand.com/zh-hans/IP%E5%9C%B0%E5%9D%80 "IP地址")将数据包从源主机传递到目标主机。为此，IP协议定义了封装要传递的数据的数据包结构。它还定义了用于用源和目的地信息标记数据报的寻址方法。

第一个架构的主要版本为[IPv4](https://www.wikiwand.com/zh-hans/IPv4 "IPv4")，目前仍然是广泛使用的互联网协议，尽管世界各地正在积极部署[IPv6](https://www.wikiwand.com/zh-hans/IPv6 "IPv6")。

## IP封装

数据在IP互联网中传送时会封装为[数据包](https://www.wikiwand.com/zh-hans/%E6%95%B0%E6%8D%AE%E5%8C%85 "数据包")。网际协议的独特之处在于：在报文交换网络中主机在传输数据之前，无须与先前未曾通信过的目的主机预先建立好特定的“通路”。互联网协议提供了“不可靠的”数据包传输机制（也称“尽力而为（英语：[Best-effort delivery](https://en.wikipedia.org/wiki/Best-effort_delivery "en:Best-effort delivery")）”或“尽最大努力交付”）；也就是说，它不保证数据能准确的传输。数据包在到达的时候可能已经损坏，顺序错乱（与其它一起传送的封包相比），产生冗余包，或者全部丢失。如果[应用](https://www.wikiwand.com/zh-hans/%E5%BA%94%E7%94%A8%E8%BD%AF%E4%BB%B6 "应用软件")需要保证可靠性，一般需要采取其他的方法，例如利用IP的上层协议控制。

>[!tldr] 
>如果只有ip层会怎么样

