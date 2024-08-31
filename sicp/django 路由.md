---
tags: 
progress: 
creation date: 2024-08-22 20:01
modification date: 星期四 22日 八月 2024 20:01:37
---
项目 urls 

```python3 
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path("polls/", include("polls.urls")),
    path("admin/", admin.site.urls),
]
```

应用 urls 
```python3
from django.urls import path

from . import views

urlpatterns = [
    path("", views.index, name="index"),
]
```

函数 [`include()`](https://docs.djangoproject.com/zh-hans/5.1/ref/urls/#django.urls.include "django.urls.include") 允许引用其它 URLconfs。每当 Django 遇到 [`include()`](https://docs.djangoproject.com/zh-hans/5.1/ref/urls/#django.urls.include "django.urls.include") 时，它会截断与此项匹配的 URL 的部分，并将剩余的字符串发送到 URLconf 以供进一步处理。

我们设计 [`include()`](https://docs.djangoproject.com/zh-hans/5.1/ref/urls/#django.urls.include "django.urls.include") 的理念是使其可以即插即用。因为投票应用有它自己的 URLconf( `polls/urls.py` )，他们能够被放在 "/polls/" ， "/fun_polls/" ，"/content/polls/"，或者其他任何路径下，这个应用都能够正常工作。


当包括其它 URL 模式时你应该总是使用 `include()` ， `admin.site.urls` 是唯一例外。

函数 [`path()`](https://docs.djangoproject.com/zh-hans/5.1/ref/urls/#django.urls.path "django.urls.path") 具有四个参数，两个必须参数：`route` 和 `view`，两个可选参数：`kwargs` 和 `name`。现在，是时候来研究这些参数的含义了。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.1/ref/urls/#django.urls.path "django.urls.path") 参数： `route`[¶](https://docs.djangoproject.com/zh-hans/5.1/intro/tutorial01/#path-argument-route "永久链接至标题")

`route` 是一个匹配 URL 的准则（类似正则表达式）。当 Django 响应一个请求时，它会从 `urlpatterns` 的第一项开始，按顺序依次匹配列表中的项，直到找到匹配的项。

这些准则不会匹配 GET 和 POST 参数或域名。例如，URLconf 在处理请求 `https://www.example.com/myapp/` 时，它会尝试匹配 `myapp/` 。处理请求 `https://www.example.com/myapp/?page=3` 时，也只会尝试匹配 `myapp/`。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.1/ref/urls/#django.urls.path "django.urls.path") 参数： `view`[¶](https://docs.djangoproject.com/zh-hans/5.1/intro/tutorial01/#path-argument-view "永久链接至标题")

当 Django 找到了一个匹配的准则，就会调用这个特定的视图函数，并传入一个 [`HttpRequest`](https://docs.djangoproject.com/zh-hans/5.1/ref/request-response/#django.http.HttpRequest "django.http.HttpRequest") 对象作为第一个参数，被“捕获”的参数以关键字参数的形式传入。稍后，我们会给出一个例子。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.1/ref/urls/#django.urls.path "django.urls.path") 参数： `kwargs`[¶](https://docs.djangoproject.com/zh-hans/5.1/intro/tutorial01/#path-argument-kwargs "永久链接至标题")

任意个关键字参数可以作为一个字典传递给目标视图函数。本教程中不会使用这一特性。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.1/ref/urls/#django.urls.path "django.urls.path") 参数： `name`[¶](https://docs.djangoproject.com/zh-hans/5.1/intro/tutorial01/#path-argument-name "永久链接至标题")

为你的 URL 取名能使你在 Django 的任意地方唯一地引用它，尤其是在模板中。这个有用的特性允许你只改一个文件就能全局地修改某个 URL 模式。

当你了解了基本的请求和响应流程后，请阅读 [教程的第 2 部分](https://docs.djangoproject.com/zh-hans/5.1/intro/tutorial02/) 开始使用数据库.





