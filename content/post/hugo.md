---
title: hugo教程
date: 2026-02-25
categories:
    - hugo
tags:
    - hugo
---

## hugo md头
```md

---
title: 标题
description: 描述
date: 2026-02-25
slug: url
categories:
    - hugo
tags:
    - hugo
---

```

## hugo常用命令

[参考](https://xxcjw.github.io/p/hugo%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4/)

## 一般开发

hugo server
它默认不会包含草稿文章

hugo server -D

## 发布

hugo --gc --minify

--gc 清除不再使用的缓存文件

--minify 文件进行压缩

## 推荐

hugo server --gc -D

启动一个实时预览服务器，同时执行垃圾回收，确保使用的是最新的缓存。适合在开发过程中使用，当频繁修改文章内容，并希望随时预览草稿文章在内的网站效果时最方便

## cf部署
[参考](https://www.heyjude.blog/zh-cn/posts/deploy-hugo-to-cloudflare/)

## 部链接用新窗口
拷贝 layouts/_markup/render-link.

新增 layouts/_default/_markup/render-link.html

## cdn外链图片无法直接点击打开
[博客参考](https://ghjayce.github.io/p/static-site-generator/hugo/hugo-theme-stack-gallery-study/)

[github 参考](https://github.com/CaiJimmy/hugo-theme-stack/discussions/659)
