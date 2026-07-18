---
title: redroid
date: 2026-05-18
slug: redroid
categories:
    - redroid
tags:
    - redroid
    - android
    - 手机
    - 模拟器
---

## 飞牛
https://club.fnnas.com/forum.php?mod=viewthread&tid=36386

## redroid-script
[面具](https://github.com/ayasa520/redroid-script)
```shell
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
python redroid.py -a 12.0.0 -gm
```

logcat

## root问题(好像是)
```shell
#!/system/bin/sh
mount -o remount,rw /system/xbin
chmod 755 /system/xbin
ln -sf /sbin/su /system/xbin/su
```

## 其他
[安卓16](https://github.com/WayDroid-ATV/waydroid-docker-builds)
