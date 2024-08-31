> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [wulc.me](https://wulc.me/2020/06/14/%E7%A8%8B%E5%BA%8F%E7%9A%84%E8%A1%A8%E7%A4%BA%E3%80%81%E8%BD%AC%E6%8D%A2%E4%B8%8E%E9%93%BE%E6%8E%A5-week5%E3%80%816/)

> 本文是 程序的表示、转换与链接 中第 5、6 周的内容，主要介绍了程序和指令的关系，目标文件的基本格式，并详细地介绍了 IA-32 体系下的指令系统，包括各种指令的类型、指令执行的基本流程，通过以上内容可以对计算机内部如何执行程序有一个感性的认识。

本文是 [程序的表示、转换与链接](https://www.coursera.org/learn/jisuanji-xitong/home/welcome) 中第 5、6 周的内容，主要介绍了程序和指令的关系，目标文件的基本格式，并详细地介绍了 IA-32 体系下的指令系统，包括各种指令的类型、指令执行的基本流程，通过以上内容可以对计算机内部如何执行程序有一个感性的认识。

目前所用的主流计算机基本上都是基于 intel 架构的，因此本文主要基于 IA-32 指令系统介绍

程序与指令
-----

在介绍 IA-32 指令系统之前，需要了解如何将高级语言程序转换为用机器指令表示的机器代码

[程序的表示、转换与链接 - week1](http://wulc.me/2020/05/30/%E7%A8%8B%E5%BA%8F%E7%9A%84%E8%A1%A8%E7%A4%BA%E3%80%81%E8%BD%AC%E6%8D%A2%E4%B8%8E%E9%93%BE%E6%8E%A5-week1/) 中介绍了计算机的硬件基本组成，基于这个硬件上面 采用的是一种**存储程序的工作方式**：即所有要通过计算机完成的任务事先要编写成程序，然后将程序装入到存储器里面，一旦要执行这个程序，计算机就可以把 程序当中的指令一条一条取到这个 CPU 里面自动的来完成

从计算机的执行角度来看，程序可认为是由指令和数据组成的，它们都是存放在存储器里面的， 指令和数据都是用二进制的 01 进行编码的，**在指令执行过程当中，指令和数据从存储器要取到 CPU，然后存放在 CPU 的寄存器里面**，两者的关系如下图所示

[![](https://wulc.me/imgs/instructionAndData.jpg)](https://wulc.me/imgs/instructionAndData.jpg "instructionAndData") instructionAndData

上面说的指令指的是机器指令，在一些其他地方往往也能看到**微指令与伪指令**的概念，三者简单对比如下

*   指令: 也叫机器指令，在**硬件和软件交界面上**的，表示计算机的一个完整的基本动作
*   微指令: **硬件范畴**，一条机器指令的功能是若干条微指令组成的序列来实现的
*   伪指令：**软件范畴**，由若干个机器指令构成的一个指令序列

机器指令和汇编指令是一一对应的，由硬件决定

目标代码
----

这一小节的内容基本在 [《链接、装载与库》 阅读笔记 (1)- 基本概念与静态链接](http://wulc.me/2020/05/31/%E3%80%8A%E9%93%BE%E6%8E%A5%E3%80%81%E8%A3%85%E8%BD%BD%E4%B8%8E%E5%BA%93%E3%80%8B%E9%98%85%E8%AF%BB%E7%AC%94%E8%AE%B0%281%29-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%E4%B8%8E%E9%9D%99%E6%80%81%E9%93%BE%E6%8E%A5/) 有描述，因此这里只做简单的记录，下图描述了编译的过程，即如何从源文件生成目标文件

[![](https://wulc.me/imgs/compile.jpg)](https://wulc.me/imgs/compile.jpg "compile") compile

上图中的 `.c` 表示 C 语言的源文件、`.i` 表示预编译文件、`.s` 表示汇编文件、`.o` 表示目标文件 (也叫可重定位目标文件)

从上图可知，原始的汇编文件中的指令与反汇编得到的汇编指令在形式上有一点差别，主要体现在 (1) 编译得到的汇编指令的助记符当中都带长度后缀，而反汇编的都没有 (2) 编译得到的汇编指令用的是十进制，反汇编用的则是十六进制

此外的 **test.o 里面的起始地址总是从零开始，因为它还没有链接，还没生成可执行文件，所以它的地址是不确定的**，需要被链接生成可执行目标文件，链接的过程也可参考上面的[文章](http://wulc.me/2020/05/31/%E3%80%8A%E9%93%BE%E6%8E%A5%E3%80%81%E8%A3%85%E8%BD%BD%E4%B8%8E%E5%BA%93%E3%80%8B%E9%98%85%E8%AF%BB%E7%AC%94%E8%AE%B0%281%29-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%E4%B8%8E%E9%9D%99%E6%80%81%E9%93%BE%E6%8E%A5/)，从下图中可知，可执行目标文件中的地址不再是从 0 开始的，而是一个确切的地址

[![](https://wulc.me/imgs/ObjectFileCompare.jpg)](https://wulc.me/imgs/ObjectFileCompare.jpg "objectFilesCompared") objectFilesCompared

这个确切的地址是什么地址呢？实际上是一个虚拟地址，它是在一个虚拟地址空间里面的，其实就是进程的虚拟地址空间，如下图所示

[![](https://wulc.me/imgs/object_file_mapping.jpg)](https://wulc.me/imgs/object_file_mapping.jpg "object file mapping") object file mapping

Intel 处理器与 IA-32 指令系统
---------------------

x86 实际上是 Intel 开发的一类处理器体系结构的一个泛称，比如说 8086、80286 是 16 位的； 而 386、486 这些是 32 位的 因为这些架构后面都带有 86，所以就称为 x86，Intel 处理器中涉及的产品如下

[![](https://wulc.me/imgs/IntelProcess.jpg)](https://wulc.me/imgs/IntelProcess.jpg "IntelProcessor") IntelProcessor

Intel 把 32 位的 x86 架构的名称 x86-32 改为 IA -32，IA 实际上是 Intel Architecture 的缩写 因此 **IA-32 就是 Intel 体系结构的 32 位机器的一个泛称**

IA-32 体系结构规定了寄存器的个数、 各自指令功能以及寄存器的宽度、存储空间等；整体体系结构大致如下图所示

[![](https://wulc.me/imgs/IA-32.jpg)](https://wulc.me/imgs/IA-32.jpg "IA-32") IA-32

从上图可知，IA-32 的体系结构的一些特点如下

*   8 个通用寄存器，编号就是 0 到 7
*   2 个专用寄存器：标志寄存器 EFLAGS 和指令计数器 (PC), 叫 EIP
*   6 个段寄存器（间接给出段基址）
*   可寻址的空间是 4GB，也就是 寻址空间是从 0 一直到 0xffffffff

随着体系结构从 8 位的机器，逐渐扩展到 16 位、32 位的机器，寄存器的宽度也在不断地扩展，如下图所示是

[![](https://wulc.me/imgs/IA32Register.jpg)](https://wulc.me/imgs/IA32Register.jpg "IA32-Register") IA32-Register

后面的 80 位的寄存器和 120 位的寄存器在后面的内容中会有介绍。

寻址方式
----

什么叫寻址方式？寻址方式是**根据指令当中给出来的信息得到操作数或者操作数地址的方式** 根据操作数可能的地址，寻址方式也有多种

*   指令中：立即寻址
*   寄存器中：寄存器寻址
*   存储单元 (属于**存储器操作数，按字节编址**)：其他寻址方式（多种）

详细如下图所示

[![](https://wulc.me/imgs/SearchAddress.jpg)](https://wulc.me/imgs/SearchAddress.jpg "searchAdd") searchAdd

下面是高级语言中的一个寻址例子，描述了声明变量时，内存的分配情况以及访问变量时的寻址方法（以 Windows 为例）

[![](https://wulc.me/imgs/CSearchAddress.jpg)](https://wulc.me/imgs/CSearchAddress.jpg "CSearchAddress") CSearchAddress

前面提到，寻址指的是从如何从机器指令中获取找到操作数的地址，那 IA-32 中的指令是怎么组织然后坐到这一点的呢？ 下面是关于这个指令的一张详细的图，比较复杂，这里不展开讨论了

[![](https://wulc.me/imgs/MachineCode.jpg)](https://wulc.me/imgs/MachineCode.jpg "MachineCode") MachineCode

IA-32 常用指令类型
------------

### 传送指令

IA-32 当中的指令类型有很多种，最基本的就是传送指令，就是从一个地方传到另外一个地方。传送指令又可分为以下几种

*   数据传送指令，数据传送指令一般是用 MOV 指令来实现的；
*   地址传送指令，用于加载有效地址
*   输入输出指令，用于输入 / 输出端口和通用寄存器之间进行数据的交换
*   标志传送指令，将标志寄存器的值入 / 出栈

如下图中的 LEA (Load Effective Address) 指令就是用来加载有效地址的，leal 做的就是把 edx 的值 (相当于是基址值) 加上 eax 的值 (相当于是变址值), 基址加变址，这样两个寄存的内容加起来，这样就是一个有效地址

[![](https://wulc.me/imgs/TransformInstruction.jpg)](https://wulc.me/imgs/TransformInstruction.jpg "Transform Instruction") Transform Instruction

如下如所示是一个程序通过反汇编得到的指令，标红的是就是数据传送指令；实际执行时会从上到下依次执行

[![](https://wulc.me/imgs/Program2Instruction.jpg)](https://wulc.me/imgs/Program2Instruction.jpg "program2instruction") program2instruction

下面以上图中第一条指令即 55 这条指令介绍其执行过程， 这条指令可大概分为 3 个步骤，下面其描述过程，可配合后面的图进一步理解

1.  取指令，取指令的过程主要分为下面 3 步（1）控制器把指令地址（即 80483d4）写入指令计数器即 PC，在 IA-32 中 PC 为 EIP，然后 MAR **把指令的地址送到地址线**（2）时控制器会发出一个读命令，**把控制信号送到控制线**上面去，告诉存储器说，我要去读 读这个单元的内容（3）存储器接收到读命令和地址以后，开始进行读 读操作并**把地址里面的内容读到数据线**
2.  指令译码，数据线上的指令（即 5589e5）读过来，读到 MDR 里面去以后，再把它送到 IR 即指令寄存器里面去译码，而指令寄存器里面的高位，就是 op 字段，会送到控制器里面进行译码； 译码以后，计算机就知道这条指令的功能是**把 esp (栈指针，用于指向栈的栈顶) 内容减 4 送到 esp（即入栈，因为栈地址是从高到低增长的）; 然后把 ebp (帧指针，指向当前活动记录的底部) 的内容送到 esp 所指向的那个内存单元**
3.  指令执行，这一步执行的其实就是根据步骤 2 译码出来的过程，对 esp 减 4 是在 ALU 中完成的（就是从 bfff0000 变为 beeefffc），然后把 ebp 的内容 (即 bfff0020) 写到 beeefffc 中，原理也跟取指令的过程差不多，MAR 把地址 beeefffc 送到地址线上，同时把写的数据（即 ebp 的内容）写入 MDR 中， MDR 通过数据线把内容传输给存储器；这样当控制器发一个写信号到存储器时，存储器会将数据线的内容写入到存储器中，最后则是将 EIP 加 1，继续执行下一条指令

下图是上面步骤 1~2 步骤的执行过程和结果

[![](https://wulc.me/imgs/mov_instruction_exec.jpg)](https://wulc.me/imgs/mov_instruction_exec.jpg "step1-2") step1-2

下图是上面步骤 3 步骤的执行过程和结果

[![](https://wulc.me/imgs/mov_instruction_exec1.jpg)](https://wulc.me/imgs/mov_instruction_exec1.jpg "step3") step3

### 定点运算指令

常用的定点运算指令包括下面这些，可以有个大概的认识，这里就不详细展开讲了

[![](https://wulc.me/imgs/fixednum_instruction.jpg)](https://wulc.me/imgs/fixednum_instruction.jpg "fixed num") fixed num

### 逻辑运算指令

常用的逻辑运算指令包括下面这些，同样地可以有个大概的认识，不详细展开讲了

[![](https://wulc.me/imgs/logic_instruction.jpg)](https://wulc.me/imgs/logic_instruction.jpg "logic instruction") logic instruction

### 条件转移指令

**指令的执行有两种顺序**，一种是按顺序执行，还有一种是跳转到一个转移目标指令处执行，也称为条件转移指令

条件转移指令也可以根据是否要满足条件来跳转分为以下几种

[![](https://wulc.me/imgs/jump_instruction.jpg)](https://wulc.me/imgs/jump_instruction.jpg "jump instruction") jump instruction

*   **无条件转移指令**，比如说 jump 指令就是无条件地转移到 DST 所指出的那个目标指令处执行
*   **条件转移指令**，这个指令属于分支指令，可能是按顺序执行，也可能要跳转到转移目标指令处执行；所以在这个指令当中有一些是表示条件的 助记符，如上面的 cc 就是一个条件码，是根据标志 也就是条件标志来判断是否满足条件 如果满足条件就转移到目标指令 DST 处执行，否则就按顺序执行
*   **条件设置指令**， 跟上面的条件转移指令类似的 在指令助记符当中有条件码，如果满足这个条件就把目标寄存器 DST 里面置 1，否则是 0
*   **调用和返回指令**，就是在函数调用或者过程调用的时候使用的指令，调用指令就是 call 指令 CALL，返回指令就是 return 指令 RET。这两个指令会通过调用函数的地址关联起来，其实就是我们常听到的函数调用时入栈和出栈的操作
*   **中断指令**它也是一种跳转指令， 实际上是**从用户态跳转到了内核态** 详细的信息在后面的章节中介绍

在这些条件转移或者条件设置指令 当中有一个非常重要的寄存器就是标志寄存器，如下图所示，这些标志信息 实际上是用来进行判断条件是否满足的一些信息

[![](https://wulc.me/imgs/flag_register.jpg)](https://wulc.me/imgs/flag_register.jpg "flag register") flag register

在 C 语言里面规定，在一个表达式当中 只要有一个变量是无符号数变量，unsigned 的， 那么所有的运算都按无符号数进行运算。 所以这边比较的结果按无符号数来比。

小结
--

IA-32 是典型的 CISC (复杂指令集计算机) 风格 ISA，前几章提到 ISA 是一种规约，**规定了软件如何使用硬件**，包括指令的集合、寄存器的类型和数量、指令可接受的操作数类型

而 IA-32 复杂的点在于：指令可能会很长 有些字段有可无，操作码和指令是不定长 且每个字段的含义还要有另外一个字段来解释

其特点包括

*   有 8 个通用的寄存器， 并且可以扩展，从 8 位扩展到 16 位，到 32 位
*   有 2 个专用寄存器：EIP（PC）、标志寄存器（EFLAGS）
*   有 6 个段寄存器是间接的给出段址（存放的是一个指针，根据这个指针到另外一个地方去取，取出来的就是段基址）
*   寻址空间是 4 个 GB (存储单元的地址是 32 位的)
*   寻址方式有多种
    *   立即寻址
    *   寄存器寻址
    *   存储器寻址
    *   相对寻址
*   指令和操作码是变长的

同时介绍了 IA-32 常用指令类型包括传送指令、运算指令、逻辑指令、条件转移指令等，并且介绍了一些执行结果不符合源程序的例子，其原因往往是类型的默认装换导致了最终生成的指令不同，这一点可以作为程序排查时的一个思路。