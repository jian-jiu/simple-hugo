---
title: dae daed
date: 2026-07-24
slug: dae
categories:
    - dae
tags:
    - dae
    - daed
    - daede
---

[配置参考](https://idev.dev/proxy/dae-configuration.html)

## 重启
```shell
/etc/init.d/dae restart
chmod 600 /etc/dae/config.dae
```

## dead
https://github.com/kenzok8/openwrt-daede

## simple
[规则文件](https://github.com/daeuniverse/dae/blob/main/example.dae)
dns googledns https://dns.google:443
```
# 参考 https://github.com/daeuniverse/dae/blob/main/docs/zh/configuration/routing.md
# 路由 新增配置
domain(suffix: argotunnel.com) -> direct
dport(7844) -> direct

domain(suffix: google.com) -> direct
```
