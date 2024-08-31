---
tags: 
progress: 
creation date: 2024-08-18 04:00
modification date: 星期日 18日 八月 2024 04:00:07
---
- [ ] Python如何计算装饰器句法 
- [ ] Python如何判断变量是不是局部的
- [ ] 闭包存在的原因和工作原理 
- [ ] nonlocal 能解决什么问题
- [ ] 实现行为良好的装饰器
- [ ] 标准库中有用的装饰器 
- [ ] 实现一个参数化装饰器



**装饰器**==是可调用的对象==，其参数是另一个函数（被装饰的函数）。 装饰器可能会处理被装饰的函数，然后把它返回，或者将其替换成另一个 函数或可调用对象。

简单来说装饰器就是使用函数作为参数的high order function 

```python3
def deco(func):
    def inner():
        print("run in the inner")
    return inner

@deco
def target():
    print("running target()")

target()
print(target)
```


![[decorator-20240818040900525.webp]]



## 1	何时运行


```python3 
registry = []

def register(func):
    print(f"running register {func}")
    registry.append(func)
    return func

@register
def f1():
    print("running f1()")


@register
def f2():
    print("running f2()")


def f3():
    print("running f3()")

def main():
    print("running main()")
    print("registry ->",registry)
    f1()
    f2()
    f3()

if __name__ == '__main__':
    main()
```


![[decorator-20240818041829854.webp]]



