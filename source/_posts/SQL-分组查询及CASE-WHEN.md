---
title: SQL 分组查询
date: 2018-09-12 14:47:04
categories: SQL
tags: SQL 
description:SQL分组查询
---
# 使用场景
在出租日志表中查询出今天所有用户的使用时长。用户使用时长不固定，次数不固定

## 查询分析
先查询今天所有的记录，然后根据用户的id进行分组，将同一用户的使用时长进行统计

## case WHEN 的使用
```
 SELECT ABS(timestampdiff(Hour,case when 1>2 then '2018-09-10 00:00:00' else '2018-09-11 00:00:00' end,
            case when 1>2 then '2018-09-10 00:00:00' else '2018-09-11 08:00:00' end))
```

## 第一版SQL
内容包括：根据用户的开始使用时间来判断用户是否是连续的在使用，并计算今天的使用时长。分组排序限制条目
```
SELECT
user_id, sum(case when start_time>'2018-09-11 00:00:00' then used_time else  ABS(timestampdiff(Minute,end_time,'2018-09-11 00:00:00')) end) as use_time
FROM
use_record  GROUP BY user_id ORDER BY use_time DESC LIMIT 50
```
## 第二版SQL
由于用户ID展示出来很不好看，需要将用户名称展示出来
```
SELECT
a.username, sum(case when start_time>'2018-09-11 00:00:00' then used_time else  ABS(timestampdiff(Minute,end_time,'2018-09-11 00:00:00')) end) as use_time
FROM
use_record as b
LEFT JOIN pre_ucenter_members as a ON b.user_id = a.uid GROUP BY a.username ORDER BY use_time DESC LIMIT 50
```


