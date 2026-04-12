---
title: ssr
date: 2026-04-12
slug: ssr
categories:
    - ssr
tags:
    - ssr
---

[参考](https://juejin.cn/post/7233220422242091063)

[使用 Service worker 实现加速/离线访问博客](https://neveryu.github.io/2017/06/08/service-worker/)

```java
<!-- 注册 Service Worker 加速访问 -->
<script>
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('/sw.js?v=22');
  });
}
</script></div>
<div style="margin-top:2px; margin-top:6px;">
GMT+8, 2021-11-18 20:27, PE: 0.011677s , QE: 3, Redis On.</div>
</div>
</div>

<!-- 百度统计 -->
<script>
var _hmt = _hmt || [];
_hmt.push(['_setUserId', window.discuz_uid]);
(function() {
var hm = document.createElement("script");
hm.src = "https://hm.baidu.com/hm.js?8cf683f51eb4795c5d09446e920329c3";
var s = document.getElementsByTagName("script")[0]; 
s.parentNode.insertBefore(hm, s);
})();
</script>

<!-- Google 统计 -->
<script src="https://www.googletagmanager.com/gtag/js?id=UA-17880378-3" type="text/javascript"></script>
<script>
window.dataLayer = window.dataLayer || [];
function gtag(){dataLayer.push(arguments);}
gtag('js', new Date());
gtag('config', 'UA-17880378-3', { 'user_id': window.discuz_uid });
</script>
</div>
<script src="home.php?mod=misc&ac=sendmail&rand=1637238465" type="text/javascript"></script>

```