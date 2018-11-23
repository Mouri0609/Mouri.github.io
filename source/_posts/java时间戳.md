---
title: java时间戳
date: 2018-11-21 13:25:23
categories:
- java基础
tags:
- java基础
---
### java时间戳
<!--more-->
-------------------
#### 获取当前时间戳
一共三种方法
- System.currentTimeMillis();  
- Calendar.getInstance().getTimeInMillis()；
- new Date().getTime()；

#### 获取当前时间
>SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//设置日期格式
String date = df.format(new Date());// new Date()为获取当前系统时间，也可使用当前时间戳

仿照博客进行了时间测试。分别测试一次，时间差别不大。查看博客上的测试都是测试十万次。这样测得的数据才具有代表性。
[参考博客](https://blog.csdn.net/qq_15037231/article/details/78224321)
