---
title: mpv
date: 2026-03-07
slug: mpv
categories:
    - mpv
tags:
    - mpv
    - 视频
---

[官网](https://mpv.io/)

## 增加弹幕
[原仓库](https://github.com/itKelis/MPV-Play-BiliBili-Comments)
[gitee](https://gitee.com/kuaifuzhi/blbl-dm)

cmd 管理员命令执行
mklink /D xxx\mpv-lazy\portable_config\scripts\bilibiliAssert xxx\scripts\bilibiliAssert

[下载弹幕](https://greasyfork.org/zh-CN/scripts/524107-bilibili-腾讯视频弹幕下载) 

## 帧提升
开启:  shift键 + 上数字键1
### 2025-12-29
mpv-lazy\portable_config\vs\MEMC_MVT_LQ.vpy
container_fps*2 -> container_fps*4

### 旧
mpv-lazy\portable_config\vs\MEMC_MVT_STD.vpy
H_Pre = 1440
Fps_Out = 85.0
Lk_Fmt = False

## 动画 Anime4K
[参考](https://zhuanlan.zhihu.com/p/117531230)
[参考](https://www.bilibili.com/read/cv13643303/)
[官方仓库](https://github.com/bloc97/Anime4K)
