---
tags: 
created_date: 2024-05-11 03:38
modified_date: ' 2024-05-11 03:38:56'
review_date: 
pomodoro_time: 
used_time: 
aliases:
---
`fprintf` 是 C 语言标准库中的一个函数，用于将格式化的数据输出到指定的文件流中。它的原型如下：

`int fprintf(FILE *stream, const char *format, ...);`

### 参数说明

- `stream`：指向要写入的文件流的指针，可以是 `stdout`（标准输出）、`stderr`（标准错误输出）、已打开的文件指针等。
- `format`：格式化字符串，包含了要输出的文本以及格式化说明符，类似于 `printf` 函数中的格式化字符串。
- `...`：可变参数列表，用于填充格式化字符串中的格式化说明符。

### 功能

`fprintf` 函数的作用是将格式化的数据按照指定的格式写入到指定的文件流中。它与 `printf` 函数类似，但 `printf` 将输出内容发送到标准输出流（屏幕），而 `fprintf` 可以将输出内容发送到指定的文件流中。

### 示例

下面是一个简单的示例，演示了如何使用 `fprintf` 将数据写入到文件中：

```c 
#include <stdio.h>

int main() {
  FILE *fp = fopen("output.txt", "w");
  if (fp == NULL) {
    perror("Error opening file");
    return 1;
  }

  int num = 42;
  float pi = 3.14159;
  char str[] = "Hello, World!";

  fprintf(fp, "Integer: %d\n", num);
  fprintf(fp, "Float: %f\n", pi);
  fprintf(fp, "String: %s\n", str);

  fclose(fp);
  return 0;
}
```

在这个示例中，程序打开一个名为 `output.txt` 的文件，并使用 `fprintf` 将整数、浮点数和字符串写入到文件中。最后，关闭文件流。

### 总结

`fprintf` 函数是一个非常有用的函数，可以将格式化的数据输出到文件中，适用于需要将输出保存到文件的情况。通过合理使用 `fprintf` 函数，可以实现将程序的输出保存到文件中，方便后续查看和分析。