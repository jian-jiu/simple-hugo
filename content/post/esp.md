---
title: esp
date: 2026-03-07
categories:
    - esp
tags:
    - esp
    - esphome
    - esp8266
---

## esphome
[蓝牙代理](https://bbs.hassbian.com/thread-20157-1-1.html)

### esp01s
[rst cause:2, boot mode:(\*,\*) 错误意思](https://www.singleye.net/2017/05/esp8266%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F---%E5%A6%82%E4%BD%95%E7%90%86%E8%A7%A3rst-cause2-boot-mode36/)

## 小智
[MCP](https://www.bilibili.com/video/BV1Hg7szeEMT/)

## clion
### platformio
安装python
```shell
pip install platformio
pio settings get

# 查看环境变量platformio
pio system info
修改 PLATFORMIO_CORE_DIR 为自定义
PlatformIO Core Directory 就是自己想要的了
```
### arduino教程
[参考](https://www.echo.cool/docs/category/arduino-%E6%95%99%E7%A8%8B)


[esp32文档](https://wiki.luatos.com/chips/esp32c3/index.html)

[Clion配置ESP32开发](https://blog.csdn.net/m0_52162042/article/details/142099459)

[CLion 安装 platformIO 教程](https://songhailong.cn/2023/05/CLion-install-PlatformIO/)

[ESPAsyncWebServer](https://blog.csdn.net/Naisu_kun/article/details/107164633)

[esp32更新](https://blog.csdn.net/Naisu_kun/article/details/107358763)
[程序太大](https://blog.csdn.net/qq_51961498/article/details/122723544)

[下载慢问题](https://github.com/platformio/platformio-core/issues/5235)
无法解决用vpn吧

```shell
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

~\.platformio\penv\pip.conf

[global]
user=no
timeout = 1000
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

C:\Users\Administrator\.platformio\penv\Scripts\pio.exe project init --board airm2m_core_esp32c3
pio project init --board esp01_1m
```

### 命令下载
```shell
pio project init --board airm2m_core_esp32c3
```
### esp32 c3
[风扇参考](https://blog.csdn.net/u012388993/article/details/130318569)

[pwm问题](https://blog.csdn.net/qq_42679566/article/details/119549859)
[pwm问题1](https://blog.csdn.net/qq_57139623/article/details/139859580)

[mcpwm](https://www.codeleading.com/article/84763231155/)

## ESP-01S(ESP8266)
## 烧录
GPIO0 拉地(接地 负极)

### 无法深度睡眠下唤醒
存储芯片的问题
小点旁边[小点2引脚](第二个 [DO MISO SD_D0]) 10k(电阻) vcc(电源正) 
[参考1](https://www.soinside.com/question/dKemPdY2JXERXA6WoKxoeE)

[参考](https://github.com/esp8266/Arduino/issues/6007)
[参考](https://www.bilibili.com/opus/894260792904384517)
[参考](https://www.mydigit.cn/forum.php?mod=viewthread&tid=434740&extra=page=1&page=1)

[参考](https://blog.csdn.net/qq_42417071/article/details/135255643)
[使用USB转TTL下载固件](https://blog.csdn.net/weixin_43764787/article/details/116333778)

### 「 ESP8266-01s刷入固件报SP8266 Chip efuse check error esp_check_mac_and_efuse」
esptool --port COM6 write_flash 0x00000

#### 睡眠
[参考1](https://blog.csdn.net/Nirvana_6174/article/details/104485963)
[参考2](https://blog.51cto.com/u_16099215/10675454)
[参考3](https://www.tubring.cn/articles/85)

## platformio
```cpp
#include <Arduino.h>

void setup()
{
    Serial.begin(115200);
    pinMode(2,OUTPUT); //初始化GPIO2为输出模式
}

void loop()
{
    digitalWrite(2,HIGH); //GPIO2输出高电平
    delay(500);
    digitalWrite(2,LOW); //GPIO2输出低电平
    delay(500);

    Serial.printf("getFlashChipId  %u \n", EspClass::getFlashChipId());
    Serial.printf("getFlashChipVendorId  %hhu \n", EspClass::getFlashChipVendorId());
    Serial.printf("getFlashChipRealSize  %hhu \n", EspClass::getFlashChipRealSize());
}
```
```ini
[env:esp01_1m]
platform = espressif8266
board = esp01_1m
framework = arduino
;monitor_speed = 115200
monitor_speed = 74880
```