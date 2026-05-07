---
title: wsl虚拟机
date: 2026-05-05
slug: wsl
categories:
    - wsl
tags:
    - wsl
    - 虚拟机
---

```shell
# 可以安装的
wsl --list --online

# 查看子系统状态
wsl -l -v

# 卸载
wsl --list
wsl --unregister Ubuntu-20.04
```

## win
https://dangzitou.github.io/posts/windows11-install-wsl-ubuntu-24-04

[下载](https://ubuntu.com/desktop/wsl)
https://releases.ubuntu.com/20.04/
解压  加 .tar
wsl --import Ubuntu-20.04 F:\wsl\Ubuntu-20.04 F:\wsl\ubuntu-20.04.6-wsl-amd64.tar

```shell
# 备份
wsl --export Ubuntu-20.04 F:\wsl\bak\Ubuntu-20.04.tar

# 还原
wsl --import Ubuntu-20.04 F:\wsl\Ubuntu-20.04 F:\wsl\bak\Ubuntu-20.04.tar
```
