---
title: docker
slug: docker
date: 2026-02-26
categories:
    - docker
tags:
    - docker
---

## 下载慢
[地址1](https://status.1panel.top/)
[地址2](https://status.anye.xyz/)

[获取地址 Portainer](https://www.portainer.cn/)

# 编辑
sudo vi /etc/docker/daemon.json
```shell
{
"registry-mirrors": [
"https://docker.fnnas.com",
"https://docker.1panel.live",
"https://docker.1ms.run",
"https://hub1.nat.tf",
"https://hub2.nat.tf",
"https://hub.rat.dev",
"https://docker.anye.in",
"https://docker.chenby.cn",
"https://docker.m.ixdev.cn",
"https://docker.amingg.com",
"https://docker.rainbond.cc",
"https://docker.xuanyuan.me",
"https://docker.awsl9527.cn",
"https://docker.anyhub.us.kg",
"https://docker.13140521.xyz",
"https://docker.m.daocloud.io",
"https://dockerhub.icu",
"https://dockerhub.timeweb.cloud",
"https://noohub.ru",
"https://huecker.io",
"https://dockerproxy.net",
"https://dhub.kubesre.xyz",
"https://image.cloudlayer.icu",
"https://registry.hub.docker.com"
]
}
```

完成编辑后，重新加载 daemon.json 文件并重启 Docker：

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```


## 可视化管理面板
[dpanel](https://dpanel.cc/)

[Portainer](https://www.portainer.cn/)

wud 太复杂了

## 应用推荐

### 小雅
[参考](https://hub.docker.com/r/xiaoyaliu/alist)

[参考](https://github.com/xiaoyaDev/xiaoya-alist/issues/312)

	# Docker介绍
[docker官网](https://www.docker.com/) [docker官方镜像](https://hub.docker.com/) [阿里云docker镜像](https://cr.console.aliyun.com/)
>Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。
>Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
>容器是完全使用沙箱机制，相互之间不会有任何接口（类似 手机 的 app）,更重要的是容器性能开销极低

# win汉化
[推荐](https://github.com/asxez/DockerDesktop-CN)
# 配置
```json
buildkit 并行构建、缓存优化
containerd-snapshotter 断点续传
{
  "features": {
    "buildkit": true,
    "containerd-snapshotter": true
  },
  "log-driver": "json-file",
  "log-opts": {
    "max-file": "3",
    "max-size": "500m"
  },
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    
https://docker.fnnas.com
https://docker.1panel.live
https://docker.m.daocloud.io
https://docker.1ms.run
https://hub1.nat.tf
https://hub2.nat.tf
https://hub.rat.dev
https://docker.amingg.com
https://docker.xuanyuan.me

  ]
}
```
# Ubuntu
[在 Ubuntu 上安装 Docker Engine](https://docs.docker.com/engine/install/ubuntu/)
[下载文件地址](https://download.docker.com/linux/ubuntu/dists/xenial/pool/stable/armhf/)
# D-Bus 问题
[参考](https://blog.csdn.net/u013084266/article/details/125116798)
```shell
# 替换 重启
curl https://raw.githubusercontent.com/gdraheim/docker-systemctl-replacement/master/files/docker/systemctl.py > /usr/bin/systemctl
chmod +x /usr/bin/systemctl
```
# 安装Docker
[Ubuntu参考](https://zhuanlan.zhihu.com/p/143156163)
## centos
```shell
sudo yum -y install yum-utils device-mapper-persistent-data lvm2 gcc gcc-c++

# 可以跳过
yum -y install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.10-3.2.fc30.x86_64.rpm
yum -y install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.13-3.2.fc30.x86_64.rpm

# 配置阿里云仓库
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

yum -y install docker-ce 

# 创建docker镜像源
vim /etc/docker/daemon.json
{
看上面
  	"data-root":"/data/docker"
}
[参考](https://www.cnblogs.com/zinan/p/11284236.html)
[官方文档](https://docs.docker.com/config/containers/logging/configure/)

# 重启docker守护进程

systemctl daemon-reload

systemctl restart docker

```
## ubuntu
```shell
# 更新现有的软件包列表
apt update
# 安装一些必备软件包，让 apt 通过 HTTPS 使用软件包
apt install apt-transport-https ca-certificates curl software-properties-common
# 将官方 Docker 版本库的 GPG 密钥添加到系统中
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# Docker 版本库添加到APT源
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
# 用新添加的 Docker 软件包来进行升级更新
apt update
# 确保要从 Docker 版本库，而不是默认的 Ubuntu 版本库进行安装
apt-cache policy docker-ce
# 安装 Docker
apt install docker-ce
```
# 基本操作
和[linux服务命令](https://blog.csdn.net/weixin_44193021/article/details/118966577)一样
如: systemctl status docker ...
| 操作 | 命令 |
|--|--|
| 查看版本号信息 | docker -v |
| 查看版本号信息（详细） | docker version |
| 查看详细信息 | docker info |
# 镜像操作
| 命令 | 操作 |
|--|--|
| 查看所有镜像 | docker images |
| 拉取镜像 | docker pull 镜像标识:版本号 |
| 删除镜像 | docker rmi 镜像标识:版本号 |
| 删除所有的镜像 | docker rmi $(docker images -aq) |

run 命令

- -d ： 后台运行容器，并返回容器ID
- -i ：  以交互模式运行容器，通常与 -t 同时使用
- -t ： 为容器重新分配一个伪输入终端，通常与 -i 同时使用
- --name="xxx" ： 为容器指定一个名称
- -P ： 随机端口映射，容器内部端口随机映射到主机的高端口
- -p ： 指定端口映射（如 [ -p 宿主机端口:容器端口]）
- -e username="xxx" ： 设置环境变量
- -m ： 设置容器使用内存最大值
- --volume , -v ： 绑定一个卷（如 [ -v 宿主机目录:/容器目录 ]）
- --net ： 配置网络模式（如 --net brigge ）

# 容器操作
基本命令 docker start | stop | restart | rm 容器标识
参数说明
| 操作 | 命令 |
|--|--|
| -i | 运行容器 |
| -t | 以交互式的方式运行容器，会进入容器的命令行内部 |
| -d | 以守护式的方式运行容器，在后台运行 |
| -p | 端口映射 |
| -v|目录挂载，将宿主机的目录映射到容器的目录上 |

查看所有容器
docker ps -a
删除容器
docker rm id/名称

exec 命令
- -d ：分离模式: 在后台运行
- -i ： 即使未连接，也保持标准输入打开
- -t ： 分配一个伪终端

# 文件拷贝
- 将宿主机的文件拷贝到容器中 ： docker cp  宿主机文件路径 镜像标识:容器路径
 ```shell
如 ： docker cp /etc/docker/nginx/nginx.config nginx:/etc/nginx/
```
- 将容器中的文件拷贝到宿主机上 ： docker cp 镜像标识:容器文件路径 宿主机路径
```shell
如 ： docker cp nginx:/etc/nginx/nginx.config /etc/docker/nginx/
```

# 查看容器映射路径
```
docker inspect container_name | grep Mounts -A 20
docker inspect container_id | grep Mounts -A 20
```
# 日志
docker logs 参数 镜像标识
参数
- --since : 此参数指定了输出日志开始日期，即只输出指定日期之后的日志
  （如 --since=“2020-10-10” 只输出（包含）2020-10-10后的日志）
- -f : 查看实时日志
- -t : 查看日志产生的日期
- -tail=10 : 查看最后的10条日志

```shell
#!/bin/sh

echo "======== start clean docker containers logs ========"

logs=$(find /var/lib/docker/containers/ -name *-json.log)

for log in $logs
do
echo "clean logs : $log"
cat /dev/null > $log
done

echo "======== end clean docker containers logs ========"
```
# 网络
## 网络模式
- bridge ： 桥接（默认）
- none ：  不配置网络
- host ： 和宿主机共享
- container ： 容器网络连通（用的少）
## 创建网络
### 参数
- --driver 使用的网络模式
- --subnet 子网
- --gateway 网关
```shell
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
```
## 本文特殊描述
- 镜像标识
  如: nginx redis ... 等等
- 版本号
  如:1.0.0 2.2.2 ... 不指定版本号为最新发布版本

## 远程安全登录
飞牛nas /etc/systemd/system/docker.service
```shell
#!/bin/bash

sed -i '8s/$/ -H tcp:\/\/0.0.0.0:2375/' /etc/systemd/system/docker.service

systemctl daemon-reload 

systemctl restart docker 

echo "完成"

```
配置文件可能在 /lib /uer /etc
[参考文档1](https://www.cnblogs.com/-wenli/p/13555833.html)
[参考文档2](https://blog.csdn.net/qq_40298902/article/details/106543208)
```shell
#!/bin/bash
# -------------------------------------------------------------
# 自动创建 Docker TLS 证书
# sed -i 's/\r$//' xxx.sh // 修复命令
# -------------------------------------------------------------

# --[BEGIN]------------------------------
# 代码，可以随便写
CODE="WRETCHANT"
# 服务器的外网IP
IP="xxx"
# CA证书的密码
PASSWORD="xxx"
# 国家
COUNTRY="CN"
# 地区
STATE="guangdong"
# 城市
CITY="guangzhou"
# 组织
ORGANIZATION="WRETCHANT.EDU"
# 组织单元
ORGANIZATIONAL_UNIT="Dev"
# 通用名称
COMMON_NAME="$IP"
# 邮件地址
EMAIL="xxx@gmail.com"

# --[END]--

# 创建存放脚本的文件夹
mkdir -p /simple/docker/ca/
cd /simple/docker/ca/ || exit

# 生成 CA 密钥
openssl genrsa -aes256 -passout "pass:$PASSWORD" -out "ca-key.pem" 4096
# 生成 CA
openssl req -new -x509 -days 365 -key "ca-key.pem" -sha256 -out "ca.pem" -passin "pass:$PASSWORD" -subj "/C=$COUNTRY/ST=$STATE/L=$CITY/O=$ORGANIZATION/OU=$ORGANIZATIONAL_UNIT/CN=$COMMON_NAME/emailAddress=$EMAIL"
# 生成服务器密钥
openssl genrsa -out "server-key.pem" 4096

# 生成服务器证书
openssl req -subj "/CN=$COMMON_NAME" -sha256 -new -key "server-key.pem" -out server.csr

echo "subjectAltName = IP:$IP,IP:127.0.0.1" >>extfile.cnf
echo "extendedKeyUsage = serverAuth" >>extfile.cnf

openssl x509 -req -days 365 -sha256 -in server.csr -passin "pass:$PASSWORD" -CA "ca.pem" -CAkey "ca-key.pem" -CAcreateserial -out "server-cert.pem" -extfile extfile.cnf

# 生成客户端证书
rm -f extfile.cnf

openssl genrsa -out "key.pem" 4096
openssl req -subj '/CN=client' -new -key "key.pem" -out client.csr
echo extendedKeyUsage = clientAuth >>extfile.cnf
openssl x509 -req -days 365 -sha256 -in client.csr -passin "pass:$PASSWORD" -CA "ca.pem" -CAkey "ca-key.pem" -CAcreateserial -out "cert.pem" -extfile extfile.cnf

rm -vf client.csr server.csr extfile.cnf

chmod -v 0400 "ca-key.pem" "key.pem" "server-key.pem"
chmod -v 0444 "ca.pem" "server-cert.pem" "cert.pem"

# ca.pem ca-key.pem cert.pem key.pem 为客户端证书
# /lib/systemd/system/docker.service
# ExecStart=/usr/bin/dockerd --tlsverify --tlscacert=/simple/docker/ca/ca.pem --tlscert=/simple/docker/ca/server-cert.pem --tlskey=/simple/docker/ca/server-key.pem -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock

# systemctl daemon-reload

# systemctl restart docker

# ca.pem server-cert.pem server-key.pem 为服务器证书

# ca.pem ca-key.pem cert.pem key.pem 为客户端证书
```
## docker-compose命令
[详情](https://www.jianshu.com/p/c51d92a9f91d)

安装
```shell
curl -L https://get.daocloud.io/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose version
```
