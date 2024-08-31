---
tags:
  - doing
progress: 50
---
[Strings and Character Data in Python – Real Python](https://realpython.com/python-strings/)

[Python's F-String for String Interpolation and Formatting – Real Python](https://realpython.com/python-f-strings/)

[Python String Formatting: Available Tools and Their Features – Real Python](https://realpython.com/python-string-formatting/)

[realpython/materials: Bonus materials, exercises, and example projects for our Python tutorials](https://github.com/realpython/materials/tree/master)

>[!check]
>- [x] Learn the **format mini-language syntax** ✅ 2024-07-17
>- [x] **Align** and **fill** textual output in your code ✅ 2024-07-18
>- [x] **Convert** between data **types** in your outputs ✅ 2024-07-18
>- [x] Provide **format fields** dynamically ✅ 2024-07-18
>- [x] Format **numeric values** in different ways ✅ 2024-07-18


- [x] fstring 📅 2024-07-17 ✅ 2024-07-18

## Using String Interpolation and Replacement Fields

[Python's Format Mini-Language for Tidy Strings – Real Python](https://realpython.com/python-format-mini-language/)

When you’re working with strings, it’s common that you need to embed or insert values and objects into your strings so that you can build new strings dynamically. This task is commonly known as **string interpolation**.

In Python, you’ll find three popular tools that allow you to perform string interpolation:

1. the modulo operator(%)
2. the string.format() method
3. F-string literals

To dynamically interpolate a value into a string, you need something called **replacement fields**. In both `str.format()` and f-strings, curly braces (`{}`) delimit replacement fields. Inside these braces, you can put a variable, expression, or any object. When you run the code, Python replaces the field with the actual value
<!--ID: 1721824704242-->


## 2	The `str.format()` Method

```python 
>>> "Hello, {0}! Good {1}!".format("Pythonista", "morning")
'Hello, Pythonista! Good morning!'

>>> "Hello, {name}! Good {moment}!".format(
...    name="Pythonista", moment="morning"
... )
'Hello, Pythonista! Good morning!'
```

In the first example, you use integer indices to define the order in which you want to insert each argument into the replacement fields. In the second example, you use explicit argument names to insert the values into the final string.

The Python documentation uses the following  [[BNF]] to define the syntax of a replacement field for the `.format()` method:

```BNF
replacement_field ::=  "{"
                            [field_name]
                            ["!" conversion]
                            [":" format_spec]
                       "}"
```

From this BNF rule, you can conclude that the field name is optional. After that, you can use an exclamation mark (`!`) to provide a quick `conversion` field. This field can take one of the following forms:

- `!s` calls [`str()`](https://docs.python.org/3/library/functions.html?highlight=built#func-str) on the argument.
- `!r` calls [`repr()`](https://docs.python.org/3/library/functions.html?highlight=built#repr) on the argument.
- `!a` calls [`ascii()`](https://docs.python.org/3/library/functions.html?highlight=built#ascii) on the argument.

As you can see, the `conversion` field allows you to use different [string representations](https://realpython.com/python-repr-vs-str/#how-can-you-access-an-objects-string-representations) for the value that you want to interpolate into your strin

As an example of how the `conversion` field works, say that you have the following `Person` class:

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"I'm {self.name}, and I'm {self.age} years old."

    def __repr__(self):
        return f"{type(self).__name__}(name='{self.name}', age={self.age})"
```

In this class, you have two [instance attributes](https://realpython.com/python-classes/#instance-attributes), `.name` and `.age`. Then, you have `.__str__()` and `.__repr__()` [special methods](https://realpython.com/python-magic-methods/) to provide user-friendly and developer-friendly string representations for your class, respectively.

To learn how the `conversion` field works, go ahead and run the following code:

```python
from person import Person

jane = Person("Jane Doe", 25)

"Hi! {!s}".format(jane)


"An instance: {!r}".format(jane)
```

In these examples, you use the `!s` and `!r` conversion fields with different purposes. In the first example, you get a user-friendly message. In the second example, you get a developer-friendly message.

The final part of the replacement field syntax is also optional. It’s a string that must start with a colon (`:`) and can continue with a **format specifier**. You’ll learn about format specifiers in a moment. For now, you’ll continue with f-strings and learn how they implement replacement fields.
- 🍅 (pomodoro::WORK) (duration:: 25m) (begin:: 2024-07-17 15:19) - (end:: 2024-07-17 15:44)
<!--ID: 1721204088742-->
