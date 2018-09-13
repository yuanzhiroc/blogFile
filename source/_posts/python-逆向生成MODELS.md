---
title: python 逆向生成Models求
date: 2018-09-08 10:47:14
categories: python, Django, SQL
tags: python, Django, SQL 
description:python 逆向生成Models
---
# Django

> python manage.py inspectdb > models.py

# sqlachemy
```
# 必须先安装sqlacodegen
sqlacodegen mysql+pymysql://user:password@IP_ADDR:3306/database_name > models.py
```
