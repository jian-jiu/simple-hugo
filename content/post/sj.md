---
title: 手机
date: 2026-03-10
slug: sj
categories:
    - 手机
tags:
    - 手机
    - 刷机
---


[中文网](https://lsposed.cn)

[烟雨OTA工具箱](https://www.rainyweb.cn/)

[rom站](https://www.hyperos.fans/zh/devices)

## 手机
k90pm
### 屏幕
OPPOx9也是 ipts

### 电池
电压 4.53
额定容量 7420mAh
典型电容量 7560mAh
标称电压 3.79
额定能量 28.13Wh
典型能量 28.66Wh

## 模块

### 元模块
[解释](https://kernelsu.org/zh_CN/guide/metamodule.html)
需要第一个安装

[Magic Mount v1.0.1-sprout](https://github.com/KernelSU-Modules-Repo/meta-mm)
这个是基础环境 不用安装

[推荐 hybrid_mount](https://github.com/Hybrid-Mount/meta-hybrid_mount)

### 环境隐藏
[Zygisk Next-1.3.2-688-2c60cdd-release 5ec1cff](https://github.com/Dr-TSNG/ZygiskNext)
最新已内置隐藏环境（不需要shamiko）

[NeoZygisk](https://github.com/JingMatrix/NeoZygisk)
这个更新比较勤快

[Shamiko-v1.2.5-414](https://github.com/LSPosed/LSPosed.github.io/releases)

### LSPosed
目前看到最高 1.9.2-it(7469) github Dxandme
[JingMatrix/LSPosed 1.11.0](https://github.com/JingMatrix/LSPosed/releases)

### TrickyStore
[仓库](https://github.com/5ec1cff/TrickyStore)

### Scene
[9](https://download.omarea.com/#/versions?folder=scene9)

#### 吟婉兮调度(不推荐)
[下载](https://github.com/yinwanxi/Uperf-Game-Turbo/releases)

### AdGuardHomeForRoot dns 过滤广告
[官方](https://github.com/twoone-3/AdGuardHomeForRoot)

[其他个人修改版本](https://github.com/zFlqwovo/AdGuardHomeForRoot)
[区别](https://github.com/zFlqwovo/AdGuardHomeForRoot/issues/2)

## AutoPurge 清理垃圾
[1.0.12.0](https://lsposed.cn/1107)

## InstallerX 安装器
[下载](https://github.com/wxxsfxyzm/InstallerX-Revived)

## HyperCeiler
[官网](https://hyperceiler.sevtinge.com)
[下载](https://github.com/ReChronoRain/HyperCeiler/releases)
[r1](https://github.com/Aurora-dotcom/HyperCeiler-R1)

## oshape
[github](https://github.com/xzakota/HyperOShape)
模块在 飞机

## HyperLight 液态玻璃 高光样式
[下载](https://github.com/KiminonawaResa/HyperLight)

## HookVip
[官网](https://github.com/Xposed-Modules-Repo/top.hookvip.pro)

### 下面的不知道干嘛的

### 游戏完整性修复
[下载](https://github.com/KOWX712/PlayIntegrityFix/)

### 模块用于修改 Android Keystore 生成的 Android KeyAttestation 证书链
[下载](https://github.com/5ec1cff/TrickyStore)

### magic_mount_rs 为KernelSU 提供 Systemless 修改功能
[下载](https://github.com/KernelSU-Modules-Repo/magic_mount_rs/)

## lsp模块
### 隐藏应用列表
[仓库](https://github.com/Dr-TSNG/Hide-My-Applist/releases)

## 推荐
[酷安](https://www.coolapk.com)

爱玩机工具箱(酷安下载)

## usb
头
黑(负) 绿(D+) 黄(cc) 白(D-) 红(正)
尾

## 直供电（test）
### 重启
1. 5.5v 法拉电容 10f 以上
2. 输入线 并联 200uf 电解电容

## 其他 
[吾爱破解参考](https://www.52pojie.cn/thread-1143511-1-1.html)
[高通9008模式](https://onfix.cn/course/3476)

## twrp

[ADB 调试手机的三种方式（USB、WLAN、WIFI）](https://cloud.tencent.com/developer/article/1809910)

### twrp下载地址
[https://twrp.me/Devices](https://twrp.me/Devices)

### 其他
[吾爱破解参考](https://www.52pojie.cn/thread-1143511-1-1.html)
[高通9008模式](https://onfix.cn/course/3476)

### Ubuntu 镜像
mount -o remount rw /

[参考](https://developer.aliyun.com/mirror/ubuntu)
```
deb https://mirrors.aliyun.com/ubuntu/ xenial main
deb-src https://mirrors.aliyun.com/ubuntu/ xenial main

deb https://mirrors.aliyun.com/ubuntu/ xenial-updates main
deb-src https://mirrors.aliyun.com/ubuntu/ xenial-updates main

deb https://mirrors.aliyun.com/ubuntu/ xenial universe
deb-src https://mirrors.aliyun.com/ubuntu/ xenial universe
deb https://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb-src https://mirrors.aliyun.com/ubuntu/ xenial-updates universe

deb https://mirrors.aliyun.com/ubuntu/ xenial-security main
deb-src https://mirrors.aliyun.com/ubuntu/ xenial-security main
deb https://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb-src https://mirrors.aliyun.com/ubuntu/ xenial-security universe
```

### 小米4
根目录扩容
```shell
#!/bin/sh

# 此脚本会调整手机的根分区大小，请小心。

echo "调整根分区的大小，这可能需要一些时间。。。"
echo "完成后，它将重新启动设备，确保您没有丢失任何工作！"

cd /userdata
echo "创建新的磁盘映像 4096"
dd bs=1M count=6144 if=/dev/zero of=system2.img
#这可能需要一段时间, put the kettle on

#mount it as a loopback device
losetup -f --show system2.img
#copy from existing loopback to current one
dd if=/dev/loop0 of=/dev/loop2

echo "然后调整我们新设备上的文件系统大小。这可能会发出警告，接受它"
e2fsck -f /dev/loop2
resize2fs /dev/loop2

echo "现在把我们的新形象换成旧形象"
mv system.img system.old
mv system2.img system.img

reboot

echo "享受你的新 / space"
df -h
#清理旧文件
rm /userdata/system.old
```