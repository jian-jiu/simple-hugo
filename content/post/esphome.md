---
title: esphome
date: 2026-03-07
categories:
    - esp
tags:
    - esp
    - esphome
---

[蓝牙代理](https://bbs.hassbian.com/thread-20157-1-1.html)

## 开发
[开发者文档](https://developers.esphome.io/)
mklink /D "./components" "有内容的\components"
```yaml
external_components:
  - source: ./components
```

## 在线烧录
https://web.esphome.io/

## tds 水质
[参考](https://github.com/JochenZhou/esphome-custom-components)
https://github.com/ryanh7/esphome-custom-components
https://github.com/hqc201105/esphome_tds_sensor
https://bbs.hassbian.com/thread-23740-1-1.html


## 风扇
pwm 用 IO2
风速 用 IO1(不要用03)
[参考1](https://bbs.hassbian.com/thread-26892-1-1.html)
[参考2](https://bbs.hassbian.com/thread-19915-1-1.html)
