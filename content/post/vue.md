---
title: vue
date: 2026-03-09
slug: vue
categories:
  - vue
tags:
  - vue
---

## 安装
[详情](https://blog.csdn.net/fxbfxb111/article/details/85094092)


## 表单验证
[async-validator](https://github.com/yiminghe/async-validator)

## vuecli 中 chainWebpack 的常用操作
[详情](https://www.cnblogs.com/linjunfu/p/14520345.html)

### cacheGroups 分包优化
[详情](https://juejin.cn/post/6919684767575179278)

## ssr prerender-spa-plugin预渲染
[详情](https://www.cnblogs.com/chuaWeb/p/prerender-plugin.html)

## 样式穿透
[参考](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0023-scoped-styles-changes.md#deep-selectors)

## quasar
[官网](https://quasar.dev/)
# quasar create 出现错误
由于 github.com 访问太慢，导致卡死。
下载到本地，避免从 github.com 上拉取
[连接](https://github.com/quasarframework/quasar-starter-kit)

## 开始创建项目
项目名称 可以不用
项目产品名称（如果构建移动应用程序，必须以字母开头）（Quasar应用程序）

项目名、作者名之类
选择 css 预处理器
是否使用 eslint：eslint 为 JS 语法校验， 会帮你检查 js 语法错误和形成团队的代码规范。
是否支持 ie11
是否自动安装依赖包，以及哪种形式安装（yarn 或 npm)： 没装 yarn 的话还是选否吧，npm 安装包真的很慢...
其它...

## vite 按需引入
[参考1](https://juejin.cn/post/7012446423367024676)
[参考2](https://www.xiaoboy.com/topic/202109180735.html)

## 全局登陆弹窗
[参考](https://blog.csdn.net/HelloWorldLJY/article/details/124844422)

## VUE2 弹窗组件实现动态挂载
需要在某些页面中动态加载一个弹窗，比如未登录的时候，在点击某些地方，需要弹出登录框要求登录。

如果在每个页面都import进去未免太过繁琐，在根页面引入后监听也不咋好的感觉。

因此，可以使用VUE.extend动态挂在组件。以登录框组件为例：

首先，自然是要一个登录组件页面，login.vue，这个按照自己的需求写就行了。

然后，新建login.js，代码如下：
```
import Vue from "vue";
import login from "./login.vue";
import store from "../../store";
const PopupBox = Vue.extend(login);
let instance;
login.install = function(data) {
  instance = new PopupBox({
    data
  }).$mount();
  instance.$store = store;
  document.body.appendChild(instance.$el);
 
  Vue.nextTick(() => {
    instance.showLogin();
  });
};
login.unInstall = function() {
  if (instance) {
    document.body.removeChild(instance.$el);
    instance.$destroy();
    instance = null;
  }
};
export default login;
```
之后，在main.js里加入：
```
import login from "./components/Login/login";
Vue.prototype.$login = login;
```
这个时候，就可以在有需要的地方，使用install()来挂在弹框：
```
this.$login.install();
```
在需要关闭弹框的地方，就用：
```
this.$login.unInstall();
```
还可以往组件传参，比如要传一个id：
```
 instance = new PopupBox({
    propsData:{
      id:id
   }
  }).$mount();
 ```
这样在登录组件里，id就是props的存在啦~或者之间用data覆盖。

[纯js调用公共组件弹框弹窗](https://www.jianshu.com/p/b6f7d004d418)

## setup 和 tsx
[参考](https://juejin.cn/post/7282692088016437307)
