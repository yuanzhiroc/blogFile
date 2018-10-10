---
title: SQL UPDATE更新多条记录中的一条
date: 2018-09-07 14:47:04
categories: SQL
tags: SQL 
description: SQL UPDATE更新多条记录中的一条
---

# 问题环境
数据库中有这样一张表record
id | box | paper | owner_id
-- | -- | -- | --
1 | 1 | 0 | NULL
2 | 1 | 1 | NULL
3 | 1 | 2 | NULL
4 | 2 | 0 | NULL
5 | 2 | 1 | NULL
6 | 2 | 2 | NULL
7 | 2 | 3 | NULL
# 问题原因
我有这样一条查询语句
```
update record set owner_id = user_id where box = 1 and owner_id is NULL
```
本来的目的是想让一个用户占用一个盒子空纸张的记录，但这样的查询语句会让用户成为盒子里所有纸张的占有者
# 解决问题
经过查询后的解决方案：LIMIT
```
UPDATE record set owner_id = user_id where box = 1 and owner_id is NULL limit 1
```
这样就可以让更新的数据只作用于一条