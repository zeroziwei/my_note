---
tags: 
progress: 
creation date: 2024-08-22 21:29
modification date: 星期四 22日 八月 2024 21:29:06
---
在 Python 中，有多种方法可以查看对象的信息，包括其类型、属性和方法等。以下是一些常用的方法：

### 1. 使用 `type()` 函数

`type()` 函数可以返回对象的类型：

```python
obj = [1, 2, 3]
print(type(obj))  # 输出: <class 'list'>
```

### 2. 使用 `dir()` 函数

`dir()` 函数可以返回对象的所有属性和方法：

```python
obj = [1, 2, 3]
print(dir(obj))
# 输出: ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

### 3. 使用 `help()` 函数

`help()` 函数可以提供对象的详细文档信息：

```python
obj = [1, 2, 3]
help(obj)
# 输出: 显示 list 类的详细文档信息
```

### 4. 使用 `vars()` 函数

`vars()` 函数可以返回对象的属性和属性值的字典：

```python
class MyClass:
    def __init__(self):
        self.x = 1
        self.y = 2

obj = MyClass()
print(vars(obj))  # 输出: {'x': 1, 'y': 2}
```

### 5. 使用 `__dict__` 属性

对于大多数对象，`__dict__` 属性可以返回对象的属性和属性值的字典：

```python
class MyClass:
    def __init__(self):
        self.x = 1
        self.y = 2

obj = MyClass()
print(obj.__dict__)  # 输出: {'x': 1, 'y': 2}
```

### 6. 使用 `inspect` 模块

`inspect` 模块提供了更高级的 introspection 功能，可以查看对象的详细信息：

```python
import inspect

class MyClass:
    def __init__(self):
        self.x = 1
        self.y = 2

    def my_method(self):
        pass

obj = MyClass()
print(inspect.getmembers(obj))
# 输出: 显示对象的所有成员信息
```

### 7. 使用 `__class__` 属性

`__class__` 属性可以返回对象的类：

```python
obj = [1, 2, 3]
print(obj.__class__)  # 输出: <class 'list'>
```

### 总结

以上方法可以帮助你查看 Python 对象的类型、属性和方法等信息。根据具体需求选择合适的方法，可以更方便地进行调试和开发。