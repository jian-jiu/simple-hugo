---
title: js
date: 2026-03-07
slug: js
categories:
    - js
tags:
    - js
---

// 获取描述信息（可查询不可修改）

浅拷贝 Object.assign()

深拷贝 Object.getOwnPropertyDescriptors()

想要修改需要使用

Object.defineProperties

# Object.defineProperties()

方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象

```js
Object.defineProperties({},student对象) 
// 相当于拷贝一个新的student对象
```

在js中使用 = 
为赋值对象的引用(指针)，并不能创建一个一模一样的对象
