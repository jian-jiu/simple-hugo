---
title: linux
date: 2026-03-09
slug: linux
categories:
    - linux
tags:
    - linux
---

## 定时器
[详细介绍](https://www.cnblogs.com/guoew/p/11453231.html)

## emmc
```shell
var="$(cat /sys/kernel/debug/mmc0/mmc0\:0001/ext_csd)"
eol=${var:534:2};slc=${var:536:2};mlc=${var:538:2}
echo "EOL:0x$eol SLC:0x$slc MLC:0x$mlc"
```

## 流量监控
### iftop
[参考](https://geekdaxue.co/read/xingqi8-n5too@wnuhxf/hhmsyn)

## Debian armbian

# 不同的版本名称
Debian 11        Bullseye
Debian 12        Bookworm       
Ubuntu 22.04 LTS Jammy Jellyfish
Ubuntu 24.04 LTS Noble Numbat

# 换Debian源

## 命令
```shell
armbian-sync
armbian-apt
```

```shell
/etc/apt/sources.list
```
```shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
# deb-src https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware

deb http://mirrors.tuna.tsinghua.edu.cn/armbian/ buster main buster-utils buster-desktop
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security/ bullseye-security main contrib non-free



#清华
 
deb https://mirrors.ustc.edu.cn/debian/ bullseye main non-free contrib
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye main non-free contrib
 
deb https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main
deb-src https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main
 
deb https://mirrors.ustc.edu.cn/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-updates main non-free contrib
 
deb https://mirrors.ustc.edu.cn/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-backports main non-free contrib
 
#阿里源
 
deb https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
#deb-src https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
 
deb https://mirrors.aliyun.com/debian-security/ bullseye-security main
#deb-src https://mirrors.aliyun.com/debian-security/ bullseye-security main
 
deb https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
#deb-src https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
 
deb https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
#deb-src https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib


Armbian默认

deb http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware
#deb-src http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware

deb http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware
#deb-src http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware

deb http://deb.debian.org/debian bookworm-backports main contrib non-free non-free-firmware
#deb-src http://deb.debian.org/debian bookworm-backports main contrib non-free non-free-firmware

deb http://security.debian.org/ bookworm-security main contrib non-free non-free-firmware
#deb-src http://security.debian.org/ bookworm-security main contrib non-free non-free-firmware

# 中科大
deb https://mirrors.ustc.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
deb-src https://mirrors.ustc.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
deb https://mirrors.ustc.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
deb-src https://mirrors.ustc.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
deb https://mirrors.ustc.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
deb-src https://mirrors.ustc.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
deb https://mirrors.ustc.edu.cn/debian-security/ bookworm-security main contrib non-free non-free-firmware
deb-src https://mirrors.ustc.edu.cn/debian-security/ bookworm-security main contrib non-free non-free-firmware

# 清华
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main non-free non-free-firmware contrib
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main non-free non-free-firmware contrib
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security/ bookworm-security main
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security/ bookworm-security main
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main non-free non-free-firmware contrib
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main non-free non-free-firmware contrib
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main non-free non-free-firmware contrib
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main non-free non-free-firmware contrib

# 阿里
deb https://mirrors.aliyun.com/debian/ bookworm main non-free non-free-firmware contrib
deb-src https://mirrors.aliyun.com/debian/ bookworm main non-free non-free-firmware contrib
deb https://mirrors.aliyun.com/debian-security/ bookworm-security main
deb-src https://mirrors.aliyun.com/debian-security/ bookworm-security main
deb https://mirrors.aliyun.com/debian/ bookworm-updates main non-free non-free-firmware contrib
deb-src https://mirrors.aliyun.com/debian/ bookworm-updates main non-free non-free-firmware contrib
deb https://mirrors.aliyun.com/debian/ bookworm-backports main non-free non-free-firmware contrib
deb-src https://mirrors.aliyun.com/debian/ bookworm-backports main non-free non-free-firmware contrib

# 腾讯
deb https://mirrors.tencent.com/debian/ bookworm main non-free non-free-firmware contrib
deb-src https://mirrors.tencent.com/debian/ bookworm main non-free non-free-firmware contrib
deb https://mirrors.tencent.com/debian-security/ bookworm-security main
deb-src https://mirrors.tencent.com/debian-security/ bookworm-security main
deb https://mirrors.tencent.com/debian/ bookworm-updates main non-free non-free-firmware contrib
deb-src https://mirrors.tencent.com/debian/ bookworm-updates main non-free non-free-firmware contrib
deb https://mirrors.tencent.com/debian/ bookworm-backports main non-free non-free-firmware contrib
deb-src https://mirrors.tencent.com/debian/ bookworm-backports main non-free non-free-firmware contrib

# 网易
deb https://mirrors.163.com/debian/ bookworm main non-free non-free-firmware contrib
deb-src https://mirrors.163.com/debian/ bookworm main non-free non-free-firmware contrib
deb https://mirrors.163.com/debian-security/ bookworm-security main
deb-src https://mirrors.163.com/debian-security/ bookworm-security main
deb https://mirrors.163.com/debian/ bookworm-updates main non-free non-free-firmware contrib
deb-src https://mirrors.163.com/debian/ bookworm-updates main non-free non-free-firmware contrib
deb https://mirrors.163.com/debian/ bookworm-backports main non-free non-free-firmware contrib
deb-src https://mirrors.163.com/debian/ bookworm-backports main non-free non-free-firmware contrib

# 华为
deb https://mirrors.huaweicloud.com/debian/ bookworm main non-free non-free-firmware contrib
deb-src https://mirrors.huaweicloud.com/debian/ bookworm main non-free non-free-firmware contrib
deb https://mirrors.huaweicloud.com/debian-security/ bookworm-security main
deb-src https://mirrors.huaweicloud.com/debian-security/ bookworm-security main
deb https://mirrors.huaweicloud.com/debian/ bookworm-updates main non-free non-free-firmware contrib
deb-src https://mirrors.huaweicloud.com/debian/ bookworm-updates main non-free non-free-firmware contrib
deb https://mirrors.huaweicloud.com/debian/ bookworm-backports main non-free non-free-firmware contrib
deb-src https://mirrors.huaweicloud.com/debian/ bookworm-backports main non-free non-free-firmware contrib
```

# 更新

apt-get update && apt-get upgrade

错误 The following signatures couldn't be verified because the public key is not available
```shell
6ED0E7B82643E131
54404762BBB6E853
93D6889F9F0E78D5
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5523BAEEB01FA116 其中的5523BAEEB01FA116是根据错误提示写的
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys
```

trusted.gpg问题
```shell
cd /etc/apt
sudo cp trusted.gpg trusted.gpg.d

sudo cp /etc/apt/trusted.gpg /etc/apt/trusted.gpg.d
```

# No sandbox user '_apt' on the system, can not drop privileges
[参考](https://askubuntu.com/questions/882039/no-sandbox-user-apt-on-the-system-can-not-drop-privileges)
```shell
sudo adduser --force-badname --system --no-create-home _apt
```

## 交换区设置
[参考](https://blog.donothing.site/2019/11/16/linux-swap/)

## 查大文件
```
 find / -type f -size +1G
```

## 时区
```shell
# 查看
timedatectl
# 设置
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
## 服务命令
### 防火墙
[参考文档](https://www.php.cn/linux-443556.html)
## service
命令只支持基础 LSB 动作
start（启动）
stop（停止）
restart（重启）
try-restart(尝试重新启动)
reload(重新加载)、
force-reload(强制重新加载)、
status（状态）
如: service firewalld status （查看防火墙状态，注意顺序: service 服务 状态）
### systemctl
和service拥有一样的命令
如: systemctl status firewalld （查看防火墙状态，注意顺序: systemctl 状态 服务）


## 清空文件
[参考](https://blog.csdn.net/pi9nc/article/details/18257593)
```shell
cat /dev/null > /var/log/messages
推荐下面的
: > /var/log/messages
```

## 压缩解压缩

J(大写):使用压缩 .xz

z:使用压缩  .gz

c:创建

v:显示

f:指定文件

x:解压缩

压缩文件 tar -zcvf 名字.tar.gz 1~n个文件

解压文件 tar -zxvf  文件 -C 地址

## 动态输出日志

tail -f 文件

##### 全局配置颜色：

	打开配置文件：
	sudo vim ~/.bashrc
	
	在最后加一行：
	PS1='[\[\e[33;40m\]\u@\h \w \t]$ '
	
	然后使之生效：
	source ~/.bashrc
## [快速查找文件](https://blog.csdn.net/xxmonstor/article/details/80507769)

修改文件权限
修改文件权限使用chmod指令。该指令常用的有两种使用方式：

1.chmod abc filename

指令中的a、b、c分别表示一个数字，其中a对应文件所有者权限，b对应文件所有者所在组权限，c对应其他身份权限。

对于a、b、c各自来讲，它们都是0~7的数字，对应r、w、x三个二进制位按序组成的二进制数，举个例子，如果是只可读，对应的二进制数就是“100”，也就是4；如果是可读可写不可执行，那么对应二进制数为“110”，也就是6……

再举个最常见的chmod 777 xxxx指令，这里有3个7，但是每个7的含义是不同的。7的二进制形式为111，表示可读可写可执行，第1个7表示文件对于文件所有者来说可读可写可执行；第2个7表示文件对于文件所有者所在组来说可读可写可执行；第3个7表示文件对于其他身份的用户来说可读可写可执行。也就是说，通过chmod 777，文件就没有了读写执行权限限制了。

如果我要将上述client.cpp文件权限改为“文件所有者可读可写可执行，其余身份只可读”，那么就可以使用如下指令：