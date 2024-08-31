>[!question]
>如何在不可靠的网络层上,实现可靠的数据传输

![[Pasted image 20240703031738.png]]
- lost 
- corrupt
- reorder data
- 发送放不知道发生了什么 

![[Pasted image 20240703031843.png]]

隔这猜疑链是吧


Reliable data transfer protocol (rdt): interfaces

![[Pasted image 20240703034506.png]]

1. rdt_send()
2. udt_send()
3. ret_rcv()
4. deliver_data()


## rdt2.0: channel with bit errors

- underlying channel may flip bits in packet
	- checksum (e.g., Internet checksum) to detect bit errors
	- the question: how to recover from errors?

>[!tip]
>How do humans recover from “errors” during conversation?

![[Pasted image 20240703234553.png]]

![[Pasted image 20240703234650.png]]


>[!hint]
>如果 ack / nak 发生错误了怎么办

> 人类的一脸懵逼总不会出问题


> something to you and you reply back to me I'll 阿吧啊吧啊吧


## rdt 3.0

>[!hint] 
>假如你是个老师，你问学生问题，学生不理你怎么办？
>
>repeat



