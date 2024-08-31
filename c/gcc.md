---
tags:
  - "#undo"
created-date: 2023-11-15 18:10
modifed_date: " 2023-11-15 18:10:36"
aliases: 
root: 
father: 
related: https://gcc.gnu.org/onlinedocs/gcc/
---
## 相关文档
[Top (Using the GNU Compiler Collection (GCC))](https://gcc.gnu.org/onlinedocs/gcc/)


## GCC 简介

Ø GCC（GNU Compiler Collection）
• https://gcc.gnu.org/
• 由 GNU开发的，遵循 GPL 许可证发行的编译器套件。
• 支持 C、C++、Objective-C、Fortran、Ada 和 Go 语
言等多种语言前端，已被移植到多种计算机体系架构
上，如 x86、ARM、RISC-V 等。
• GCC 的初衷是为 GNU 操作系统专门编写一款编译器，
现已被大多数 “Unix-like”操作系统（如 Linux、BSD、
MacOS 等）采纳为标准的编译器。

## GCC 的命令格式
`gcc [options] [filenames]`
![[Pasted image 20240724180957.png]]

## GCC 的主要执行步骤

1. 编译（cc1，这里针对 C 语言，不同的语言有自己的编译器）：编译器完成 “预处理” 和 “编译”，“预处理” 指处理源文件中以 “#” 开头的预处理指令，譬如 `#include`、`#define` 等；“编译” 则针对预处理的结果进行一系列的词法分析、语法分析、语义分析，优化后生成汇编指令，存放在 .o 为后缀的目标文件中。
2. 汇编（as）：汇编器将汇编语言代码转换为机器（CPU）可以执行的指令。
3. 链接（ld）：链接器将汇编器生成的目标文件和一些标准库（譬如 libc）文件组合，形成最
终可执行的应用程序。
![[Pasted image 20240724181006.png]]
## GCC 涉及的文件类型
• .c：C 源文件
• .cc/.cxx/.cpp：C++ 源文件
• .i：经过预处理的 C 源文件
• .s/.S：汇编语言源文件
• .h：头（header）文件
• .o：目标（object）文件
• .a/.so：编译后的静态库（archive）文件和共享库
（shared object）文件
• a.out：可执行文件

## 针对多个源文件的处理


![[Pasted image 20240724181338.png]]