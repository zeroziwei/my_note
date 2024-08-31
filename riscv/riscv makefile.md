---
tags: 
created_date: 2024-04-24 16:27
modified_date: ' 2024-04-24 16:27:33'
review_date: 
pomodoro_time: 
used_time: 
aliases:
---
## Common. mk 通用配置信息

```makefile 
CROSS_COMPILE = riscv64-unknown-elf-
CFLAGS = -nostdlib -fno-builtin -march=rv32g -mabi=ilp32 -g -Wall

QEMU = qemu-system-riscv32
QFLAGS = -nographic -smp 1 -machine virt -bios none

GDB = gdb-multiarch
CC = ${CROSS_COMPILE}gcc
OBJCOPY = ${CROSS_COMPILE}objcopy
OBJDUMP = ${CROSS_COMPILE}objdump

```

这条命令使用 `riscv64-unknown-elf-gcc` 编译器编译 `start.S` 汇编文件，生成目标文件 `start.o`。下面是命令中各个选项的含义：
- `-nostdlib`：告诉编译器不使用标准库，不链接标准库函数。
- `-fno-builtin`：禁用内建函数，不使用编译器提供的内建函数。
- `-march=rv32g`：指定生成的代码针对 RISC-V 架构的 rv32g 子集。 
- `-mabi=ilp32`：指定使用 ilp32 数据模型，即 32 位整数、长整数和指针。 
-  `-g`：生成调试信息，方便调试程序。
-  `-Wall`：开启所有警告信息。
-  `-D CONFIG_SYSCALL`：定义一个名为 `CONFIG_SYSCALL` 的宏。
-  `-c`：只编译不链接，生成目标文件。
-  `-o start.o`：指定生成的目标文件名为 `start.o`。
-  `start.S`：指定要编译的源文件为 `start.S`，这里的 `.S` 表示汇编文件。
-  通过这条命令，可以将 `start.S` 汇编文件编译成目标文件 `start.o`，用于后续链接生成可执行文件或者库文件。

> commom. mk 可以理解为全局变量

这是一个使用 QEMU 模拟器来运行 RISC-V 32位架构的操作系统的命令。让我解释一下各个参数的含义：
- `qemu-system-riscv32`: 这是 QEMU 模拟器的命令，用于模拟运行 RISC-V 32位架构的程序。
-  `-nographic`: 这个参数告诉 QEMU 不要使用图形界面，而是在终端中显示输出。
-  `-smp 1`: 这个参数指定了模拟器中的处理器数量，这里是 1 个处理器。
-  `-machine virt`: 这个参数指定了使用 virt 机器模型，这是 QEMU 中用于模拟虚拟机的一种模型。
-  `-bios none`: 这个参数告诉 QEMU 不要加载任何 BIOS，因为在这种情况下，操作系统的启动是直接从内核开始的。 
- `-kernel os.elf`: 这个参数指定了要加载和运行的操作系统内核文件，这里是 `os.elf`。

通常，操作系统内核文件会被加载到模拟器中，并且模拟器会开始执行内核中的代码来启动操作系统。 综合起来，这个命令告诉 QEMU 模拟器使用 RISC-V 32位架构来运行一个操作系统，不使用图形界面，在终端中显示输出，使用 1 个处理器，使用 virt 机器模型，不加载 BIOS，直接加载并运行 `os.elf` 内核文件。
## rule. mk 规则文件

```make
nclude ../../common.mk

.DEFAULT_GOAL := all
all:
	@${CC} ${CFLAGS} ${SRC} -Ttext=0x80000000 -o ${EXEC}.elf
	@${OBJCOPY} -O binary ${EXEC}.elf ${EXEC}.bin

.PHONY : run
run: all
	@echo "Press Ctrl-A and then X to exit QEMU"
	@echo "------------------------------------"
	@echo "No output, please run 'make debug' to see details"
	@${QEMU} ${QFLAGS} -kernel ./${EXEC}.elf

.PHONY : debug
debug: all
	@echo "Press Ctrl-C and then input 'quit' to exit GDB and QEMU"
	@echo "-------------------------------------------------------"
	@${QEMU} ${QFLAGS} -kernel ${EXEC}.elf -s -S &
	@${GDB} ${EXEC}.elf -q -x ${GDBINIT}

.PHONY : code
code: all
	@${OBJDUMP} -S ${EXEC}.elf | less

.PHONY : hex
hex: all
	@hexdump -C ${EXEC}.bin

.PHONY : clean
clean:
	rm -rf *.o *.bin *.elf
```


## build. mk

``` make 
EXEC = test

SRC = ${EXEC}.s

GDBINIT = ../gdbinit

include ../rule.mk
```

## gdbinit 

``` makefile 
display/z $x5
display/z $x6
display/z $x7

set disassemble-next-line on
b _start
target remote : 1234
c
```