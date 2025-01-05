---
tags: 
created_date: 2024-01-13 15:32
modified_date: ' 2024-01-13 15:32:00'
review_date: 
pomodoro_time: 
used_time: 
aliases:
---
## ISA 是什么

ISA（Instruction Set Architecture）指令集架构：是底层硬件电路面向上层软件程序提供的一层接口规范。
ISA 定义了：
• 基本数据类型：BYTE/HALFWORD/WORD/……
• 寄存器（Register）
• 指令
• 寻址模式
• 异常或者中断的处理方式
• 等等 ......

![[Pasted image 20240113153303.png|500]]


## 为什么需要ISA

为上层软件提供一层抽象，制定规则和约束，让编程者不用操心具体的电路结构。

IBM 360 是第一个将 ISA 与其实现分离的计算机。

## CISC vs RISC

CISC 复杂指令集
（Complex Instruction Set Computing）
• 针对特定的功能实现特定的指令，导致指令数目比较
多，但生成的程序长度相对较短。

RISC 精简指令集
（Reduced Instruction Set Computing）
• 只定义常用指令，对复杂的功能采用常用指令组合实
现，这导致指令数目比较精简，但生成的程序长度相
对较长。

现如今，RISC 和 CISC 也逐渐有相互融合的趋
势

## ISA 的宽度

ISA （处理器）的宽度指的是 CPU 中 通用寄存器 的
宽度（二进制的位数），这决定了寻址范围的大小、以
及数据运算的能力。

![[Pasted image 20240113153454.png]]


注意一个问题：ISA 的宽度和指令编码长度无关。

## RISC-V 的历史简介

RISC-V 念作 “risk-five”，代表着 Berkeley 所研发的第五代精简指令集。
该项目 2010 年始于加州大学伯克利（Berkeley）分校，希望选择一款 ISA
用于科研和教学。经过前期多年的研究和选型，最终决定放弃使用现成的
X86 和 ARM 等 ISA，而是自己从头研发一款：
• X86：太复杂，IP 问题
• ARM：一样的复杂，而且在 2010 年之前还不支持 64 位，以及同样的 IP 问题。
主要研发人员：
• Andrew Waterman, Yunsup Lee, David Patterson,
Krste Asanovic

## RISC-V 究竟是什么

一款高质量，免许可证，开放的
RISC ISA
Ø 一套由非营利的 RISC-V 基金会
维护的标准: https://riscv.org/
Ø 适用于所有类型的计算系统：从
微控制器到超级计算机
Ø RISC-V 不是一家公司，也不是一
款 CPU 实现。

## RISC-V 的特点

Ø 简单
Ø 清晰的分层设计
Ø 模块化
Ø 稳定
Ø 社区化

## RISC-V ISA 命名规范
![[riscv_ISA-20240424174331346.webp|1463]]

## 模块化的 ISA

增量 ISA: 计算机体系结构的传统方法，同一个体系架构下的新一代处理
器不仅实现了新的 ISA 扩展，还必须实现过去的所有扩展，目的是为了
保持向后的二进制兼容性。典型的，以 80x86 为代表
![[Pasted image 20240113154057.png]]
模块化 ISA: 由 1 个基本整数指令集 + 多个可选的扩展指令集组成。基
础指令集是固定的，永远不会改变。

RISC ISA = 1 个基本整数指令集 + 多个可选的扩展指令集

![[Pasted image 20240113154200.png]]


![[Pasted image 20240113154715.png]]

## Hart 
![[Pasted image 20240113154734.png]]
![[Pasted image 20240113154757.png]]


## 特权级别（ Privileged Level）

![[Pasted image 20240113154908.png]]

## Control and Status Registers (CSR)

![[Pasted image 20240113155121.png]]


## 内存管理与保护

![[Pasted image 20240113155213.png]]

## 异常和中断
Ø 异常（Exception）：“an unusual condition occurring
at run time associated with an instruction in the
current RISC-V hart”
Ø 中断（Interrupt）：“an external asynchronous event
that may cause a RISC-V hart to experience an
unexpected transfer of control”

![[Pasted image 20240113155533.png]]

