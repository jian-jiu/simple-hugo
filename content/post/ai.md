---
title: ai
date: 2026-03-07
slug: ai
categories:
    - ai
tags:
    - ai
---

## 图片高清化 面部修复 一键抠图
[参考](https://v11enp9ok1h.feishu.cn/wiki/QzYswcqoKiXlj7kJIi3c89Lwnnj)

## Tesseract训练识别数字
[详情](https://blog.csdn.net/soband_xiang/article/details/103808321)

## nacos教程

[官网](https://nacos.io/zh-cn/)
[参考](https://blog.csdn.net/chijiansong/article/details/114531115)

### 安装
#### 添加数据库表
sql在conf文件夹中的nacos-mysql.sql
在数据库添加sql nacos需要依赖数据库
#### 修改配置
在conf文件夹中application.properties
```sh
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/数据库名称?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
db.user.0=root
db.password.0=123456
```
#### win自动启动
nacos.bat
```sh
@echo off
D:\Software\nacos\bin\startup.cmd -m standalone
# pause
```

[访问地址](http://127.0.0.1:8848/nacos)
