---
title: win电脑
date: 2026-03-10
slug: win
categories:
    - 电脑
tags:
    - 电脑
    - win
---

## 哈希值 md5
```shell
Get-FileHash 文件
```

## hyper-v创建win11虚拟机
安全 设置 启用受信任的平台模块
硬盘顺序可能要修改一下
启动后提示要【操作】【ctrl alt delete】 然后要多等等

## bat
bat 上传文件
```bat
pscp -v -r -p -pw 密码 ./redis.conf   root@106.55.239.122:/simple/redis/conf
@REM scp.exe -r ./. root@106.55.239.122:/simple/redis/conf
```

取参
%0 %1

加入 pause 可以不退出

if写法
```bat
if "%1%"=="redis" (
call ./redis/redis.bat
) else (
echo 11111111111
)
```
