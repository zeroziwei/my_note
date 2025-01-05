---
tags: 
progress: 
creation date: 2025-01-02 01:25
modification date: 星期四 2日 一月 2025 01:25:24
---
`yield from` 是 Python 3.3 引入的一种更高级的生成器语法，用于简化生成器函数之间的委托调用。使用 `yield from` 可以有效地将生成器逻辑进行组合，使代码更高效、更可读，减少了手动迭代和转发的繁琐。

### 基本语法

```python
yield from <iterable>
```

这里的 `<iterable>` 可以是一个生成器、迭代器，或者任何可迭代对象。

### 主要功能

1. **委托迭代**：`yield from <iterable>` 将迭代对象中所有的值一个接一个地产出，就如同它们是直接由外围生成器所产生的。

2. **返回值的传递**：如果被委托的生成器或者迭代器使用了 `return` 语句，该返回值会被捕获为 `yield from` 表达式的值。通常的生成器中，`return value` 会触发 `StopIteration(value)` 异常，`yield from` 可以捕捉此异常，并直接将 `value` 作为结果返回。

3. **处理发送和异常**：当使用 `send()` 向生成器发送值时，`yield from` 可以将值直接传递给内部的生成器。同样，通过 `throw()` 方法发送的异常也可以被内部生成器捕获。

4. **最终协议**：委托生成器正常结束（没有显式 `return`）后，会继续执行 `yield from` 之后的代码。

### 例子

以下是一个简单的例子，展示 `yield from` 的用法：

```python
def subgenerator():
    yield 1
    yield 2
    return 'Done'

def delegating_generator():
    result = yield from subgenerator()
    print('Subgenerator return value:', result)
    yield 3

# 使用生成器
gen = delegating_generator()

for value in gen:
    print(value)
```

### 运行结果

```
1
2
Subgenerator return value: Done
3
```

### 解释

- `subgenerator` 是一个简单的子生成器，它 `yield` 了两个值 (`1` 和 `2`)，然后使用 `return 'Done'` 来终止，返回 `'Done'`。

- `delegating_generator` 使用 `yield from subgenerator()` 来委托子生成器的值。当子生成器结束并返回 `Done` 时，这个返回值被捕获到了 `result` 中，并打印出来。

### 使用场景

- **简化生成器链**：在复杂的生成器场景下，`yield from` 通过让一个生成器委托给多个子生成器，避免了手动转发值、异常和返回值的麻烦。

- **协程和并发**：在异步协程模型出现在 Python 3.5 之前，`yield from` 被广泛用于简化异步操作和协程的实现。

总而言之，`yield from` 提供了一种优雅的方式来解决生成器的复合和委托问题，减少了大量样板代码并提升了代码的可读性。






使用 `yield from` 可以有效地减少手动解决**生成器**之间转发值、处理异常和返回返回值的**复杂性**。让我们逐一详细说明这些方面：

### 1. 转发值

在没有 `yield from` 的情况下，如果一个生成器 `A` 想要利用另一个生成器 `B` 的值，通常需要手动迭代生成器 `B` 的所有值并依次产出，类似于下面的**伪代码**：

```python
def generator_a():
    for value in generator_b():
        # 手动产出 generator_b 生成的值
        yield value
```

这种手动迭代尽管可以实现相同功能，但需要显式地写出循环来处理每一个值，这在复杂生成器链中的多个嵌套层次中可能成为重复的样板代码。

使用 `yield from`，这个过程被自动化：

```python
def generator_a():
    yield from generator_b()
```

`yield from` 自动处理迭代，顺序地产出 `generator_b` 的每一个值。

### 2. 处理异常

当生成器内部有异常抛出时，手动转发需要显式处理，例如**捕获异常**并将其重新抛出，或者在两层生成器间特别处理这些异常：

```python
def generator_a():
    it = iter(generator_b())
    while True:
        try:
            value = next(it)
            yield value
        except StopIteration:
            break
        except SomeException as e:
            # 手动处理异常
            raise e
```

`yield from` 自动承担了这些异常的传播，并自动处理来自被委托生成器的任何异常传播：

```python
def generator_a():
    yield from generator_b()
```

这里 `yield from` 会自动捕获 `generator_b` 抛出的异常，并适当地将它们传播到外部生成器中。

### 3. 获取返回值

当一个生成器完成执行并使用 `return` 返回一个值时，Python 生成器实际上是利用 `StopIteration` 异常携带返回值。手动捕获这个返回值需要访问异常对象的 `value` 属性：

```python
def generator_a():
    it = iter(generator_b())
    try:
        while True:
            value = next(it)
            yield value
    except StopIteration as e:
        result = e.value
```

`yield from` 自动捕获并分配这个返回值：

```python
def generator_a():
    result = yield from generator_b()
    # 在这里可以直接使用 result
```

### 总结

`yield from` 在概念上相当于一条打包的管道，处理所有的值传递、异常传递和返回值传递的细节，让开发者可以专注于生成器逻辑本身，而不必为多个嵌套生成器场景的样板代码操心。这样不仅减少了代码量，也降低了出错的概率，使代码更加简洁和可维护。

