---
title: hasattr 对字典使用无效 
date: 2018-08-16 11:47:00 
categories: python 
tags: errorUse 
description: 对字典使用了hasattr的错误
---

# 原因
今天在判断字典的内容的时候使用了hasattr来判断字典中是否有这个key
```
hasattr(dict,key)
```
这样做是错误的，在运行时无法达到我所预期的效果

正确的做法：
---
```
key in dict.keys()
```
由于自己已经这样用了多次，记录一下提醒自己
