
## h3-2s
### 桥接
宽带设置

新增一个

模式 桥模式

### 光猫改桥接后op访问光猫后台的设置
https://www.bilibili.com/video/BV1F5411Q7Xp
https://zhuanlan.zhihu.com/p/1924973204142810446
创建接口 静态地址 192.168.1.x + 网关 关闭【默认网关】 有MAC就对了

[http://192.168.1.1/usr=CMCCAdmin&psw=aDm8H%25MdA&cmd=1&telnet.gch](http://192.168.1.1/usr=CMCCAdmin&psw=aDm8H%25MdA&cmd=1&telnet.gch)

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
### dns
smartdns