---
title: mq
date: 2026-04-12
slug: mq
categories:
    - mq
tags:
    - mq
    - rabbitmq
---
# 下载
rabbitmq基于erlang语言需要erlang环境
## erlang官网下载地址
[个人下载网站](https://erl.uip6.com)
[https://www.erlang.org/downloads](https://www.erlang.org/downloads)


## rabbitmq官网下载地址
[https://www.rabbitmq.com/download.html](https://www.rabbitmq.com/download.html)
Windows Installer
> windows进去后还要下拉到中间位置点击这个下载
Direct Downloads
# 安装
erlang无脑一下步就行了，可以自己配置一下安装目录
> 配置rabbitmq环境变量  %ERLANG_HOME%\bin

rabbitmq无脑一下步就行了，可以自己配置一下安装目录
## 安装RabbitMQ-Plugins
有了这个就能在浏览器中查看 RabbitMQ 了
cmd进入 /sbin 下，输入
```shell
rabbitmq-plugins enable rabbitmq_management
```
浏览器中输入 [http://localhost:15672](http://localhost:15672) 就能看到  RabbitMQ的首页，默认账号与密码为 guest