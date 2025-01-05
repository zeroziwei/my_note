---
tags: 
created_date: 2024-01-13 16:03
modified_date: " 2024-01-13 16:03:52"
review_date: 
pomodoro_time: 
used_time: 
aliases: 
---
[[ELF 文件]]

## ELF 文件是什么
ELF（Executable Linkable Format）是一种 Unix-like系统上的二进制文件格式标准。ELF 标准中定义的采用 ELF 格式的文件分为 4 类：

![[Pasted image 20240724180902.png]]
## ELF 文件格式

![[Pasted image 20240724180914.png]]


## ELF 文件处理相关工具：Binutils

https://www.gnu.org/software/binutils/

• ar：归档文件，将多个文件打包成一个大文件。
• as：被 gcc 调用，输入汇编文件，输出目标文件供链接器 ld 连接。
• ld：GNU 链接器。被 gcc 调用，它把目标文件和各种库文件结合在一起，重定位数据，并链接符号引用。
• objcopy：执行文件格式转换。
• objdump：显示 ELF 文件的信息。
• readelf：显示更多 ELF 格式文件的信息（包括DWARF 调试信息）。
• ......

## 学习编译和链接的好处

Ø 有利于程序员优化程序的性能。譬如：switch vs if-else；函数调用的开销，传参数时传变量还是指针，......
Ø 理解并解决编译链接时出现的错误。譬如：“error:XXXXXX redefined”，“error: cannot find
XXXXXX”, ......
Ø 写出更健壮的程序。缓冲区溢出，非法访问，“Segmentation fault”，......

