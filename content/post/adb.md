---
title: adb
date: 2026-05-18
slug: adb
categories:
    - adb
tags:
    - adb
    - android
    - 手机
---

```shell
# 设备列表
adb devices

# 连接设备
adb connect xxx:5555

# 安装应用
adb -s <设备编号> install xxx.apk

# 上传文件
adb push <本地文件路径> /sdcard/Download/
```

## scrcpy
https://github.com/Genymobile/scrcpy
./scrcpy.exe --tcpip=192.168.2.33:5555  --no-audio  --max-size 1080 --video-bit-rate=2M
