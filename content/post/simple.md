---
title: simple
date: 2026-03-07
slug: simple
categories:
  - simple
tags:
  - 简单
  - simple
---

## minio rustfs
https://juejin.cn/post/7523256725127987226

[官网](https://docs.min.io/)

# win自动启动
minio.bat
```bat
@echo off
cd /d %~dp0 # 跳转到当前脚本的目录
.\minio.exe server .\
# pause
```
[启动文件](https://editor.csdn.net/md/?articleId=126314536)

[访问链接](http://127.0.0.1:9090)
密码：minioadmin | minioadmin

启动 .\minio.exe server C:\minio --console-address :9000


## 版本区别
Alpha、Beta、RC、GA版本的区别
[参考](https://www.jianshu.com/p/d69226decbfe)

## 记笔记
[typora](https://www.typora.io/)
[xmind （思维导图）](https://www.xmind.cn/)
### 在线笔记
[有道云](https://note.youdao.com/)
[印象](https://yinxiang.com/)
[processon （思维导图）](https://processon.com/)

## web hutool 文档生成框架
[docsify官网](https://docsify.js.org/#/)

## Shell编码规范手册(shellcheck错误汇总)
[详情](https://blog.csdn.net/qq_38123721/article/details/114117300)

## 999感冒灵
睡觉 助眠
扑尔敏(抗过敏) 副作用(瞌睡)

## 电线（test）
硬芯国标铜导线（单股）线经
- 1.5平方 1.38mm
- 2.5平方 1.78mm
- 4平方 2.25mm
- 6平方 2.76mm
- 10平方 多为7股绞合（单1.35mm）

线径是导体直径 国标GB/T 3956-2008 允许0.02微小公差

## 羽绒服(test)
看 克罗值（test）
c棉(老美) -> 火箭棉 ~> 金标p棉

## ssh升级

解压 tar -zxvf openssh-9.1p1.tar.gz

备份配置文件
cp /etc/ssh/sshd_config sshd_config.backup
cp /etc/pam.d/sshd sshd.backup

卸载原来的
rpm -e --nodeps `rpm -qa | grep openssh`

编译配置
./configure --prefix=/usr --sysconfdir=/etc/ssh --with-md5-passwords --with-pam --with-zlib --with-tcp-wrappers --with-ssl-dir=/usr/local/ssl --without-hardening

编译安装
make && make install

调整文件权限
chmod 600 /etc/ssh/ssh_host_rsa_key /etc/ssh/ssh_host_ecdsa_key /etc/ssh/ssh_host_ed25519_key

复制配置文件
cp -a contrib/redhat/sshd.init /etc/init.d/sshd
chmod u+x /etc/init.d/sshd
mv ../sshd.backup /etc/pam.d/sshd
mv ../sshd_config.backup /etc/ssh/sshd_config

添加自启服务ssh到开机启动项
chkconfig --add sshd
chkconfig sshd on

重启sshd服务
systemctl restart sshd

查看最新版本
ssh -V


OpenSSL

tar -xf openssl-1.1.1j.tar.gz
cd openssl-1.1.1j
./config --prefix=/usr/local/openssl shared
make
make install
安装OpenSSL成功
vi /etc/ld.so.conf
添加一行：
/usr/local/openssl/lib
保存后执行：
ldconfig

## 计算机基础
### 一：与运算符（&）

预算规则：

0&0=0；0&1=0；1&0=0；1&1=1

即：两个同时为1，结果为1，否则为0

例如：3&5

十进制3转为二进制的3：0000 0011

十进制5转为二进制的5：0000 0101

------------------------结果：0000 0001 ->转为十进制：1

即：3&5 = 1

### 二：或运算（|）

运算规则：

0|0=0； 0|1=1； 1|0=1； 1|1=1；

即 ：参加运算的两个对象，一个为1，其值为1。

例如：3|5　即 00000011 | 0000 0101 = 00000111，因此，3|5=7。　

### 三：异或运算符（^）

运算规则：0^0=0； 0^1=1； 1^0=1； 1^1=0；

即：参加运算的两个对象，如果两个位为“异”（值不同），则该位结果为1，否则为0。

例如：3^5 = 0000 0011 | 0000 0101 =0000 0110，因此，3^5 = 6

## 菜板推荐
银杏木 > 杨树

## 区块链
[参考](https://juejin.cn/post/7049718389836611597)
