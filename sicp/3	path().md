## 3	path()

函数 [`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path") 具有四个参数，两个必须参数：`route` 和 `view`，两个可选参数：`kwargs` 和 `name`。现在，是时候来研究这些参数的含义了。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path") 参数： `route`[¶](https://docs.djangoproject.com/zh-hans/5.0/intro/tutorial01/#path-argument-route "永久链接至标题")

`route` 是一个匹配 URL 的准则（类似正则表达式）。当 Django 响应一个请求时，它会从 `urlpatterns` 的第一项开始，按顺序依次匹配列表中的项，直到找到匹配的项。

这些准则不会匹配 GET 和 POST 参数或域名。例如，URLconf 在处理请求 `https://www.example.com/myapp/` 时，它会尝试匹配 `myapp/` 。处理请求 `https://www.example.com/myapp/?page=3` 时，也只会尝试匹配 `myapp/`。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path") 参数： `view`[¶](https://docs.djangoproject.com/zh-hans/5.0/intro/tutorial01/#path-argument-view "永久链接至标题")

当 Django 找到了一个匹配的准则，就会调用这个特定的视图函数，并传入一个 [`HttpRequest`](https://docs.djangoproject.com/zh-hans/5.0/ref/request-response/#django.http.HttpRequest "django.http.HttpRequest") 对象作为第一个参数，被“捕获”的参数以关键字参数的形式传入。稍后，我们会给出一个例子。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path") 参数： `kwargs`[¶](https://docs.djangoproject.com/zh-hans/5.0/intro/tutorial01/#path-argument-kwargs "永久链接至标题")

任意个关键字参数可以作为一个字典传递给目标视图函数。本教程中不会使用这一特性。

### [`path()`](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path "django.urls.path") 参数： `name`[¶](https://docs.djangoproject.com/zh-hans/5.0/intro/tutorial01/#path-argument-name "永久链接至标题")

为你的 URL 取名能使你在 Django 的任意地方唯一地引用它，尤其是在模板中。这个有用的特性允许你只改一个文件就能全局地修改某个 URL 模式。