---
tags: 
progress: 
creation date: 2025-01-02 00:38
modification date: 星期四 2日 一月 2025 00:38:10
---
"easier to ask for forgiveness than permission"（更容易请求原谅而不是许可）是一种编程风格，在 Python 中表现为优先假设操作是可行的，然后在假设错误的情况下处理异常。这种风格被称为 EAFP（Easier to Ask for Forgiveness than Permission）。与此相对的风格是 LBYL（Look Before You Leap），在操作之前通过条件检查来确保操作的安全性。这两种风格各有优劣，适用于不同的编程情境。

### EAFP 示例

在 Python 中，EAFP 风格常常通过 `try` 和 `except` 语句来实现。下面是使用 EAFP 风格访问字典的示例：

```python
data = {'name': 'Alice', 'age': 30}

# EAFP 风格
try:
    age = data['age']
    print(f"Age: {age}")
except KeyError:
    print("Age key not found.")
```

在这个例子中，代码假设字典 `data` 中存在键 `'age'`。如果键不存在，会引发 `KeyError` 异常，随即在 `except` 代码块中进行处理。

### LBYL 示例

LBYL 风格在执行操作之前进行明确检查，使代码更具防御性：

```python
data = {'name': 'Alice', 'age': 30}

# LBYL 风格
if 'age' in data:
    age = data['age']
    print(f"Age: {age}")
else:
    print("Age key not found.")
```

在这个例子中，代码先检查 `'age'` 键是否存在，然后再进行访问。这样的代码在某些场合下更易读，并防止了潜在异常。

### 优缺点比较

- **EAFP 风格**：
  - **优点**：代码更简洁，假设操作大多数情况下都会成功。由于 Python 提供了高效的异常处理机制，在正常情况下可能比 LBYL 更快。
  - **缺点**：如果操作失败比较频繁，性能可能会受到影响。此外，在不清楚异常可能性的情境下，代码的可读性和维护性较差。

- **LBYL 风格**：
  - **优点**：在操作之前进行检查，避免了不必要的异常，在失败风险较高的情况下更可靠。
  - **缺点**：代码可能更加冗长，尤其是在需要做大量的检查时。

### 总结

选择 EAFP 还是 LBYL 风格通常取决于具体的应用场景和开发者的偏好。在处理不确定性较高的操作时，EAFP 风格可能提供了更为直接的处理方法；而在需要更多控制并且希望避免异常的场合，LBYL 可能更合适。Python 由于其动态性和对异常处理的高效支持，通常鼓励使用 EAFP 风格。