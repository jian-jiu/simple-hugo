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

## 系统
[官方下载](https://www.microsoft.com/zh-cn/software-download/windows11)
### windows系统重装工具
[rufus](https://rufus.ie/zh/)
微pe[官网](https://www.wepe.com.cn/)
我告诉你[官网](https://msdn.itellyou.cn/)
原版软件[官网](https://next.itellyou.cn/)

### windows镜像 business editions和consumer editions说明（推荐Business editions）
Consumer editions包括：家庭版、教育版、专业版；
Business editions包括：企业版、教育版、专业版；

## ntfs和fat32的区别（推荐ntfs）
[参考](https://zhuanlan.zhihu.com/p/44879302)

## mbr和guid的区别（推荐guid）
“MBR的意思是主引导记录，是传统分区类型，只支持一个硬盘上最多4个主分区，最大的缺点是不支持2TB以上硬盘容量，
而GUID就是新兴的GPT方式，支持的主分区数量没有限制；MBR支持win7版本系统以下的32位和64位，
而GUID支持win7版本以上的64位系统，不支持32位系统。”

## 命令行工具 PowerShell
快捷方式 wt
### 文件管理器默认打开当前
编辑JSON文件: 在 "profiles" 的 "default" 下添加 "startingDirectory": "."


## host 文件位置
```shell
C:\WINDOWS\system32\drivers\etc
```

## 查看文件占用情况 文件查看/搜索
[Everything](https://www.voidtools.com/zh-cn/downloads/)
[WinTree](https://www.diskanalyzer.com/download)
[Anytxt Searcher](https://anytxt.net/download/)

## cmd在开始菜单创建快捷方式
[参考](https://www.bilibili.com/opus/982804893816324112)

## 下载器
[https://www.zhihu.com/question/19820075](https://www.zhihu.com/question/19820075)

## 哈希值 md5
[不要再使用md5](https://zhuanlan.zhihu.com/p/27335023)
```shell
Get-FileHash 文件 md5(可选)
```

## hyper-v创建win11虚拟机
[参考](https://www.bilibili.com/video/BV11QBfBKEwQ)
网络不连接
安全 设置 启用受信任的平台模块
1. 启动按回车
2. 硬盘顺序可能要修改一下
 启动后提示要【操作】【ctrl alt delete】 然后要多等等

## 跳过验证
联网界面按快捷键 Shift+F10
输入 start ms-cxh:localonly 按回车

## 命令行编码
[参考](https://blog.csdn.net/qq_39715000/article/details/125853042)
```shell
# 查看
chcp

# utf8
chcp 65001

# gbk
chcp 936
```

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

## 查看指定端口是否占用
```shell
查看端口被哪个pid占用
netstat -aon|findstr 9090
查看名字
tasklist|findstr "9090"
杀死
taskkill /T /F /PID 9090
```
如果查询不到使用下面的方式
### 查看动态端口范围
```shell
netsh int ipv4 show dynamicport tcp
```
### 设置动态端口范围
```shell
netsh int ipv4 set dynamicportrange tcp start=49152 num=16384
```

## win脚本自动启动
### 添加文件
方式1--->直接打开 C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
方式2-->通过打开菜单页面某一个快捷方式，在进入启动文件夹
方式3-->通过任务管理器的启动页面打开文件地方
### 文件名 start.vbs
```vbs
set ws=wscript.createobject("wscript.shell")
ws.run "D:\Programme\must\minio\minio.bat",0
```

## cpu 超频
[参考](https://www.intel.com/content/www/us/en/download/17881/intel-extreme-tuning-utility-intel-xtu.html)

## nmap(网络测试工具)
```shell
nmap -Pn --script ssl-enum-ciphers -p 443 113.104.126.249
pause
```

## 同步文件
[Syncthing参考](https://www.cnblogs.com/jackadam/p/8568833.html#%E4%B8%89%EF%BC%9A%E5%9C%A8windows%E4%B8%AD%E5%AE%89%E8%A3%85)

## 远程观看
[SyncPlay 参考](https://blog.csdn.net/xjl456852/article/details/130517470)

## Aria2
[参考](https://blog.csdn.net/qq_55058006/article/details/115570993)
下载配置
/var/apps/aria2/shares/Download

## idm 下载器
[参考](https://blog.csdn.net/qq_61621323/article/details/141061544)

[参考](https://zhuanlan.zhihu.com/p/430535305)

## 压缩工具
[rar弹窗](https://zhuanlan.zhihu.com/p/680852417)
[推荐 7-zip](https://www.7-zip.org/download.html)
![参考](https://cdn.242499.xyz/2026/04/12/02d56296ead3a25d75a51c8cfcfe3c7e.jpeg)
