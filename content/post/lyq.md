---
title: 路由器
date: 2026-04-26
slug: lyq
categories:
    - 路由器
tags:
    - 路由器
---

[upnp和dmz到底要不要一起开](https://www.right.com.cn/forum/thread-8134132-1-1.html)

## h3-2s
### 桥接
宽带设置

新增一个

模式 桥模式

![参考](https://cdn.242499.xyz/2026/05/17/f9b391b10c9becf19b880dafd9abd6df.png)

### 光猫改桥接后op访问光猫后台的设置
https://www.bilibili.com/video/BV1F5411Q7Xp
https://zhuanlan.zhihu.com/p/1924973204142810446
创建接口 静态地址 192.168.1.x + 网关 关闭【默认网关】 有MAC就对了

[开启telnet](http://192.168.1.1/usr=CMCCAdmin&psw=aDm8H%25MdA&cmd=1&telnet.gch)

CMCCAdmin
user
Qqxxx@.
[参考](https://blog.csdn.net/l1677516854/article/details/136281150)
[参考](https://blog.csdn.net/cb20040401/article/details/128730222)
[参考](https://www.right.com.cn/forum/thread-7936266-1-1.html)

```shell
telnet 192.168.1.1

CMCCAdmin

aDm8H%MdA

sidbg 1 DB decry /userconfig/cfg/db_user_cfg.xml

vi /tmp/debug-decry-cfg

搜索 :/CMCCAdmin
```

[nat参考](https://blog.csdn.net/qq_46648437/article/details/113747066)

## istoreos
https://fw.koolcenter.com/

[passwall](https://shdvgj.github.io/2023/07/06/2023/07/bridge-mode-starter-istoreos)

### dns
smartdns

## MER1200G
[MER1200G刷第三方固件（不需编程器和UBOOT)连WIFI就可刷机](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=8226512&highlight=MER1200G)
[水星MER1200G刷完整版operwrt 思路](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=2323889&highlight=MER1200G)
[水星MAC1200R千兆版 刷机汇总](https://www.right.com.cn/forum/thread-8263049-1-1.html)
[刷回原厂固件 ttl](https://blog.csdn.net/weixin_44658419/article/details/123284833)
[刷机教程](https://blog.csdn.net/weixin_44658419/article/details/122634305)

## 虚拟机 软路由
https://worldcovert.com/不想买软路由硬件也想体验软路由-教你如何在虚-2
