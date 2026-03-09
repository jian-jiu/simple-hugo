---
title: js
date: 2026-03-09
slug: js
categories:
    - js
tags:
  - 网页
    - js
---

## RSA 公钥解密
版本: "jsencrypt": "3.2.0",
[原地址](https://www.yht7.com/news/93708)
js文件
```js
import JSEncrypt from 'jsencrypt/bin/jsencrypt.min'

// 密钥对生成 http://web.chacuo.net/netrsakeypair

// 公钥
const publicKey = ``

// 利用公钥对数据加密
export function encrypt(text) {
  const encryptor = new JSEncrypt()
  encryptor.setPublicKey(publicKey)
  return encryptor.encrypt(text)
}

// 利用公钥对私钥加密的数据进行解密
export function decrypt(text) {
  const encryptor = new JSEncrypt()
  encryptor.setPublicKey(publicKey)
  let decrypt = encryptor.decrypt(text)
  return decrypt ? decrypt : ''
}
```

RSA默认不支持公钥解密私钥
修改jsencrypt.js文件
```js
t.prototype.decrypt=function(t){
  // var e=P(t,16),i=this.doPrivate(e);
  var e=P(t,16),i=this.doPublic(e);
  return null==i?null:function(t,e){
    for(var i=t.toByteArray(),r=0;r<i.length&&0==i[r];)++r;
    // if(i.length-r!=e-1||2!=i[r])return null;
    for(++r;0!=i[r];)if(++r>=i.length)return null;
```

后台java文件
```java
import org.apache.commons.codec.binary.Base64;

import javax.crypto.Cipher;
import java.security.KeyFactory;
import java.security.PrivateKey;
import java.security.spec.PKCS8EncodedKeySpec;

/**
 * rsa 密钥加密解密
 */
public class RsaUtils {

	/**
	 * Rsa 私钥
	 */
	public static String privateKey = "";

	/**
	 * 私钥解密
	 *
	 * @param text 待解密的文本
	 * @return 解密后的文本
	 */
	public static String decryptByPrivateKey(String text) throws Exception {
		return decryptByPrivateKey(privateKey, text);
	}

	/**
	 * 私钥加密
	 *
	 * @param text 待加密的文本
	 * @return 加密后的文本
	 */
	public static String decryptByPublicKey(String text) throws Exception {
		return encryptByPrivateKey(privateKey, text);
	}

	/**
	 * 私钥加密
	 *
	 * @param privateKeyString 私钥
	 * @param text             待加密的信息
	 * @return 加密后的文本
	 */
	public static String encryptByPrivateKey(String privateKeyString, String text) throws Exception {
		PKCS8EncodedKeySpec pkcs8EncodedKeySpec = new PKCS8EncodedKeySpec(Base64.decodeBase64(privateKeyString));
		KeyFactory keyFactory = KeyFactory.getInstance("RSA");
		PrivateKey privateKey = keyFactory.generatePrivate(pkcs8EncodedKeySpec);
		Cipher cipher = Cipher.getInstance("RSA");
		cipher.init(Cipher.ENCRYPT_MODE, privateKey);
		byte[] result = cipher.doFinal(text.getBytes());
		return Base64.encodeBase64String(result);
	}

	/**
	 * 私钥解密
	 *
	 * @param privateKeyString 私钥
	 * @param text             待解密的文本
	 * @return 解密后的文本
	 */
	public static String decryptByPrivateKey(String privateKeyString, String text) throws Exception {
		PKCS8EncodedKeySpec pkcs8EncodedKeySpec5 = new PKCS8EncodedKeySpec(Base64.decodeBase64(privateKeyString));
		KeyFactory keyFactory = KeyFactory.getInstance("RSA");
		PrivateKey privateKey = keyFactory.generatePrivate(pkcs8EncodedKeySpec5);
		Cipher cipher = Cipher.getInstance("RSA");
		cipher.init(Cipher.DECRYPT_MODE, privateKey);
		byte[] result = cipher.doFinal(Base64.decodeBase64(text));
		return new String(result);
	}

}
```

## 异步 Promise详解（resolve,reject,catch）
[详情](https://blog.csdn.net/qq_37939251/article/details/93650542)

## 其他

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

## mouseover(鼠标覆盖)mouseenter(鼠标进入)两个事件的区别
[详情](https://www.cnblogs.com/showcase/p/11274683.html)


[剪切板复制粘贴](https://clipboardjs.com/)

## highlightjs前端代码高亮
[官方](https://highlightjs.org/)
[中文网站](https://fenxianglu.cn/highlight.html)

### vue使用highlight.js的坑
[详情](https://www.jianshu.com/p/6c1e4fcc6d6f)
