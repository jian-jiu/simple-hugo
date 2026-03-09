---
title: springboot
date: 2026-03-09
slug: springboot
categories:
    - springboot
tags:
    - springboot
---

## 把接口参数中的空白值替换为null值
[参考文档](https://zhuanlan.zhihu.com/p/344190594)


## 事件发布以及注册
[参考](https://www.cnblogs.com/juncaoit/p/13275339.html)

# 注意事项：
1、如果2个事件之间是继承关系，会先监听到子类事件，处理完再监听父类。

2、监听器方法一定要try-catchy异常，否则会造成发布事件（有事务的）的方法进行回滚

3、可以使用@Order注解控制多个监听器的执行顺序，@Order 传入的值越小，执行顺序越高