---
tags: 
progress: 
creation date: 2024-08-22 20:49
modification date: 星期四 22日 八月 2024 20:49:36
---
	from django.db import models 

models.Model

[[python3 自省函数]]

Model.objects  是一个 Question.objects django.db.models.manager.Manager

Model.objects.all() 



In [23]: type(Question.objects.all())
Out[23]: django.db.models.query.QuerySet


