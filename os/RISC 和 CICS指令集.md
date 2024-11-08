---
tags: 
progress: 
creation date: 2024-07-28 13:33
modification date: 星期日 28日 七月 2024 13:33:43
---
86-64有时称为“复杂指令集计算机”（CISC，读作“sisk), 与“精简指令集计算机”（RISC，读作“risk )　相对。

从历史上看，先出现了CISC机器，它从最早的计算机演化而来。到20世纪80年代早期，随着机器设计者加入了很多新指令来支持高级任务(例如处理循环缓冲区，执行十进制数计算，以及求多项式的值），大型机和小型机的指令集已经变得非常庞大了。最早的微处理器出现在20世纪70年代早期，因为当时的集成电路技术极大地制约了一块芯片上能实现些什么，所以它们的指令集非常有限。微处理器发展得很快，到20世纪80年代早期，大型机和小型机的指令集复杂度一直都在增加。x86家族沿着这条道路发展到IA32,最近是x86-64ÿ即使是x86系列也仍然在不断地变化，基于新出现的应用的需要，增加新的指令类。20世纪80年代早期，RISC的设计理念是作为上述发展趋势的一种替代而发展起来的。IBM的一组硬件和编译器专家受到IBM研究员JohnCocke的很大影响，认为他们可以为更简单的指令集形式产生高效的代码。实际上，许多加到指令集中的高级指令很难被编译器产生，所以也很少被用到。一个较为简单的指令集可以用很少的硬件实现，能以高效的流水线结构组织起来，类似于本章后面描述的情况。直到多年以后IBM才将这个理念商品化，开发出了Power和PowerPCISAÿ加州大学伯克利分校的DavidPatterson和斯坦福大学的JohnHennessy进一步发展了RISC的概念。Patterson将这种新的机器类型命名为RISCÿ而将以前的那种称为CISC,因为以前没有必要给一种几乎是通用的指令集格式起名字。比较CISC和最初的RISC指令集，我们发现下面这些一般特性