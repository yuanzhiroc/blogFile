---
title: python 定时任务
date: 2018-08-20 09:47:04
categories: python
tags: python
description: python 定时任务
---
## 自己写一个定时任务
### 首先创建一个执行时间
```
sch_time = datetime.datetime(2018,8,20,10,0,0)
```
### 写一个函数用来测试定时任务是否执行
```
def fun(str_msg):
    print("hello I'm run", str_msg)
```
### 定时任务的功能
```
def sch_fun(sch_time):
    flag = 0 #是否执行
    run_time = sched_timer.strftime("%Y-%m-%d %H:%M:%S")
    # 转换成统一格式的字符串进行比较
    while True:
        now = datetime.datetime.now()
        now_time = now.strftime("%Y-%m-%d %H:%M:%S")
        # 得到当前时间并转换成统一格式的字符串进行比较
        if now_time == run_time and flag == 0:
            fun("time 1") # 执行
            flag = 1
        else: # 上面部分就是定时任务的执行，下面是间隔任务处理
            if flag == 1:
                sched_timer = sched_timer+datetime.timedelta(seconds=5)
                run_time = sched_time.strftime("%Y-%m-%d %H:%M:%S")
                flag = 0
                # 如果执行过了调到下一个执行时间（5秒后)
            else:
                if sched_timer <= now:
                # 调整时间
                    sched_timer = sched_timer + datetime.timedelta(seconds=5)
                    run_time = sched_timer.strftime("%Y-%m-%d %H:%M:%S")

```
然后进行调用就可以运行了
## 使用schedule
这个我感觉不怎么好用（但是比自己写的要好） 就贴上从别人那找来的代码好了
```
import schedule
import time,datetime
def job():
    print("I'm working...")

schedule.every(10).minutes.do(job)
# 每10分钟执行一次
schedule.every().hour.do(job)
# 每小时执行一次
schedule.every().day.at("10:30").do(job)
# 每天的10：30执行一次
schedule.every().monday.do(job)
# 每周一执行一次
schedule.every().wednesday.at("13:14").do(job)
# 每个星期三的13：14分执行一次
# 在job后面直接加参数即可传参‘，’分隔
while True:
    schedule.run_pending()
    time.sleep(1)
# time.sleep(1)这里就是我运行时发现麻烦的地方！
# 不sleep他就会一直占用cpu运行判断是否到了执行时间
```
## 使用APscheduler
安装包的包名就是apscheduler
### 引入包
```
from apscheduler.schedulers.blocking import BlockingScheduler
```
### 使用方式
这里只写了执行的流程
```
# 初始化
sche = BlockingScheduler()
# 增加任务
sche.add_job(任务)
# 执行
sche.start()
```
### 定义执行的任务
这个是要在定时任务里执行的方法（函数）
```
def my_job(str_msg):
    # 定义执行的任务
    print("hello run!!!!",str_msg)
```

### 增加间隔式的任务
间隔式任务我这里是指不管现实的时间，只管运行时间
```
# 参数（函数名，类型，关键字参数）
sched.add_job(my_job, 'interval', seconds=20, args=("seconds 20",))
# 每隔20秒运行
sched.add_job(my_job, 'interval',hours=1, minutes=1, seconds=1, args=("minute 1 seconds 1",))
# 每隔一个小时一分一秒运行

```
### 增加定时式的任务
定时任务是指与现实时间挂钩
```
sched.add_job(my_job, 'cron', second=11, args=("every second 11",))
# 每分钟的11s
sched.add_job(my_job, 'cron', minute=3, args=("ever minute 3",))
# 每小时的第3分钟
```
### 更多的参数请参考
本文参考地址：(https://www.cnblogs.com/luxiaojun/p/6567132.html#[3990018])
