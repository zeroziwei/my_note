---
tags: 
created_date: 2024-01-12 19:37
modified_date: ' 2024-01-12 19:37:52'
review_date: 
pomodoro_time: 
used_time: 
aliases:
---

Person: 
- 在教教我strtok() 函数

ChatGPT: 
- `strtok()` 函数是 C 语言标准库中用于分割字符串的函数。它可以用来将字符串分割成一系列的“令牌”（tokens），通常基于一组指定的分隔符。

这里是如何使用 `strtok()` 的基本步骤：
### 函数原型

```c
char *strtok(char *str, const char *delim);
```

 
- `str`：待分割的字符串。在第一次调用时，`str` 应该指向要分割的字符串。在后续的调用中，为了获取更多的令牌，应将 `str` 设置为 `NULL`。 
- `delim`：一个包含所有分隔符的字符串。这些分隔符用来决定如何分割 `str`。
### 使用步骤 
1. **首次调用** ：在首次调用 `strtok()` 时，`str` 应该指向你想要分割的字符串。 
2. **后续调用** ：在后续调用 `strtok()` 时，将 `str` 参数设置为 `NULL`，以继续分割同一个字符串。 
3. **循环分割** ：通常，你会在一个循环中调用 `strtok()`，直到它返回 `NULL`，表示没有更多的令牌可以提取。 
4. **处理令牌** ：在每次调用 `strtok()` 后，处理返回的令牌（如果非 `NULL`）。
### 示例

这是一个简单的示例，展示如何使用 `strtok()` 来分割以逗号为分隔符的字符串：

```c
#include <stdio.h>
#include <string.h>

int main() {
    char str[] = "one,two,three,four";
    char *token;
    char *delim = ",";

    // 获取第一个令牌
    token = strtok(str, delim);

    // 继续获取其他令牌
    while (token != NULL) {
        printf("%s\n", token);
        token = strtok(NULL, delim);
    }

    return 0;
}
```


### 注意事项 
- `strtok()` 在内部使用静态变量来保持当前位置的状态，因此它是不可重入的。这意味着它在多线程环境中可能不安全。 
- `strtok()` 会修改输入字符串，通过在每个分隔符处插入 `NULL` 字符来分割令牌。 
- 在并发或多线程程序中，建议使用 `strtok_r()`，这是一个可重入的版本。

使用 `strtok()` 时，请确保理解它如何修改输入字符串以及如何在多次调用之间保持状态。
