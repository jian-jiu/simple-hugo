---
title: nas
description: 飞牛nas
date: 2026-03-29
slug: nas
categories:
    - nas
tags:
    - nas
    - 飞牛
---

使用的是【apt】包管理器

## wifi
```shell
#!/bin/bash

ping -c 1 "8.8.8.8" >/dev/null 2>&1

if [ $? -eq 0 ]; then
    echo "ok"
else
    echo "fail"
    nmcli device wifi connect "CMCC-888" password "18200723687"
fi
```

## wifi 省电模式
```shell
apt install iw
ifconfig
iw dev wlp2s0 get power_save
```

创建文件 /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
```shell
[connection]
wifi.powersave = 2
```

### 永久关闭???
```shell
nmcli radio wifi
nmcli connection show xxx
```

## 笔记本关屏不休眠

[参考](https://zhuanlan.zhihu.com/p/1926745250128429307)
[参考](https://blog.csdn.net/qq_45797625/article/details/146341824)

## 第三方商店
```config
ignore 忽略
# 电池供电时合盖
HandleLidSwitch=ignore
# 外接电源时合盖
HandleLidSwitchExternalPower=ignore
# 连接扩展坞时合盖
HandleLidSwitchDocked=ignore

LidSwitchIgnoreInhibited=yes
```

## docker问题
```shell
#!/bin/bash

sed -i '10s/$/ -H tcp:\/\/0.0.0.0:2375/' /etc/systemd/system/docker.service
sed -i '11s/^/#/' /etc/systemd/system/docker.service

systemctl daemon-reload 

systemctl restart docker 

echo "ok"
```

## 蓝牙
```shell
# 进入bluetooth命令行交互模式
bluetoothctl
# 列出设备及其mac地址
devices
exit
```