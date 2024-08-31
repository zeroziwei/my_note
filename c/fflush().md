---
tags: 
created_date: 2024-05-11 03:48
modified_date: ' 2024-05-11 03:48:06'
review_date: 
pomodoro_time: 
used_time: 
aliases:
---

`fflush` 是 C 语言标准库中的一个函数，用于刷新流的缓冲区，确保数据被立即写入到文件中。它的原型如下：

`int fflush(FILE *stream);`


### 参数说明

- `stream`：指向要刷新缓冲区的文件流的指针。

### 功能

`fflush` 函数的作用是将流的缓冲区中的数据立即写入到文件中。在默认情况下，标准输出流（`stdout`）通常是行缓冲的，即当遇到换行符时才会将缓冲区中的数据写入到文件中；而标准错误输出流（`stderr`）通常是不带缓冲的，即每次输出都会立即写入到文件中。但对于其他文件流，如打开的文件流，默认情况下是全缓冲的，即缓冲区满了或者调用 `fclose` 时才会将数据写入到文件中。使用 `fflush` 可以强制将缓冲区中的数据立即写入到文件中。

### 示例

下面是一个简单的示例，演示了如何使用 `fflush` 函数：

``` c
#include <stdio.h>

int main() {
  FILE *fp = fopen("output.txt", "w");
  if (fp == NULL) {
    perror("Error opening file");
    return 1;
  }

  fprintf(fp, "Hello, World!\n");
  fflush(fp); // 刷新缓冲区，确保数据被写入到文件中

  fclose(fp);
  return 0;
}****
```


在这个示例中，程序打开一个名为 `output.txt` 的文件，并使用 `fprintf` 将数据写入到文件中。然后调用 `fflush` 函数来确保数据被立即写入到文件中。最后，关闭文件流。

### 总结

`fflush` 函数是一个重要的函数，用于确保流的缓冲区中的数据被立即写入到文件中。在需要立即将数据写入到文件中的情况下，可以使用 `fflush` 函数来实现这一目的