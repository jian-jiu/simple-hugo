---
title: ai
date: 2026-03-07
slug: ai
categories:
    - ai
tags:
    - ai
---

## GitHub Copilot
[汉化](https://github.com/TC999/jetbrains-copilot-chinese)

## 图片高清化 面部修复 一键抠图
[参考](https://v11enp9ok1h.feishu.cn/wiki/QzYswcqoKiXlj7kJIi3c89Lwnnj)

## Tesseract训练识别数字
[详情](https://blog.csdn.net/soband_xiang/article/details/103808321)

## nacos教程

[官网](https://nacos.io/zh-cn/)
[参考](https://blog.csdn.net/chijiansong/article/details/114531115)

### 安装
#### 添加数据库表
**sql在conf文件夹中的**nacos-mysql.sql
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

## 其他
[换脸](https://www.marsai.top)
[爬取网站](https://same.new/)
[AI绘画模型平台](https://civitai.com/)
[音视频翻译和配音工具](https://github.com/krillinai/KlicStudio)

## 网站ai翻译
[仓库](https://gitee.com/mail_osc/translate)
```html
<script>
    var head= document.getElementsByTagName('head')[0];
    var script= document.createElement('script');
    script.type= 'text/javascript';
    script.src= 'https://res.zvo.cn/translate/translate.js';
    script.onload = script.onreadystatechange = function() {
        translate.selectLanguageTag.show = false;
        translate.setUseVersion2();

        //translate.ignore.tag.push('small');

        translate.ignore.class.push('ant-card-head');
        translate.ignore.class.push('inline-block-tight');
        translate.ignore.class.push('last-keywords');
        translate.ignore.class.push('ant-table-tbody');
        translate.ignore.class.push('ant-select-dropdown-menu-item-group-list');
        translate.ignore.class.push('ant-select-dropdown-menu-vertical');
        translate.ignore.class.push('ant-select-selection-selected-value');

        translate.changeLanguage('chinese_simplified');

        window.onload = function() {
            translate.listener.start();
            translate.execute();
        };
    }
    head.appendChild(script);
</script>
```
