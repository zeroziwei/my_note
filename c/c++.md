---
tags: 
created_date: 2024-04-14 18:37
modified_date: ' 2024-04-14 18:37:36'
review_date: 
pomodoro_time: 
used_time: 
aliases:
---
## 资源

[[陈明][南京大学][23 春][面向对象编程基础][泛型算法]_哔哩哔哩_bilibili]( https://www.bilibili.com/video/BV1Hh4y1H7gv/?spm_id_from=333.999.0.0&vd_source=ee35ef6ed26be3a27128aa2dd21b20d8 )

[南京大学 - 陈明 - 《面向对象编程基础》](https://developer.aliyun.com/adc/series/university-nanjing)

[《面向对象编程基础》 课程主页2023](https://cpp.njuer.org/)

[[clang]]

[玩转3个月免费试用攻略-阿里云帮助中心](https://help.aliyun.com/zh/ecs/3-month-free-trial)

[Cpp_Primer_Practice/notes/ch03.md at master · applenob/Cpp_Primer_Practice](https://github.com/applenob/Cpp_Primer_Practice/blob/master/notes/ch03.md)

- [ ] 迭代器 📅 2024-04-19 
## 工具 

[C++ Insights](https://cppinsights.io/) Insights yyds !!!


## 编译和链接

  [c++ - Locating iostream in Clang++: fatal error: 'iostream' file not found - Stack Overflow](https://stackoverflow.com/questions/54521402/locating-iostream-in-clang-fatal-error-iostream-file-not-found?newreg=d55a5d41d936435086e86f6d8b88e0fe)
  
	clang++ -I/usr/include/c++/11 -I/usr/include/x86_64-linux-gnu/c++/11 -L /usr/lib/gcc/x86_64-linux-gnu/11 main.cpp -o main 


这是一个在命令行使用 `clang++` 编译器编译 C++ 程序的命令。让我们一部分一部分地解读这个命令：

- `clang++`: 这是 `clang` 的 C++ 编译器前端的命令。`clang` 是一个编译器，它属于 LLVM 项目，并且通常被认为生成高质量的错误信息和优化后的代码。

- `-I/usr/include/c++/11`: 这个选项告诉 `clang++` 在 `/usr/include/c++/11` 目录下查找包括（include）的头文件。这个路径可能包含了 C++ 标准库的头文件，特别是那些属于 C++11 标准的。

- `-I/usr/include/x86_64-linux-gnu/c++/11`: 这个选项也是指示编译器去另一个特定的目录下查找包括的头文件。这个目录通常包含了为特定架构（在这个例子中是 x86_64）或者操作系统特性定制的头文件。

- `-L /usr/lib/gcc/x86_64-linux-gnu/11`: 这个选项告诉编译器在链接时在 `/usr/lib/gcc/x86_64-linux-gnu/11` 目录下查找库文件。这是 GCC 编译器库文件的标准位置，`clang` 也可以使用这些库。

- `main.cpp`: 这是要编译的源代码文件。

- `-o main`: 这个选项指定了输出的可执行文件的名称。在这个例子中，输出的文件将被命名为 `main`。

整个命令的作用是使用 `clang++` 编译器，带有特定的头文件和库文件路径，编译 `main.cpp` 源文件，最后生成名为 `main` 的可执行文件。

需要注意的是，这个命令假设 `/usr/include/c++/11` 和 `/usr/include/x86_64-linux-gnu/c++/11` 包含了所有必要的 C++ 标准库头文件，并且 `/usr/lib/gcc/x86_64-linux-gnu/11` 包含了所有必要的库文件。如果这些路径中的文件不完整或者版本不对，编译过程可能会失败。此外，在编译较大的项目时，通常还会涉及其他编译选项和链接到其他库的操作。

	export CPLUS_INCLUDE_PATH=/usr/include/c++/11:/usr/include/x86_64-linux-gnu/c++/11
	
