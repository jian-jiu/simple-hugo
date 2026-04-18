---
title: css
date: 2026-03-09
slug: css
categories:
    - css
tags:
    - 网页
    - css
---

## 关于@media不生效的问题和meta总结
[详情](https://www.cnblogs.com/kelly2017/p/7085630.html)

[Grid 网格布局](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

### 图表
[参考](https://xiaoshen.blog.csdn.net/article/details/134333070)
[仓库](https://github.com/Sjj1024/uniapp-vue3)

## vue v-html
[参考](https://blog.csdn.net/a1056244734/article/details/125785518)

## 文字小程序
[参考](https://blog.csdn.net/qq_42740797/article/details/107819992)
```css
.text-deal{
  overflow : hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  word-break: break-all; /* 追加这一行代码 */
}
```

## taro
[参考（unplugin-auto-import）](https://blog.csdn.net/weixin_39786582/article/details/131154321)

## 前端 图片
```css
aspect-ratio: 1/1
```

## 新用户引导库
[driver.js](https://driver.employleague.cn/)
[introjs](https://introjs.com/)

## css sass less scss
[sass、scss、和css的关系](https://zhuanlan.zhihu.com/p/39083902)

## 前端大数据
[素材](https://bbs.fit2cloud.com/t/topic/46/2)
[素材](https://fvd.fanruan.com/)

## element-ui
[el-table多列同时排序且样式保留(多用于后台排序)](https://segmentfault.com/a/1190000043304424)

[CSS — BEM 命名规范](https://juejin.cn/post/6844903672162304013)

## 小程序登录解决方案（可能）
```js
import auth from './utils/auth';

const preCheckLogin = function () {
  return new Promise((resolve, rejects) => {
    const app = getApp();
    if (!app.globalData.isLogin()) {
      wx.login({
        timeout: 30000,
        success(wr) {
          if (wr.code) {
            auth.authLogin(wr.code).then(res => {
              if (res.status) {
                resolve(true);
              } else {
                rejects('not login');
              }
            }).catch(e => {
              console.error(e);
              rejects('not login');
            })
          } else {
            rejects('not wx login');
          }
        },
        fail(e) {
          console.error(e);
          rejects('not wx login fail');
        }
      })
    } else {
      resolve(true);
    }
  })
}

// 对原有的Page进行装饰
const PageExtend = page => {
  return obj => {
    const {onReady} = obj;
    obj.onReady = function (options) {
      // 这里检查是否已登陆，如果没有登陆，则跳转到登陆页面
      preCheckLogin()
        .then(() => {
          if (typeof onReady === 'function') {
            onReady.call(this, options)
          }
        })
        .catch((e) => {
          wx.navigateTo({
            url: '/pages/login/index',
          })
        })
    }
    return page(obj);
  }
}

const originPage = Page
Page = PageExtend(originPage);

export default originPage;
```

## css动画
[css动画](https://css-loaders.com)