---
title: homeassistant
date: 2026-03-31
categories:
    - homeassistant
tags:
    - homeassistant
    - esphome
---

homeassistant智能家居
[hacs](https://blog.csdn.net/2301_79855962/article/details/144984832)
[小米](https://zhuanlan.zhihu.com/p/16015024718)

"docker"版就是没有加载项商店，需要supervised或home assistant os版本

## 主题
mushroom

## 首页恢复默认
 ```
 概览->右上角三个小点->编辑仪表盘->再点三个小点->raw configuration editor->删除里面内容->输入a
strategy:
  type: original-states
保存，over
 ```

## 反向代理
configuration.yaml 最后添加
```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 127.0.0.1
    - ::1/128
```
