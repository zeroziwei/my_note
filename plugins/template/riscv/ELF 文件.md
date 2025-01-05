ELF 文件格式是一个开放标准，各种 UNIX 系统的可执行文件都采用 ELF 格式，它有三种不同的类型：

*   可重定位的目标文件（Relocatable，或者 Object File）
    
*   可执行文件（Executable）
    
*   共享库（Shared Object，或者 Shared Library）
    

共享库留到[第 4 节 “共享库”](https://akaedu.github.io/book/ch20s04.html#link.shared) 再详细介绍，本节我们以[例 18.2 “求一组数的最大值的汇编程序”](https://akaedu.github.io/book/ch18s03.html#asm.max) 为例讨论目标文件和可执行文件的格式。现在详细解释一下这个程序的汇编、链接、运行过程：

1.  写一个汇编程序保存成文本文件 [[max.s]]。
    
2.  汇编器读取这个文本文件转换成目标文件`max.o`，目标文件由若干个 Section 组成，我们在汇编程序中声明的`.section`会成为目标文件中的 Section，此外[[汇编器]]还会自动添加一些 Section（比如符号表）。
    
3.  然后链接器把目标文件中的 Section 合并成几个 Segment，生成可执行文件`max`。
    
4.  最后加载器（Loader）根据可执行文件中的 Segment 信息加载运行这个程序。
    

ELF 格式提供了两种不同的视角，链接器把 ELF 文件看成是 Section 的集合，而加载器把 ELF 文件看成是 Segment 的集合。如下图所示。

**图 18.1. ELF 文件**

![](https://akaedu.github.io/book/images/asm.elfoverview.png)

左边是从链接器的视角来看 ELF 文件，开头的 ELF Header 描述了体系结构和操作系统等基本信息，并指出[[Section Header Table]] 和 [[Program Header Table]]在文件中的什么位置，Program Header Table 在链接过程中用不到，所以是可有可无的，Section Header Table 中保存了所有 Section 的描述信息，通过 Section Header Table 可以找到每个 Section 在文件中的位置。

右边是从加载器的视角来看 ELF 文件，开头是 ELF Header，Program Header Table 中保存了所有 Segment 的描述信息，Section Header Table 在加载过程中用不到，所以是可有可无的。从上图可以看出，**一个[[ Segment]] 由一个或多个 [[Section]] 组成，这些 Section 加载到内存时具有相同的访问权限**。

有些 Section 只对链接器有意义，在运行时用不到，也不需要加载到内存，那么就不属于任何 Segment。注意 Section Header Table 和 Program Header Table 并不是一定要位于文件的开头和结尾，其位置由 ELF Header 指出，上图这么画只是为了清晰。

目标文件需要[[链接器]]做进一步处理，所以一定有 [[Section Header Table]]；可执行文件需要加载运行，所以一定有 [[Program Header Table]]；而共享库既要加载运行，又要在加载时做动态链接，所以既有 Section Header Table 又有 Program Header Table。

下面用`readelf`工具读出目标文件`max.o`的 [[ELF Header]] 和 Section Header Table，然后我们逐段分析。

```
$ readelf -a max.o 
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              REL (Relocatable file)
  Machine:                           Intel 80386
  Version:                           0x1
  Entry point address:               0x0
  Start of program headers:          0 (bytes into file)
  Start of section headers:          200 (bytes into file)
  Flags:                             0x0
  Size of this header:               52 (bytes)
  Size of program headers:           0 (bytes)
  Number of program headers:         0
  Size of section headers:           40 (bytes)
  Number of section headers:         8
  Section header string table index: 5
...
```

ELF Header 中描述了操作系统是 UNIX，体系结构是 80386。Section Header Table 中有 8 个 Section Header，从文件地址 200（0xc8）开始，每个 Section Header 占 40 字节，共 320 字节，到文件地址 0x207 结束。这个目标文件没有 Program Header。文件地址是这样定义的：文件开头第一个字节的地址是 0，然后每个字节占一个地址。

```
...
Section Headers:
  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
  [ 1] .text             PROGBITS        00000000 000034 00002a 00  AX  0   0  4
  [ 2] .rel.text         REL             00000000 0002b0 000010 08      6   1  4
  [ 3] .data             PROGBITS        00000000 000060 000038 00  WA  0   0  4
  [ 4] .bss              NOBITS          00000000 000098 000000 00  WA  0   0  4
  [ 5] .shstrtab         STRTAB          00000000 000098 000030 00      0   0  1
  [ 6] .symtab           SYMTAB          00000000 000208 000080 10      7   7  4
  [ 7] .strtab           STRTAB          00000000 000288 000028 00      0   0  1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings)
  I (info), L (link order), G (group), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)

There are no section groups in this file.

There are no program headers in this file.
...
```

从 Section Header 中读出各 Section 的描述信息，其中`.text`和`.data`是我们在汇编程序中声明的 Section，而其它 Section 是汇编器自动添加的。

`Addr`是这些段加载到内存中的地址（我们讲过程序中的地址都是虚拟地址），加载地址要在链接时填写，现在空缺，所以是全 0。

`Off`和`Size`两列指出了各 Section 的文件地址，比如`.data`段从文件地址 0x60 开始，一共 0x38 个字节，回去翻一下程序，`.data`段定义了 14 个 4 字节的整数，一共是 56 个字节，也就是 0x38。根据以上信息可以描绘出整个目标文件的布局。

### 我做的实验

![[Pasted image 20240120185652.png]]
### ChatGpt
这个节头（Section Header）的表格是从一个 ELF（Executable and Linkable Format）文件中提取的，它描述了文件中各个节（Section）的属性。ELF 是在类 Unix 系统中常用的一种文件格式，用于可执行文件、目标代码、共享库等。下面是对表中每一列的解释：

1. **[Nr]**: 节的序号。这是一个简单的索引，用于区分文件中的不同节。
    
2. **Name**: 节的名称。它是一个标识符，用于指代特定的节。
    
3. **Type**: 节的类型。常见的类型包括 `PROGBITS`（程序代码或数据）、`NOBITS`（在文件中不占空间的节，如未初始化的数据）、`SYMTAB`（符号表）、`STRTAB`（字符串表）、`RELA`（重定位条目）等。
    
4. **Address**: 节在内存中的起始地址。当文件被加载到内存中时，节的内容会被放置到这个地址。
    
5. **Off (Offset)**: 节在文件中的偏移量。这是节的内容在文件中的起始位置。
    
6. **Size**: 节的大小。这是节的内容在内存中所占的字节数。
    
7. **ES (Entry Size)**: 某些节中每个条目的大小，对于不包含固定大小条目的节，此值为0。
    
8. **Flg (Flags)**: 节的标志。常见的标志包括 `W`（可写）、`A`（可分配）、`X`（可执行）。
    
9. **Lk (Link)**: 关联的节索引。此字段的具体意义依赖于节的类型。
    
10. **Inf (Info)**: 额外信息。此字段的具体意义依赖于节的类型。
    
11. **Al (Alignment)**: 节的对齐。这是节的地址必须是多少字节对齐的。
    

例如，从这个表格中，我们可以看到：

- `.text` 节是程序的代码部分（PROGBITS），它是可执行的（标志 `AX`）。
- `.data` 节包含程序的初始化数据（PROGBITS），它是可写的（标志 `WA`）。
- `.bss` 节是程序的未初始化数据部分（NOBITS），它同样是可写的（标志 `WA`）。
- `.symtab` 和 `.strtab` 分别是符号表和字符串表，这些用于存储程序的符号信息和字符串数据。
- `.rela.text` 是重定位信息，用于在程序加载时调整代码和数据引用的地址。

**表 18.1. 目标文件的布局**

<table summary="目标文件的布局"><colgroup><col><col></colgroup><thead><tr><th>起始文件地址</th><th>Section 或 Header</th></tr></thead><tbody><tr><td>0</td><td>ELF Header</td></tr><tr><td>0x34</td><td><code>.text</code></td></tr><tr><td>0x60</td><td><code>.data</code></td></tr><tr><td>0x98</td><td><code>.bss</code>（此段为空）</td></tr><tr><td>0x98</td><td><code>.shstrtab</code></td></tr><tr><td>0xc8</td><td>Section Header Table</td></tr><tr><td>0x208</td><td><code>.symtab</code></td></tr><tr><td>0x288</td><td><code>.strtab</code></td></tr><tr><td>0x2b0</td><td><code>.rel.text</code></td></tr></tbody></table>

这个文件不大，我们直接用`hexdump`工具把目标文件的字节全部打印出来看。

```
$ hexdump -C max.o 
00000000  7f 45 4c 46 01 01 01 00  00 00 00 00 00 00 00 00  |.ELF............|
00000010  01 00 03 00 01 00 00 00  00 00 00 00 00 00 00 00  |................|
00000020  c8 00 00 00 00 00 00 00  34 00 00 00 00 00 28 00  |........4.....(.|
00000030  08 00 05 00 bf 00 00 00  00 8b 04 bd 00 00 00 00  |................|
00000040  89 c3 83 f8 00 74 10 47  8b 04 bd 00 00 00 00 39  |.....t.G.......9|
00000050  d8 7e ef 89 c3 eb eb b8  01 00 00 00 cd 80 00 00  |.~..............|
00000060  03 00 00 00 43 00 00 00  22 00 00 00 de 00 00 00  |....C...".......|
00000070  2d 00 00 00 4b 00 00 00  36 00 00 00 22 00 00 00  |-...K...6..."...|
00000080  2c 00 00 00 21 00 00 00  16 00 00 00 0b 00 00 00  |,...!...........|
00000090  42 00 00 00 00 00 00 00  00 2e 73 79 6d 74 61 62  |B.........symtab|
000000a0  00 2e 73 74 72 74 61 62  00 2e 73 68 73 74 72 74  |..strtab..shstrt|
000000b0  61 62 00 2e 72 65 6c 2e  74 65 78 74 00 2e 64 61  |ab..rel.text..da|
000000c0  74 61 00 2e 62 73 73 00  00 00 00 00 00 00 00 00  |ta..bss.........|
000000d0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
000000f0  1f 00 00 00 01 00 00 00  06 00 00 00 00 00 00 00  |................|
00000100  34 00 00 00 2a 00 00 00  00 00 00 00 00 00 00 00  |4...*...........|
00000110  04 00 00 00 00 00 00 00  1b 00 00 00 09 00 00 00  |................|
00000120  00 00 00 00 00 00 00 00  b0 02 00 00 10 00 00 00  |................|
00000130  06 00 00 00 01 00 00 00  04 00 00 00 08 00 00 00  |................|
00000140  25 00 00 00 01 00 00 00  03 00 00 00 00 00 00 00  |%...............|
00000150  60 00 00 00 38 00 00 00  00 00 00 00 00 00 00 00  |`...8...........|
00000160  04 00 00 00 00 00 00 00  2b 00 00 00 08 00 00 00  |........+.......|
00000170  03 00 00 00 00 00 00 00  98 00 00 00 00 00 00 00  |................|
00000180  00 00 00 00 00 00 00 00  04 00 00 00 00 00 00 00  |................|
00000190  11 00 00 00 03 00 00 00  00 00 00 00 00 00 00 00  |................|
000001a0  98 00 00 00 30 00 00 00  00 00 00 00 00 00 00 00  |....0...........|
000001b0  01 00 00 00 00 00 00 00  01 00 00 00 02 00 00 00  |................|
000001c0  00 00 00 00 00 00 00 00  08 02 00 00 80 00 00 00  |................|
000001d0  07 00 00 00 07 00 00 00  04 00 00 00 10 00 00 00  |................|
000001e0  09 00 00 00 03 00 00 00  00 00 00 00 00 00 00 00  |................|
000001f0  88 02 00 00 28 00 00 00  00 00 00 00 00 00 00 00  |....(...........|
00000200  01 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000210  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000220  00 00 00 00 03 00 01 00  00 00 00 00 00 00 00 00  |................|
00000230  00 00 00 00 03 00 03 00  00 00 00 00 00 00 00 00  |................|
00000240  00 00 00 00 03 00 04 00  01 00 00 00 00 00 00 00  |................|
00000250  00 00 00 00 00 00 03 00  0c 00 00 00 0e 00 00 00  |................|
00000260  00 00 00 00 00 00 01 00  17 00 00 00 23 00 00 00  |............#...|
00000270  00 00 00 00 00 00 01 00  21 00 00 00 00 00 00 00  |........!.......|
00000280  00 00 00 00 10 00 01 00  00 64 61 74 61 5f 69 74  |.........data_it|
00000290  65 6d 73 00 73 74 61 72  74 5f 6c 6f 6f 70 00 6c  |ems.start_loop.l|
000002a0  6f 6f 70 5f 65 78 69 74  00 5f 73 74 61 72 74 00  |oop_exit._start.|
000002b0  08 00 00 00 01 02 00 00  17 00 00 00 01 02 00 00  |................|
000002c0
```

左边一列是文件地址，中间是每个字节的十六进制表示，右边是把这些字节解释成 ASCII 码所对应的字符。中间有一个 * 号表示省略的部分全是 0。`.data`段对应的是这一块：

```
...
00000060  03 00 00 00 43 00 00 00  22 00 00 00 de 00 00 00  |....C...".......|
00000070  2d 00 00 00 4b 00 00 00  36 00 00 00 22 00 00 00  |-...K...6..."...|
00000080  2c 00 00 00 21 00 00 00  16 00 00 00 0b 00 00 00  |,...!...........|
00000090  42 00 00 00 00 00 00 00
...
```

`.data`段将被原封不动地加载到内存中，下一小节会看到`.data`段被加载到内存地址 0x080490a0~0x080490d7。

`.shstrtab`和`.strtab`这两个 Section 中存放的都是 ASCII 码：

```
...
                                   00 2e 73 79 6d 74 61 62  |B.........symtab|
000000a0  00 2e 73 74 72 74 61 62  00 2e 73 68 73 74 72 74  |..strtab..shstrt|
000000b0  61 62 00 2e 72 65 6c 2e  74 65 78 74 00 2e 64 61  |ab..rel.text..da|
000000c0  74 61 00 2e 62 73 73 00                           |ta..bss.........|
...
                                   00 64 61 74 61 5f 69 74  |.........data_it|
00000290  65 6d 73 00 73 74 61 72  74 5f 6c 6f 6f 70 00 6c  |ems.start_loop.l|
000002a0  6f 6f 70 5f 65 78 69 74  00 5f 73 74 61 72 74 00  |oop_exit._start.|
...
```

可见`.shstrtab`段保存着各 Section 的名字，`.strtab`段保存着程序中用到的符号的名字。每个名字都是以`'\0'`结尾的字符串。

我们知道，C 语言的全局变量如果在代码中没有初始化，就会在程序加载时用 0 初始化。这种数据属于`.bss`段，在加载时它和`.data`段一样都是可读可写的数据，但是在 ELF 文件中`.data`段需要占用一部分空间保存初始值，而`.bss`段则不需要。也就是说，`.bss`段在文件中只占一个 Section Header 而没有对应的 Section，程序加载时`.bss`段占多大内存空间在 Section Header 中描述。在我们这个例子中没有用到`.bss`段，在[第 3 节 “变量的存储布局”](https://akaedu.github.io/book/ch19s03.html#asmc.layout) 会看到这样的例子。

我们继续分析`readelf`输出的最后一部分，是从`.rel.text`和`.symtab`这两个 Section 中读出的信息。

```
...
Relocation section '.rel.text' at offset 0x2b0 contains 2 entries:
 Offset     Info    Type            Sym.Value  Sym. Name
00000008  00000201 R_386_32          00000000   .data
00000017  00000201 R_386_32          00000000   .data

There are no unwind sections in this file.

Symbol table '.symtab' contains 8 entries:
   Num:    Value  Size Type    Bind   Vis      Ndx Name
     0: 00000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 00000000     0 SECTION LOCAL  DEFAULT    1 
     2: 00000000     0 SECTION LOCAL  DEFAULT    3 
     3: 00000000     0 SECTION LOCAL  DEFAULT    4 
     4: 00000000     0 NOTYPE  LOCAL  DEFAULT    3 data_items
     5: 0000000e     0 NOTYPE  LOCAL  DEFAULT    1 start_loop
     6: 00000023     0 NOTYPE  LOCAL  DEFAULT    1 loop_exit
     7: 00000000     0 NOTYPE  GLOBAL DEFAULT    1 _start

No version information found in this file.
```

`.rel.text`告诉链接器指令中的哪些地方需要做重定位，在下一小节详细讨论。

`.symtab`是符号表。`Ndx`列是每个符号所在的 Section 编号，例如符号`data_items`在第 3 个 Section 里（也就是`.data`段），各 Section 的编号见 Section Header Table。`Value`列是每个符号所代表的地址，在目标文件中，符号地址都是相对于该符号所在 Section 的相对地址，比如`data_items`位于`.data`段的开头，所以地址是 0，`_start`位于`.text`段的开头，所以地址也是 0，但是`start_loop`和`loop_exit`相对于`.text`段的地址就不是 0 了。从`Bind`这一列可以看出`_start`这个符号是`GLOBAL`的，而其它符号是`LOCAL`的，`GLOBAL`符号是在汇编程序中用`.globl`指示声明过的符号。

现在剩下`.text`段没有分析，`objdump`工具可以把程序中的机器指令反汇编（Disassemble），那么反汇编的结果是否跟原来写的汇编代码一模一样呢？我们对比分析一下。

```
$ objdump -d max.o

max.o:     file format elf32-i386


Disassembly of section .text:

00000000 <_start>:
   0:	bf 00 00 00 00       	mov    $0x0,%edi
   5:	8b 04 bd 00 00 00 00 	mov    0x0(,%edi,4),%eax
   c:	89 c3                	mov    %eax,%ebx

0000000e <start_loop>:
   e:	83 f8 00             	cmp    $0x0,%eax
  11:	74 10                	je     23 <loop_exit>
  13:	47                   	inc    %edi
  14:	8b 04 bd 00 00 00 00 	mov    0x0(,%edi,4),%eax
  1b:	39 d8                	cmp    %ebx,%eax
  1d:	7e ef                	jle    e <start_loop>
  1f:	89 c3                	mov    %eax,%ebx
  21:	eb eb                	jmp    e <start_loop>

00000023 <loop_exit>:
  23:	b8 01 00 00 00       	mov    $0x1,%eax
  28:	cd 80                	int    $0x80
```

左边是机器指令的字节，右边是反汇编结果。显然，所有的符号都被替换成地址了，比如`je 23`，注意没有加`$`的数表示内存地址，而不表示立即数。这条指令后面的`<loop_exit>`并不是指令的一部分，而是反汇编器从`.symtab`和`.strtab`中查到的符号名称，写在后面是为了有更好的可读性。目前所有指令中用到的符号地址都是相对地址，下一步链接器要修改这些指令，把其中的地址都改成加载时的内存地址，这些指令才能正确执行。

现在我们按上一节的步骤分析可执行文件`max`，看看链接器都做了什么改动。

```
$ readelf -a max
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           Intel 80386
  Version:                           0x1
  Entry point address:               0x8048074
  Start of program headers:          52 (bytes into file)
  Start of section headers:          256 (bytes into file)
  Flags:                             0x0
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         2
  Size of section headers:           40 (bytes)
  Number of section headers:         6
  Section header string table index: 3

Section Headers:
  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
  [ 1] .text             PROGBITS        08048074 000074 00002a 00  AX  0   0  4
  [ 2] .data             PROGBITS        080490a0 0000a0 000038 00  WA  0   0  4
  [ 3] .shstrtab         STRTAB          00000000 0000d8 000027 00      0   0  1
  [ 4] .symtab           SYMTAB          00000000 0001f0 0000a0 10      5   6  4
  [ 5] .strtab           STRTAB          00000000 000290 000040 00      0   0  1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings)
  I (info), L (link order), G (group), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)

There are no section groups in this file.

Program Headers:
  Type           Offset   VirtAddr   PhysAddr   FileSiz MemSiz  Flg Align
  LOAD           0x000000 0x08048000 0x08048000 0x0009e 0x0009e R E 0x1000
  LOAD           0x0000a0 0x080490a0 0x080490a0 0x00038 0x00038 RW  0x1000

 Section to Segment mapping:
  Segment Sections...
   00     .text 
   01     .data 

There is no dynamic section in this file.

There are no relocations in this file.

There are no unwind sections in this file.

Symbol table '.symtab' contains 10 entries:
   Num:    Value  Size Type    Bind   Vis      Ndx Name
     0: 00000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 08048074     0 SECTION LOCAL  DEFAULT    1 
     2: 080490a0     0 SECTION LOCAL  DEFAULT    2 
     3: 080490a0     0 NOTYPE  LOCAL  DEFAULT    2 data_items
     4: 08048082     0 NOTYPE  LOCAL  DEFAULT    1 start_loop
     5: 08048097     0 NOTYPE  LOCAL  DEFAULT    1 loop_exit
     6: 08048074     0 NOTYPE  GLOBAL DEFAULT    1 _start
     7: 080490d8     0 NOTYPE  GLOBAL DEFAULT  ABS __bss_start
     8: 080490d8     0 NOTYPE  GLOBAL DEFAULT  ABS _edata
     9: 080490d8     0 NOTYPE  GLOBAL DEFAULT  ABS _end

No version information found in this file.
```

在 ELF Header 中，`Type`改成了`EXEC`，由目标文件变成可执行文件了，`Entry point address`改成了 0x8048074（这是`_start`符号的地址），还可以看出，多了两个 Program Header，少了两个 Section Header。

在 Section Header Table 中，`.text`和`.data`段的加载地址分别改成了 0x08048074 和 0x080490a0。`.bss`段没有用到，所以被删掉了。`.rel.text`段就是用于链接过程的，做完链接就没用了，所以也删掉了。

多出来的 Program Header Table 描述了两个 Segment 的信息。`.text`段和前面的 ELF Header、Program Header Table 一起组成一个 Segment（`FileSiz`指出总长度是 0x9e），`.data`段组成另一个 Segment（总长度是 0x38）。`VirtAddr`列指出第一个 Segment 加载到虚拟地址 0x08048000（注意在 x86 平台上后面的`PhysAddr`列是没有意义的，并不代表实际的物理地址），第二个 Segment 加载到地址 0x080490a0。`Flg`列指出第一个 Segment 的访问权限是可读可执行，第二个 Segment 的访问权限是可读可写。最后一列`Align`的值 0x1000（4K）是 x86 平台的内存页面大小。在加载时文件也要按内存页面大小分成若干页，文件中的一页对应内存中的一页，对应关系如下图所示。

**图 18.2. 文件和加载地址的对应关系**

![](https://akaedu.github.io/book/images/asm.load.png)

这个可执行文件很小，总共也不超过一页大小，但是两个 Segment 必须加载到内存中两个不同的页面，因为 MMU 的权限保护机制是以页为单位的，一个页面只能设置一种权限。此外还规定每个 Segment 在文件页面内偏移多少加载到内存页面仍然要偏移多少，比如第二个 Segment 在文件中的偏移是 0xa0，在内存页面 0x08049000 中的偏移仍然是 0xa0，所以从 0x080490a0 开始，这样规定是为了简化链接器和加载器的实现。从上图也可以看出`.text`段的加载地址应该是`0x08048074`，`_start`符号位于`.text`段的开头，所以`_start`符号的地址也是 0x08048074，从符号表中可以验证这一点。

原来目标文件符号表中的`Value`都是相对地址，现在都改成绝对地址了。此外还多了三个符号`__bss_start`、`_edata`和`_end`，这些符号在链接脚本中定义，被链接器添加到可执行文件中，链接脚本在[第 1 节 “多目标文件的链接”](https://akaedu.github.io/book/ch20s01.html#link.basic) 介绍。

再看一下反汇编的结果：

```
$ objdump -d max

max:     file format elf32-i386


Disassembly of section .text:

08048074 <_start>:
 8048074:	bf 00 00 00 00       	mov    $0x0,%edi
 8048079:	8b 04 bd a0 90 04 08 	mov    0x80490a0(,%edi,4),%eax
 8048080:	89 c3                	mov    %eax,%ebx

08048082 <start_loop>:
 8048082:	83 f8 00             	cmp    $0x0,%eax
 8048085:	74 10                	je     8048097 <loop_exit>
 8048087:	47                   	inc    %edi
 8048088:	8b 04 bd a0 90 04 08 	mov    0x80490a0(,%edi,4),%eax
 804808f:	39 d8                	cmp    %ebx,%eax
 8048091:	7e ef                	jle    8048082 <start_loop>
 8048093:	89 c3                	mov    %eax,%ebx
 8048095:	eb eb                	jmp    8048082 <start_loop>

08048097 <loop_exit>:
 8048097:	b8 01 00 00 00       	mov    $0x1,%eax
 804809c:	cd 80                	int    $0x80
```

指令中的相对地址都改成绝对地址了。我们仔细检查一下改了哪些地方。首先看跳转指令，原来目标文件的指令是这样：

```
...
  11:	74 10                	je     23 <loop_exit>
...
  1d:	7e ef                	jle    e <start_loop>
...
  21:	eb eb                	jmp    e <start_loop>
...
```

现在改成了这样：

```
...
 8048085:	74 10                	je     8048097 <loop_exit>
...
 8048091:	7e ef                	jle    8048082 <start_loop>
...
 8048095:	eb eb                	jmp    8048082 <start_loop>
...
```

改了吗？其实只是反汇编的结果不同了，指令的机器码根本没变。为什么不用改指令就能跳转到新的地址呢？因为跳转指令中指定的是相对于当前指令向前或向后跳多少字节，而不是指定一个完整的内存地址，内存地址有 32 位，这些跳转指令只有 16 位，显然也不可能指定一个完整的内存地址，这称为相对跳转。这种相对跳转指令只有 16 位，只能在当前指令前后的一个小范围内跳转，不可能跳得太远，也有的跳转指令指定一个完整的内存地址，可以跳到任何地方，这称绝对跳转，在[第 4.2 节 “动态链接的过程”](https://akaedu.github.io/book/ch20s04.html#link.dlprocess) 我们会看到这样的例子。

再看内存访问指令，原来目标文件的指令是这样：

```
...
   5:	8b 04 bd 00 00 00 00 	mov    0x0(,%edi,4),%eax
...
  14:	8b 04 bd 00 00 00 00 	mov    0x0(,%edi,4),%eax
...
```

现在改成了这样：

```
...
 8048079:	8b 04 bd a0 90 04 08 	mov    0x80490a0(,%edi,4),%eax
...
 8048088:	8b 04 bd a0 90 04 08 	mov    0x80490a0(,%edi,4),%eax
...
```

指令中的地址原本是 0x00000000，现在改成了 0x080409a0（注意是小端字节序）。那么链接器怎么知道要改这两处呢？是根据目标文件中的`.rel.text`段提供的重定位信息来改的：

```
...
Relocation section '.rel.text' at offset 0x2b0 contains 2 entries:
 Offset     Info    Type            Sym.Value  Sym. Name
00000008  00000201 R_386_32          00000000   .data
00000017  00000201 R_386_32          00000000   .data
...
```

第一列`Offset`的值就是`.text`段需要改的地方，在`.text`段中的相对地址是 8 和 0x17，正是这两条指令中 00 00 00 00 的位置。