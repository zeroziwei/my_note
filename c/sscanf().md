---
tags: 
created_date: 2024-01-12 19:52
modified_date: ' 2024-01-12 19:52:11'
review_date: 
pomodoro_time: 
used_time: 
aliases:
---
ChatGPT: 
- `sscanf()` 函数是 C 语言标准库中的一个函数，用于从字符串中读取格式化的输入。它类似于 `scanf()` 函数，但是从一个字符串而不是标准输入读取数据。`sscanf()` 在处理字符串时非常有用，尤其是当你需要解析字符串中的特定数据时。
### 函数原型

```c
int sscanf(const char *str, const char *format, ...);
```

 
- `str`：包含要解析的数据的字符串。 
- `format`：一个格式字符串，指定期望从 `str` 中提取数据的格式。 
- `...`：一个或多个指向变量的指针，这些变量用于存储从 `str` 中提取的数据。
### 返回值

`sscanf()` 返回成功匹配和赋值的输入项数量。如果发生错误或达到字符串末尾，可能会返回较小的数值。
### 示例使用

```c
#include <stdio.h>

int main() {
    char *str = "100 apple 200";
    int num1, num2;
    char word[20];

    // 从字符串读取格式化的数据
    int ret = sscanf(str, "%d %s %d", &num1, word, &num2);

    // 检查是否成功读取了三个值
    if (ret == 3) {
        printf("Read values: %d, %s, %d\n", num1, word, num2);
    } else {
        printf("Failed to read three items.\n");
    }

    return 0;
}
```



在这个例子中，`sscanf()` 从字符串 `str` 中读取了一个整数、一个字符串和另一个整数。格式字符串 `"%d %s %d"` 指定了要读取的数据的格式。
### 注意事项 
- 确保提供给 `sscanf()` 的目标变量具有适当的类型，以匹配格式字符串中指定的类型。 
- 当使用 `%s` 格式符读取字符串时，确保目标字符串有足够的空间来存储结果，以避免缓冲区溢出。 
- 对于字符串和字符数组，`sscanf()` 会自动添加一个空字符 (`'\0'`) 作为字符串的结尾。 
- 像 `scanf()` 一样，`sscanf()` 也可能在输入不符合预期格式时遇到问题，因此始终检查返回值以确定读取的数据项数量。

`sscanf()` 是一个强大的函数，可以用来解析复杂的字符串格式，但要小心使用，特别是在处理潜在的不可信的输入时。
