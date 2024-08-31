---
tags: 
progress: 
creation date: 2024-08-22 19:25
modification date: 星期四 22日 八月 2024 19:25:27
---
模型准确且唯一的描述了**数据**。它包含您储存的数据的重要**字段**和**行为**。一般来说，每一个模型都映射一张数据库表。

基础：

- 每个模型都是一个 Python 的类，这些类继承 [`django.db.models.Model`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/instances/#django.db.models.Model "django.db.models.Model")
- 模型类的每个属性都相当于一个数据库的字段。
- 利用这些，Django 提供了一个自动生成访问数据库的 API；请参阅 [执行查询](https://docs.djangoproject.com/zh-hans/5.1/topics/db/queries/)。

```python3 
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

每个模型被表示为 [`django.db.models.Model`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/instances/#django.db.models.Model "django.db.models.Model") 类的子类。每个模型有许多类变量，它们都表示模型里的一个数据库字段。

每个字段都是 [`Field`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.Field "django.db.models.Field") 类的实例 - 比如，字符字段被表示为 [`CharField`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.CharField "django.db.models.CharField") ，日期时间字段被表示为 [`DateTimeField`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField") 。这将告诉 Django 每个字段要处理的数据类型。

每个 [`Field`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.Field "django.db.models.Field") 类实例变量的名字（例如 `question_text` 或 `pub_date` ）也是字段名，所以最好使用对机器友好的格式。你将会在 Python 代码里使用它们，而数据库会将它们作为列名。

你可以使用可选的选项来为 [`Field`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.Field "django.db.models.Field") 定义一个人类可读的名字。这个功能在很多 Django 内部组成部分中都被使用了，而且作为文档的一部分。如果某个字段没有提供此名称，Django 将会使用对机器友好的名称，也就是变量名。在上面的例子中，我们只为 `Question.pub_date` 定义了对人类友好的名字。对于模型内的其它字段，它们的机器友好名也会被作为人类友好名使用。

定义某些 [`Field`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.Field "django.db.models.Field") 类实例需要参数。例如 [`CharField`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.CharField "django.db.models.CharField") 需要一个 [`max_length`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.CharField.max_length "django.db.models.CharField.max_length") 参数。这个参数的用处不止于用来定义数据库结构，也用于验证数据，我们稍后将会看到这方面的内容。

[`Field`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.Field "django.db.models.Field") 也能够接收多个可选参数；在上面的例子中：我们将 `votes` 的 [`default`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.Field.default "django.db.models.Field.default") 也就是默认值，设为0。

注意在最后，我们使用 [`ForeignKey`](https://docs.djangoproject.com/zh-hans/5.1/ref/models/fields/#django.db.models.ForeignKey "django.db.models.ForeignKey") 定义了一个关系。这将告诉 Django，每个 `Choice` 对象都关联到一个 `Question` 对象。Django 支持所有常用的数据库关系：多对一、多对多和一对一。

## 1	激活模型 

上面的一小段用于创建模型的代码给了 Django 很多信息，通过这些信息，Django 可以：

- 为这个应用创建数据库 schema（生成 `CREATE TABLE` 语句）。
- 创建可以与 `Question` 和 `Choice` 对象进行交互的 Python 数据库 API。

但是首先得把 `polls` 应用安装到我们的项目里。


>[!important] 
>Django 应用是“可插拔”的。你可以在多个项目中使用同一个应用。除此之外，你还可以发布自己的应用，因为它们并不会被绑定到当前安装的 Djan

>
为了在我们的工程中包含这个应用，我们需要在配置类 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/5.1/ref/settings/#std-setting-INSTALLED_APPS) 中添加设置。因为 `PollsConfig` 类写在文件 `polls/apps.py` 中，所以它的点式路径是 `'polls.apps.PollsConfig'`。在文件 `mysite/settings.py` 中 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/5.1/ref/settings/#std-setting-INSTALLED_APPS) 子项添加点式路径后，它看起来像这样：


```python3 
INSTALLED_APPS = [
    "polls.apps.PollsConfig",
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]
```

	python manage.py makemigrations polls
	

[[ORM]]

