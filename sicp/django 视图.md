---
tags: 
progress: 
creation date: 2024-08-22 19:44
modification date: 星期四 22日 八月 2024 19:44:56
---
每个视图必须要做的只有两件事：返回一个包含被请求页面内容的 [`HttpResponse`](https://docs.djangoproject.com/zh-hans/5.1/ref/request-response/#django.http.HttpResponse "django.http.HttpResponse") 对象，或者抛出一个异常，比如 [`Http404`](https://docs.djangoproject.com/zh-hans/5.1/topics/http/views/#django.http.Http404 "django.http.Http404") 。至于你还想干些什么，随便你。

```python3 
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

## 1	接受参数的视图

```python3 
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)


def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)


def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

