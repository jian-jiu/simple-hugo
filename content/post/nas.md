---
title: nas
description: 飞牛nas
date: 2026-03-29
slug: nas
categories:
    - nas
tags:
    - nas
    - 飞牛
---

使用的是【apt】包管理器

## wifi
```shell
#!/bin/bash

ping -c 1 "8.8.8.8" >/dev/null 2>&1

if [ $? -eq 0 ]; then
    echo "ok"
else
    echo "fail"
    nmcli device wifi connect "CMCC-888" password "18200723687"
fi
```

## wifi 省电模式
```shell
apt install iw
ifconfig
iw dev wlp2s0 get power_save
```

创建文件 /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
```shell
[connection]
wifi.powersave = 2
```

### 永久关闭???
```shell
nmcli radio wifi
nmcli connection show xxx
```

## 笔记本关屏不休眠

[参考](https://zhuanlan.zhihu.com/p/1926745250128429307)
[参考](https://blog.csdn.net/qq_45797625/article/details/146341824)

## 第三方商店
```config
ignore 忽略
# 电池供电时合盖
HandleLidSwitch=ignore
# 外接电源时合盖
HandleLidSwitchExternalPower=ignore
# 连接扩展坞时合盖
HandleLidSwitchDocked=ignore

LidSwitchIgnoreInhibited=yes
```

## docker问题
```shell
#!/bin/bash

sed -i '10s/$/ -H tcp:\/\/0.0.0.0:2375/' /etc/systemd/system/docker.service
sed -i '11s/^/#/' /etc/systemd/system/docker.service

systemctl daemon-reload 

systemctl restart docker 

echo "ok"
```

## 蓝牙
```shell
# 进入bluetooth命令行交互模式
bluetoothctl
# 列出设备及其mac地址
devices
exit
```

## Nas
[NAS媒体库自动化管理工具](https://github.com/jxxghp/MoviePilot)
[搭建NASToolS](https://club.fnnas.com/forum.php?mod=viewthread&tid=10781)
[docker部署Clash与yacd](https://club.fnnas.com/forum.php?mod=viewthread&tid=11682&highlight=)
[图形化V2Ray面板搭建透明代理服务](https://club.fnnas.com/forum.php?mod=viewthread&tid=33301&highlight=)
[群晖docker安装dreamacro/clash + haishanh/yacd面板](https://swihp.cn/?id=1432)

## iStoreOS
[passwall](https://shdvgj.github.io/2023/07/06/2023/07/bridge-mode-starter-istoreos)

## 青龙面板1
[参考](https://blog.csdn.net/m0_51309390/article/details/124447642)
[库 参考](https://git.metauniverse-cn.com/https://github.com/shufflewzc/faker3.git)

jd的直接用源代码下面的命令拉取

```shell

docker run -dit -v D:/Docker/ql:/ql/data-p 5700:5700 --name qinglong --hostname qinglong -e TZ=Asia/Shanghai --restart unless-stopped whyour/qinglong:latest

```

一键安装依赖
[仓库](https://github.com/FlechazoPh/QLDependency)
```sh
curl -fsSL https://ghfast.top/https://raw.githubusercontent.com/FlechazoPh/QLDependency/main/Shell/QLOneKeyDependency.sh | bash

ql raw https://ghfast.top/https://raw.githubusercontent.com/FlechazoPh/QLDependency/main/Shell/QLOneKeyDependency.sh

got问题  nodejs 需要依赖 got@11
```

# 通知
变量 jd_CheckCK_notify

脚本管理 项目库目录下的 sendNotify.js
```js
// =======================================自定义通知设置区域===========================================
const push_config = {
    WEBHOOK_URL: 'http://172.18.0.5:3000/send_private_msg', // 自定义通知 请求地址
    WEBHOOK_BODY: 'message:$title$content\nuser_id:2424585654', // 自定义通知 请求体
    WEBHOOK_HEADERS: '', // 自定义通知 请求头
    WEBHOOK_METHOD: 'POST', // 自定义通知 请求方法
    WEBHOOK_CONTENT_TYPE: 'application/json', // 自定义通知 content-type
}

        const body = parseBody(WEBHOOK_BODY, WEBHOOK_CONTENT_TYPE, (v) =>
            v
                ?.replaceAll('$title', text?.replaceAll('\n', '\\n') + '\n')
                ?.replaceAll('$content', desp),
        );
```
需要函数 webhookNotify parseString parseHeaders parseBody formatBodyFun 青龙目录下有

[自动获取参考](https://jshook.org/)
```
命令
ql raw https://ghfast.top/https://raw.githubusercontent.com/FlechazoPh/QLDependency/main/Shell/QLOneKeyDependency.sh

jd2
https://git.metauniverse-cn.com/https://github.com/shufflewzc/faker2.git

环境变量
JD_COOKIE
```

[参考](https://gist.github.com/J3n5en/34e122cec18930983273a740025c63d8)
```js
// 仓库地址 https://gist.github.com/J3n5en/34e122cec18930983273a740025c63d8/revisions
// frida -U -F -l Frida_JD_COOKIE.js
// 或者使用jshook注入

//青龙地址 结尾不含/
// const BASE_URL = "http://127.0.0.1"
const BASE_URL = "http://192.168.1.99:5700"
// 青龙面板 - 系统设置 - 应用设置 中生成
const CLIENT_ID = "_NC9budHEHOZ"
const CLIENT_SECRET = "A8nFOshKR1TnsEaPX9rmE-Et"


const toast = (text) => {
    Java.scheduleOnMainThread(function () {
        var toast = Java.use("android.widget.Toast");
        toast.makeText(Java.use("android.app.ActivityThread").currentApplication().getApplicationContext(), Java.use("java.lang.String").$new(text), 1).show();
    });
}


const updateWSCK = (value) => {
    try {
        const OkHttpClient = Java.use("okhttp3.OkHttpClient").$new()
        const RequestBuilder = Java.use("okhttp3.Request$Builder")
        const RequestBody = Java.use("okhttp3.RequestBody");
        const MediaType = Java.use("okhttp3.MediaType");

        const loginResp = OkHttpClient.newCall(
            RequestBuilder.$new().url(`${BASE_URL}/open/auth/token?client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}`).build()
        ).execute().body().string();
        const token = JSON.parse(loginResp).data.token

        const envsResp = OkHttpClient.newCall(
            RequestBuilder.$new().url(`${BASE_URL}/open/envs`).addHeader("Authorization", `Bearer ${token}`).build()
        ).execute().body().string();

        const cookie = JSON.parse(envsResp).data?.find(env => env.name === "JD_COOKIE")

        const updateEnvsResp = OkHttpClient.newCall(
            RequestBuilder.$new().url(`${BASE_URL}/open/envs`)
                .method("PUT", RequestBody.create(MediaType.parse("application/json"), JSON.stringify({
                    "value": value,
                    "name": cookie.name,
                    "id": cookie.id
                })))
                .addHeader("Authorization", `Bearer ${token}`).build()
        ).execute().body().string();

        const enableEnvsResp = OkHttpClient.newCall(
            RequestBuilder.$new().url(`${BASE_URL}/open/envs/enable`)
                .method("PUT", RequestBody.create(MediaType.parse("application/json"), JSON.stringify([cookie.id])))
                .addHeader("Authorization", `Bearer ${token}`).build()
        ).execute().body().string();


        return JSON.parse(updateEnvsResp).code === 200 && JSON.parse(enableEnvsResp).code === 200
    } catch (error) {
        return false
    }
}

const hookJava = () => Java.perform(function () {
    toast("开始")
    const cm = Java.use("com.tencent.smtt.sdk.CookieManager")
    console.log(cm);
    console.log(cm.getInstance());
    console.log(cm.getInstance().acceptCookie());

    let cookie = cm.getInstance().getCookie("https://m.jd.com")
    console.log("https://m.jd.com");
    console.log(cookie);
    if(cookie) {
        cookie = cm.getInstance().getCookie("http://m.jd.com")
        console.log("https://m.jd.com");
        console.log(cookie);
    }

    // const cookies = cm.getInstance().getCookie("*")
    // console.log(cookies);
    
    console.log('---------------------');
    
    if (cookie) {
        if (cookie.includes("pt_key") && cookie.includes("pt_pin")) {
            if (updateWSCK(cookie)) {
                toast("JD_COOKIE更新成功")
            } else {
                toast("JD_COOKIE更新失败")
            }
        }
    } else {
        toast("cookie为空")
    }
});


setTimeout(hookJava, 3000)

```

