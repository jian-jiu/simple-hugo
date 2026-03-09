---
title: go
date: 2026-03-09
slug: go
categories:
    - go
tags:
    - go
---

## 查看环境变量
```shell
go env
```

## 下载源
```shell
// direct表示如果在下载源找不到会回原来的地址去下载
go env -w GOPROXY=https://mirrors.aliyun.com/goproxy,direct
```
