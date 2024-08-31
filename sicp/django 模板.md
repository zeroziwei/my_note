---
tags: 
progress: 
creation date: 2024-08-23 00:08
modification date: 星期五 23日 八月 2024 00:08:39
---
这里有个问题：页面的设计写死在视图函数的代码里的。如果你想改变页面的样子，你需要编辑 Python 代码。所以让我们使用 Django 的模板系统，只要创建一个视图，就可以将页面的设计从代码中分离出来。

首先，在你的 `polls` 目录里创建一个 `templates` 目录。Django 将会在这个目录里查找模板文件。

你项目的 [`TEMPLATES`](https://docs.djangoproject.com/zh-hans/5.1/ref/settings/#std-setting-TEMPLATES) 配置项描述了 Django 如何载入和渲染模板。默认的设置文件设置了 `DjangoTemplates` 后端，并将 [`APP_DIRS`](https://docs.djangoproject.com/zh-hans/5.1/ref/settings/#std-setting-TEMPLATES-APP_DIRS) 设置成了 True。这一选项将会让 `DjangoTemplates` 在每个 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/5.1/ref/settings/#std-setting-INSTALLED_APPS) 文件夹中寻找 "templates" 子目录。这就是为什么尽管我们没有像在第二部分中那样修改 DIRS 设置，Django 也能正确找到 polls 的模板位置的原因。

在你刚刚创建的 `templates` 目录里，再创建一个目录 `polls`，然后在其中新建一个文件 `index.html` 。换句话说，你的模板文件的路径应该是 `polls/templates/polls/index.html` 。因为``app_directories`` 模板加载器是通过上述描述的方法运行的，所以 Django 可以引用到 `polls/index.html` 这一模板了。



