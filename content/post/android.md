---
title: android
date: 2026-03-09
slug: android
categories:
  - 安卓
tags:
  - android
---

## 全局弹出对话框
[详情](https://blog.csdn.net/u011928958/article/details/72780438/)

## 配置文件application AndroidManifest
[详情1](https://blog.csdn.net/almo_omla/article/details/53694087)

[详情2](https://blog.csdn.net/talkping/article/details/50418080)

## 安卓应用内打开其他软件的页面
[详情](https://blog.csdn.net/m0_37168878/article/details/73863046)

## 音视频对讲演示程序
[官方地址](https://github.com/cyz7758520/Android_audio_talkback_demo_program)

## 安卓编译报错
[# Execution failed for task ‘:app:checkDebugDuplicateClasses‘.
详情](https://blog.csdn.net/m0_46278918/article/details/107770473)

## 投屏
ARDC

## Android Studio
## 工具下载
[地址](https://developer.android.google.cn/studio)
## 插件下载
[地址](https://plugins.jetbrains.com/plugin/13710-chinese-simplified-language-pack----/versions/stable)

## 下载慢问题
[参考](https://blog.csdn.net/BG1230521/article/details/136605382)
[参考](https://blog.csdn.net/m0_63070489/article/details/136382231)
[参考](https://blog.csdn.net/qq_57474766/article/details/132644097)

## 最小apk
[参考](https://raiseyang.github.io/post/zui-xiao-apk-bian-yi-shi-zhan/)

## 打包
[参考](https://blog.csdn.net/qq_38436214/article/details/112288954)
## 提高下载速度

修改build.gradle
```gradle
buildscript {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven{ url 'http://maven.aliyun.com/nexus/content/repositories/jcenter'}
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.3.0-alpha13'
    }
}
allprojects {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
        maven{ url 'http://maven.aliyun.com/nexus/content/repositories/jcenter'}
    }
}
task clean(type: Delete) {
    delete rootProject.buildDir
}
```
