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
