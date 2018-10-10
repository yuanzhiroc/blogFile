---
title: Django 跨域请求
date: 2018-09-07 13:47:04
categories: 
	- python
	- Django
tags: 
	- python
	- Django 
description: Django 跨域请求
---
# 问题原因
暂时也没有了解问题的原因是怎样的，直接说解决方法吧
# 解决方案
## 安装corsheaders
```
> pip install corsheaders
```

## settings文件
```
ALLOW_HOSTS = ['*'] # 允许访问的地址
CORS_ORIGIN_ALLOW_ALL = True
CORS_ALLOW_HEADERS = ( # 允许添加自定义的头部HEADER
    'Uid', # 自定义
    'Authorization' # 这个默认会有
)
CORS_ALLOW_METHODS = (
    '*'
    # 'GET',
    # 'POST',
    # 'OPTIONS',
)
INSTALLED_APPS = [
    'corsheaders',
    .....
]
MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ....
]
# 示范 #################################
CORS_ORIGIN_WHITELIST = (
    '*'
)

CORS_ALLOW_METHODS = (
    'DELETE',
    'GET',
    'OPTIONS',
    'PATCH',
    'POST',
    'PUT',
    'VIEW',
)

CORS_ALLOW_HEADERS = (
    'XMLHttpRequest',
    'X_FILENAME',
    'accept-encoding',
    'authorization',
    'content-type',
    'dnt',
    'origin',
    'user-agent',
    'x-csrftoken',
    'x-requested-with',
    'Pragma',
)
###########################################
```