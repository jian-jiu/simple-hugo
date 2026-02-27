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
"https://docker.amingg.com",
"https://docker.xuanyuan.me",
"https://docker.m.daocloud.io",
"https://registry.hub.docker.com",
"https://noohub.ru",
"https://huecker.io",
"https://dockerhub.timeweb.cloud",
"https://docker.rainbond.cc",
"https://dockerhub.icu",
"https://docker.chenby.cn",
"https://docker.awsl9527.cn",
"https://docker.anyhub.us.kg",
"https://dhub.kubesre.xyz",
"https://docker.m.ixdev.cn",
"https://dockerproxy.net",
"https://image.cloudlayer.icu",
"https://docker.13140521.xyz",
"https://docker.anye.in"
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