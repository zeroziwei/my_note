---
tags: 
created_date: 2024-01-16 00:52
modified_date: ' 2024-01-16 00:52:57'
review_date: 
pomodoro_time: 
used_time: 
aliases:
---
=## 调试的基本思想

调试的基本思想是 “**分析现象 -> 假设错误原因 -> 产生新的现象去验证假设**” 这样一个循环，根据现象如何假设错误原因，以及如何设计新的现象去验证假设，这都需要非常严密的分析和思考，如果因为手里有了强大的工具就滥用而忽略了分析过程，往往会治标不治本地修正 Bug，导致一个错误现象消失了但 Bug 仍然存在，甚至是把程序越改越错。

## 什么是GDB

`gdb`，可以完全操控程序的运行，使得程序就像你手里的玩具一样，叫它走就走，叫它停就停，并且随时可以查看程序中所有的内部状态，比如各变量的值、传给函数的参数、当前执行的代码行等。
[[1_ 单步执行和跟踪函数调用]]
[[36-1_ 单步执行和跟踪函数调用@annote]]
## Gdb 常用命令 
![[Pasted image 20240331155508.png|800]]

![[Pasted image 20240331155640.png|800]]
![[Pasted image 20240331155448.png]]
## GDB 传递命令行参数 

GNU Debugger (GDB) 是一个功能强大的调试工具，用于调试 C 和 C++程序。如果你想要在 GDB 中给程序传递命令行参数，你可以在启动 GDB 时在命令行中指定程序名后面跟着参数，或者在 GDB 内部使用 `set args` 命令设置参数。

以下是两种设置命令行参数的方法：

### 方法一：在启动GDB时传递参数

你可以在启动GDB时就指定参数。例如，如果你的程序叫做`myprogram`并且你想传递参数`arg1` `arg2`，你可以这样启动GDB：

```bash
gdb --args myprogram arg1 arg2
```

这样做的话，当你在GDB中运行`run`命令时，`myprogram`就会接收到`arg1`和`arg2`作为命令行参数。

### 方法二：在GDB中使用 `set args` 命令

如果你已经在GDB中，你可以使用`set args`命令来设置你的程序需要的参数。下面是使用这个命令的步骤：

1. 启动GDB。

    ```bash
    gdb myprogram
    ```

2. 设置命令行参数。

    ```gdb
    (gdb) set args arg1 arg2
    ```

3. 启动你的程序。

    ```gdb
    (gdb) run
    ```

在这个例子中，当你执行`run`命令时，`myprogram`会接收`arg1`和`arg2`作为参数。

记住，如果你需要包含空格的参数，你应该用引号将该参数括起来。例如：

```gdb
(gdb) set args "arg with spaces" arg2
```

在调试完毕后，如果你想清除已设置的参数，可以不带任何参数地运行`set args`命令：

```gdb
(gdb) set args
```

这将清除以前设置的所有命令行参数。

## 汇编层次调试代码

在 GDB (GNU Debugger) 中进行汇编层面的调试意味着你将会操作和观察机器层面的指令和寄存器，而不只是高级语言源码和变量。以下是一些基本的操作，用来在汇编层次上调试代码：

1. **启动GDB**:
   在命令行中，使用 `gdb` 启动你的程序：
   ```sh
   gdb ./your_program
   ```

2. **设置断点**:
   你可以在函数的入口处设置断点，或者在具体的汇编指令地址处设置断点:
   ```gdb
   (gdb) break *0x00400510
   ```
   或者使用函数名：
   ```gdb
   (gdb) break my_function
   ```

3. **启动程序**:
   使用 `run` 命令来启动程序:
   ```gdb
   (gdb) run
   ```

4. **查看汇编代码**:
   使用 `disassemble` 命令来查看当前函数的汇编指令，或者使用 `disas /m` 显示混合源码和汇编：
   ```gdb
   (gdb) disassemble /m
   ```
   或者只查看当前指令：
   ```gdb
   (gdb) x/i $pc
   ```

5. **单步执行**:
   使用 `stepi` 或 `si` 命令来执行下一条汇编指令:
   ```gdb
   (gdb) stepi
   ```

6. **查看和设置寄存器**:
   使用 `info registers` 查看所有寄存器状态，或者查看特定寄存器（如 `rax`）:
   ```gdb
   (gdb) info registers rax
   ```
   你也可以设置寄存器的值:
   ```gdb
   (gdb) set $rax = 0x1234
   ```

7. **访问内存**:
   使用 `x` 命令来查看内存的内容。例如，查看指针 `ptr` 所指向的内存:
   ```gdb
   (gdb) x/gx ptr
   ```
   其中，`g` 表示8字节大小，`x` 表示十六进制格式。你可以根据需要更改这些选项。

8. **继续执行**:
   当你完成单步执行，想要继续运行程序直到下一个断点或程序结束，可以使用 `continue` 或 `c` 命令:
   ```gdb
   (gdb) continue
   ```

9. **退出GDB**:
   使用 `quit` 或者 `q` 命令退出GDB:
   ```gdb
   (gdb) quit
   ```

这些命令提供了在汇编层次调试程序的基本流程。当然，GDB 提供了更多复杂和强大的功能，例如设置条件断点、观察点、修改内存、调用堆栈跟踪等。通过详细的 GDB 文档或者在线教程，你可以进一步学习如何更有效地使用 GDB。


## TUI 

GDB 的 TUI（Text User Interface）模式提供了几种不同的视图来帮助用户更有效地进行调试。以下是 TUI 模式下可用的主要视图类型：

1. **源代码视图 (Src)**:
   - 使用 `layout src` 激活。
   - 显示当前正在调试的源文件。
   - 高亮显示当前执行的代码行。

2. **汇编视图 (Asm)**:
   - 使用 `layout asm` 激活。
   - 显示当前函数的汇编代码。
   - 当前执行的汇编指令会被高亮显示。

3. **寄存器视图 (Regs)**:
   - 使用 `layout regs` 激活。
   - 展示当前的寄存器值。
   - 特别有用于查看程序的低级执行状态。

4. **混合视图 (Split)**:
   - 使用 `layout split` 激活。
   - 同时显示源代码和汇编指令。
   - 允许用户看到高级语言代码和对应的汇编代码。

除了上述视图外，TUI模式还允许用户通过键盘快捷键或命令来控制和自定义视图，例如调整视图的大小或在不同视图间切换。使用这些视图，开发者可以更直观地理解程序的执行流程和状态，特别是在进行底层调试时。

**附加控制**:
- **切换视图**: 使用 `layout next` 和 `layout prev` 可以在不同视图间切换。
- **滚动**: 在某些视图中，可以使用方向键或 `Ctrl` + `P`（向上滚动）和 `Ctrl` + `N`（向下滚动）来滚动内容。
- **退出TUI模式**: 可以使用 `tui disable` 命令或 `layout none` 回到标准的GDB命令行界面。

TUI 模式是一个强大的功能，使得在没有图形界面的环境下，调试变得更为直观和高效。


## X 命令 

GDB（GNU Debugger）中的 `x` 命令是一种非常强大的内存检查工具，它用于查看程序运行时存储在内存中的数据。这个命令的基本语法是 `x/NFU ADDR`，其中各部分的含义如下：

1. **N**（可选）：表示要检查的内存单位数量。如果省略，则默认为 1。

2. **F**（可选）：表示显示格式。GDB 提供了多种格式，如十六进制（`x`）、十进制（`d`）、无符号十进制（`u`）、字符（`c`）、浮点数（`f`）等。如果省略，则使用上次的格式或默认格式。

3. **U**（可选）：表示单位大小。常用的单位有字节（`b`）、半字（`h`，两个字节）、字（`w`，四个字节）和双字（`g`，八个字节）。如果省略，则使用上次的单位或默认单位。

4. **ADDR**：表示要检查的内存地址。可以是一个具体的数值，也可以是一个表达式或寄存器的名称。

例如，命令 `x/3xw $ebp` 将以十六进制格式（`x`）显示从 `ebp` 寄存器指向的地址开始的三个字（`3w`）的内容。

`x` 命令在调试程序时非常有用，尤其是当你需要检查堆栈、寄存器或特定内存区域的内容时。它能帮助你理解程序的内存布局和数据流动，从而对程序行为有更深入的理解。


## 反向调试 

在 GDB 中实现代码的回退功能，即“逆向调试”，可以让你反向执行程序来检查之前的状态。为了使用这个功能，你需要一个支持逆向调试的 GDB 版本。下面是如何启用和使用逆向调试的步骤：

1. **检查GDB支持**: 首先，确保你的GDB版本支持逆向调试。你可以通过运行命令 `show record` 来检查。

2. **启用记录功能**: 在开始调试前，需要开启记录功能。这可以通过运行命令 `record` 来实现。一旦启用了记录，GDB会记录程序执行的步骤，允许之后的回退。

3. **正向执行**: 正常使用GDB命令（如 `next`, `step`, `continue` 等）来正向执行你的程序。

4. **逆向执行**: 当你需要回退时，可以使用逆向执行的命令。主要的逆向命令有：
   - `reverse-step` 或 `rs`: 类似于 `step` 命令，但是向后移动一步。
   - `reverse-next` 或 `rn`: 类似于 `next` 命令，但是向后移动一步。
   - `reverse-continue` 或 `rc`: 类似于 `continue` 命令，但是向后继续执行，直到遇到上一个断点。

5. **监控程序状态**: 在逆向调试过程中，你可以使用诸如 `print`、`info locals` 等命令来查看变量的状态，这些命令在逆向调试中同样有效。

需要注意的是，逆向调试可能会消耗大量的内存和处理资源，因为 GDB 需要记录程序的每一步操作。另外，并非所有的系统和平台都支持 GDB 的逆向调试功能。如果你的系统不支持，可能需要查找特定于平台的解决方案。

### 反向调试可能遇到的问题 

当你在 GDB 中使用 `record` 命令进行逆向调试，并且在运行到 `scanf` 或其他系统调用时遇到错误消息“Process record and replay doesn't support syscall number 318”，这是因为 GDB 的记录回放功能不支持某些类型的系统调用。

### 问题的根源

GDB的记录/回放功能旨在记录程序的执行过程，以便于之后可以进行逆向执行。这项技术主要适用于纯计算型的指令。然而，当程序执行到涉及系统资源、外部输入输出等操作的系统调用时，GDB的记录/回放机制可能无法正确处理，因为这些操作的结果很难在回放时准确重现。

### 解决方案和建议

1. **避免在记录状态下执行系统调用**：如果可能，避免在使用`record`功能时执行涉及系统调用的代码段。你可以尝试将对`scanf`的调用移至不需要记录的程序部分，或者使用`record`命令之后直接跳过这些部分。

2. **使用条件断点绕过问题**：在达到`scanf`之前设置一个断点，并且在该断点处停止记录（使用`record stop`命令）。这样可以绕过`scanf`调用的记录。当然，这意味着你无法逆向穿越这些被跳过的部分。

3. **考虑替代的输入方法**：对于测试目的，你可以考虑使用替代`scanf`的方式来提供输入，比如从文件读取，这可能避免了直接的系统调用问题。

4. **更新GDB版本**：确保你的GDB版本是最新的。虽然这不一定能解决所有系统调用的兼容性问题，但较新的版本可能包含了对更多系统调用的支持或改进的记录/回放机制。

5. **寻找其他调试工具**：如果GDB的这一限制对你的工作影响很大，可能需要考虑使用其他支持逆向调试并且对系统调用有更好支持的调试工具，比如RR (Mozilla的Record and Replay Framework)。

总之，虽然 GDB 的 `record` 和逆向调试功能是一个强大的工具，但它在处理系统调用时有其局限性。在实践中，需要根据具体情况调整调试策略。

### 使用条件代码跳过问题 

使用条件断点绕过问题的基本思想是，在执行到会引起问题的系统调用之前设置一个断点，并在到达该断点时停止记录。这样，你就可以避免记录到导致错误的系统调用。下面是如何操作的具体步骤：

1. **设置断点**：首先，确定你想要绕过的`scanf`调用的具体位置（假设是在文件`example.c`的第42行）。在该行之前的一个适当位置设置一个断点。你可以使用GDB的`break`命令来设置断点，例如：

   ```gdb
   break example.c:41
   ```

   这个命令在`example.c`文件的第41行设置了一个断点，即`scanf`调用之前的行。

2. **启用记录**：然后，正常启动你的程序，并启用GDB的记录功能：

   ```gdb
   start
   record
   ```

3. **运行到断点**：让程序运行，直到它达到你设置的断点。你可以使用`continue`命令来实现这一点。

4. **停止记录**：一旦程序停在了断点上，你就可以停止记录了。这样，`scanf`调用就不会被记录。使用命令`record stop`来停止记录。

   ```gdb
   record stop
   ```

5. **绕过问题代码**：现在，你可以手动绕过会引起问题的`scanf`调用。如果你仅仅是想跳过这个调用，可以使用`next`命令来执行下一行代码（假设`scanf`不是在循环或复杂条件语句中）。如果`scanf`在执行过程中需要输入，确保在GDB暂停时提供必要的输入。

6. **恢复记录（如果需要）**：如果你希望在绕过问题代码后继续记录程序的执行，可以在合适的时候重新启用记录功能：

   ```gdb
   record
   ```

通过这种方式，你可以在特定的代码段避免遇到系统调用兼容性问题，同时依然能够利用 GDB 的记录和逆向调试功能。需要注意的是，这种方法可能会改变程序的执行流程，特别是在依赖外部输入的情况下，因此需要仔细考虑如何使用。


## CGDB 

分割条上面是 Source 窗口，按 Esc 键进入 Source 窗口；分割条下面是 GDB 窗口，按“i”进入 GDB 窗口。

在 Source 窗口中，可以按上下键逐行查看，或者 page up/down 翻页查看，当前光标所在行会有一个 block 进行提示，


调整这 2 个窗口的占比，首先按下 esc，然后使用“+”和“-”来调整。



[Linux下CGDB使用教程_cgdb教程-CSDN博客](https://blog.csdn.net/whahu1989/article/details/115234011)

