---
title: 服务器
description: 服务器相关
date: 2026-03-07
slug: fwq
categories:
    - 服务器
tags:
    - 服务器
---

## https
[申请地址](https://freessl.cn/apply)
[管理地址](https://freessl.cn/login)

[Let's Encrypt证书自动更新](https://blog.csdn.net/shasharoman/article/details/80915222)

[免费和开源的HTTPS 代理](https://www.mitmproxy.org/)

## api请求工具
- 流行 [postmain](https://editor.csdn.net/md/?articleId=120696786)
  团队最多3人
- 免费 [Apifox](https://www.apifox.cn/)
  不限团队人数
- [小幺鸡](http://www.xiaoyaoji.cn/)
  不知道

## FileZilla FTP,FTPS和SFTP客户端
[FileZilla官网](https://www.filezilla.cn/)
[介绍连接](https://www.filezilla.cn/features)

## tomcat乱码
[详细](https://www.cnblogs.com/zeussbook/p/10535120.html)

### 下载txt

conf/web.xml

```js
<mime-mapping>
<extension>txt</extension>
<mime-type>application/txt</mime-type>
</mime-mapping>
```

浏览：text/plain

下载：application/txt

iis下载：application/octet-stream

### 浏览目录

<servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

改true