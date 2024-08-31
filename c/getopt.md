---
tags: 
created_date: 2024-05-11 00:24
modified_date: ' 2024-05-11 00:24:59'
review_date: 
pomodoro_time: 
used_time: 
aliases:
---
![[getopt-20240511004449872.webp]]


**optstring** 的格式举例说明比较方便，例如：

    char *optstring = "abcd:";

上面这个 optstring 在传入之后，getopt 函数将依次检查命令行是否指定了 -a， -b， -c 及 -d（这需要多次调用 getopt 函数，直到其返回 - 1），当检查到上面某一个参数被指定时，函数会返回被指定的参数名称（即该字母）

最后一个参数 d 后面带有冒号，: 表示参数 d 是可以指定值的，如 -d 100 或 -d user。

**optind** 表示的是下一个将被处理到的参数在 argv 中的下标值。

如果指定 **opterr** = 0 的话，在 getopt、getopt_long、getopt_long_only 遇到错误将不会输出错误信息到标准输出流。

**optarg** 有指定值的参数的指定值 


[[getopt 和 getopt_long 函数 - CSDN 博客]]