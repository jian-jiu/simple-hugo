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

## bun 下一代js
[bun](https://bun.sh/)

## webRtc
[网站](https://snapdrop.net/#)
[参考](https://www.cnblogs.com/x_wukong/p/12844431.html)
[参考](https://www.cnblogs.com/himax/p/webrtc_connection_flow.html)
[参考](https://smileyqp.github.io/webrtc_book/doc/%E3%80%90webrtc%E3%80%91webrtc%E9%87%87%E9%9B%86%E5%B1%8F%E5%B9%95%E6%95%B0%E6%8D%AE%EF%BC%8C%E8%8E%B7%E5%8F%96%E6%A1%8C%E9%9D%A2%EF%BC%8820%EF%BC%89.html)
[参考](https://www.creeper.me/185)

## 虚拟表格
[参考](http://www.umyui.com/umycomponent/basicEditTable/)

## 对象复制
[参考](https://blog.csdn.net/qq_32434535/article/details/121318857)

## 集成 websocket 的四种方案
[详情1](https://www.cnblogs.com/huaixiaonian/p/14030202.html)
[详情2](https://laowan.blog.csdn.net/article/details/113845879)

### netty
#### ws连接
```java
    public void ConnWebSocketServer() {
        EventLoopGroup client = new NioEventLoopGroup();
        try {
            Bootstrap bootstrap = new Bootstrap();
            URI wsUri = new URI("ws://119.29.180.139:9503/ws");
            bootstrap.group(client)
                    .channel(NioSocketChannel.class)
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel ch) {
                            ChannelPipeline pipeline = ch.pipeline();
                            pipeline.addLast(new HttpClientCodec());
                            pipeline.addLast(new HttpObjectAggregator(1024 * 10));
                            pipeline.addLast("WebSocketClientHandler", new WebSocketClientHandler(WebSocketClientHandshakerFactory.newHandshaker(wsUri,
                                    WebSocketVersion.V13, null, true, new DefaultHttpHeaders())));
                            pipeline.addLast(new StringDecoder());
                            pipeline.addLast(new StringEncoder());
                        }
                    });
            ChannelFuture cf = bootstrap.connect(wsUri.getHost(), wsUri.getPort()).sync();
            cf.channel().closeFuture().sync();
        } catch (Exception ignored) {
        } finally {
            client.shutdownGracefully();
        }
    }
```
回调类
```java
package com.wisdomsite.socket;

import io.netty.channel.*;
import io.netty.handler.codec.http.FullHttpResponse;
import io.netty.handler.codec.http.websocketx.CloseWebSocketFrame;
import io.netty.handler.codec.http.websocketx.TextWebSocketFrame;
import io.netty.handler.codec.http.websocketx.WebSocketClientHandshaker;

public class WebSocketClientHandler extends SimpleChannelInboundHandler<Object> {

    private WebSocketClientHandshaker webSocketClientHandshaker;
    private ChannelPromise handshakeFuture = null;

    public WebSocketClientHandler(WebSocketClientHandshaker webSocketClientHandshaker) {
        this.webSocketClientHandshaker = webSocketClientHandshaker;
    }

    @Override
    public void handlerAdded(ChannelHandlerContext ctx) {
        this.handshakeFuture = ctx.newPromise();
    }

    /**
     * 当客户端主动链接服务端的链接后，调用此方法
     *
     * @param channelHandlerContext ChannelHandlerContext
     */
    @Override
    public void channelActive(ChannelHandlerContext channelHandlerContext) {
        Channel channel = channelHandlerContext.channel();
        // 握手
        webSocketClientHandshaker.handshake(channel);
    }

    @Override
    protected void channelRead0(ChannelHandlerContext ctx, Object msg) throws Exception {

        // 握手协议返回，设置结束握手
        if (!this.webSocketClientHandshaker.isHandshakeComplete()) {
            FullHttpResponse response = (FullHttpResponse) msg;
            this.webSocketClientHandshaker.finishHandshake(ctx.channel(), response);
            this.handshakeFuture.setSuccess();
            return;
        }

        if (msg instanceof TextWebSocketFrame) {
            TextWebSocketFrame textFrame = (TextWebSocketFrame) msg;
            System.out.println(textFrame);
        }

        if (msg instanceof CloseWebSocketFrame) {
        }
    }

    @Override
    public void channelInactive(ChannelHandlerContext ctx) throws Exception {
//        System.out.println("WebSocketClientHandler::channelInactive 服务端连接成功");
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.channel().close();
    }

}

```

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

## Thymeleaf
### 时间

```
#dates.format(#dates.createNow(),'yyyy-MM-dd HH:mm:ss')
```

### 保留小数

${#numbers.formatDecimal(num,0,'COMMA',2,'POINT')}则显示 .00

${#numbers.formatDecimal(num,1,'COMMA',2,'POINT')}则显示 0.00

COMMA：','

POINT：‘.’

### 配置文件

spring.thymeleaf

关闭缓存

cache=false

模板位置

prefix=classpath:/**/

suffix=.html

### html属性

html头属性 xmlns:th="http://www.thymeleaf.org"

th:标签属性

#{取值}

th:text="${}"

th:object="${}" "*{}"

[[${取作用域的值}]]

url@{}

th:each="a:${**}"

aStat

第一个 first

最后一个 last

奇数 even

偶数 odd

排除 th:unless

### 使用工具包

```
#strings.isEmpty(msg)
```

## jsp攻略
### a标签

```
//新页面
target="_blank"
```

### 下拉框

```
//获取下拉框 已选择的值
alert($("#create-transactionStage option:selected").text())
//获取下拉框 已选择的内容
alert($("#create-transactionStage option:selected").val())
```

### 禁止修改属性

```
readonly 
```

```
disabled 用表单提交不了 是灰色的背景
```

### 输入的值只能是数字

```
oninput="value=value.replace(/[^\d]/g,'')"
```

### 关于表单

中有一个输入框的时候回车默认触发action事件

```
设置表单数据没有都不触发
action="javascript:;"
设置输入框不提示缓存内容
autocomplete="off"
选择多个文件
multiple
```

### 上传文件

```
enctype="multipart/form-data"

ajax
new FormData($('#dataForm')[0]),
```

### 鼠标放上去的提示以及图片加载失败提示

```
鼠标放上去的提示
title="没有图片"
    图片加载失败图片
    onerror="onerror=null;src='static/images/products/c_0001.jpg'"
```

```
获得焦点
$("#name").focus()
```

### 取整

```
parseInt（数值）
```

### 显示

```
juery对象.hide()
```

### 隐藏

```
juery对象.show()
```

### 元素追加

选择器.htm;()重新设置内部内容

选择器.empty()删除元素内部内容

选择器.append()元素内部

选择器.after()元素外部后面

选择器.before()元素外部前面

### 判断不为空

```
${not empty activity.startDate && !(activity.startDate eq null)?activity.startDate:"未设置"}
```

### 正则表达式

/表达式/.test(变量)

### 选择器

id：#id

class：.id

标签：id

表单：:id

### 提示删除信息

```
if (!confirm("确认删除吗")) return;
```

#### 过滤器(筛选器)

$("id:first") //第一个

$("id:last") //第后一个

$("id:eq(2)") //按照下标查询

#### table

```
行
rowspan=""
列
colspan=""
```

#### 空格

#### 换行

```
<br/>
```

#### 一行

<hr/>

### JSTL标签库

#### for循环

```jsp
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

```jsp
<c:forEach items="${requestScope.list}" var="emp"></c:forEach>
```

```jsp
<c:if test="${not empty 判断数据（对象）}">
//不为空    
</c:if>
```

```jsp
<c:forEach items="${集合(容器)}" var="自定义名称">
</c:forEach>
```

and 和

or 或者

#### 选择标签

```
<c:choose>
            <c:when test="">
<%--                选择标签--%>
            </c:when>
            <c:otherwise>
<%--                都不成立执行这个--%>
            </c:otherwise>
        </c:choose>
```

#### 判断回车按键

```
$(window).keydown(e => {
    if (e.key == "Enter") {
        if (!createClueModal.is(":hidden")) {
                        //判断是否失去焦点
                        if (!($("#noteContent")[0] == document.activeElement)) {
                            updateRemarkBtn.click()
                        }
            createClueBtn.click()
        }
    }
})

$(document).ready(function () {  
        $(":text").keydown(function () {    // 按键按下时触发的事件；  
            $(":text").css("background-color", "green");  
        });  

        $(":text").keyup(function () {      // 按键弹起时触发的事件；  
            $(":text").css("background-color", "blue");  
        });  

        $("button").click(function () {  
            $(":text").keyup();             // keyup()方法触发keyup事件；  
        });  

        var i = 0;  
        $(":text").keypress(function () {   // keypress：输入框每获取一个字符，就触发一次该事件。  
            $("span").text(i++);  
        });  
    });
```

### 内置对象

request：请求

response：响应

session：会话

application：ServletContext

config：ServletConfing

out：页面输出对象

page：this

exception：异常对象

pageContext：jsp核心，功能对象

### 指令元素

<$@ 元素%>

#### page里面的属性

pageEncoding:响应编码

contentType:相应类型

language: 指定语言java

isErrorPage:是否接收异常

errorPage:

import:导包

#### include:包含其他jsp

#### taglib:引入其他标签库

### 脚本元素

<% %>定义jspService()方法内容

<%! %>声明标签：定义类下的内容

<%= %>输出标签：表达式标签

<%-- --%>注释标签

### 动作元素

<jsp:include 包含其他jsp

<jsp:forward 请求转发

<jsp:param 传参数

<jsp:useBean 类实例化

<jsp:setProperty 属性设置

<jspgetProperty 属性获取

### EL表达式

${id}

默认从四个作用域中获取

从最小的page中开始获取

#### 内置对象(小>大)

(6个作用域范围)

pageScope

requsetScope

sessionScope

applicationScope

#### 获取 ？后面的请求参数

${param.id}

##### 获取同名的多个参数

${param.id[0]}

${param.id[1]}

${param.id[2]}

#### 获取cookie内容

${cookie.id}

#### 获取Map

${对象(键)}

#### 获取List

${对象[下标]}

### ajax

```
$.ajax({
    url: 'settings/qx/user/login.do',
    data: {
        loginAct: loginAct,
        loginPwd: loginPwd,
        isRemPew: isRemPew,
    },
    type: 'post',
    dataType: 'json',
    //如果你想要用传统的方式来序列化数据，那么就设置为true
    //用来上传数组
    traditional:true,
    success: function (data) {
        if (data.code == "1") {
            window.location.href = "workbench/index.do";
        } else {
            $("#msg").text(data.message);
        }
    },
    beforeSend:function () {//在发生ajax之前执行这个函数

       }
})
```

### 截取字符串

str.substr(从哪里开始,截取的长度)

str.substr(从哪里开始) 截取到字符串最后

str.substring(从哪里开始,截取到哪里)

### 获取某个值的下标

```
str.lastIndexOf(".")
```

### 返回上一页

```
onclick="window.history.back();"
```

### 全选

#### 方式1

```
var $chk = $(":checkbox[name=chk]");
// 全选
$("#selectAll").click(function() {
    // 让所有复选框的选中状态和全选按钮的选中状态一致
    $chk.prop("checked", this.checked);
});

// 当取消选中某个商品，全选按钮也应该取消选中，即选择商品的复选框会影响全选按钮的选中状态
$chk.click(function() {
    /*// 先让全选按钮选中
    $("#selectAll").prop("checked", true);
    // 遍历查找未选中的复选框，如果有，则全选按钮一定是未选中的
    $chk.each(function() {
        if (!this.checked) {
            $("#selectAll").prop("checked", false);
            // 必须return false才可以结束each循环
            return false;
        }
    });*/

    // 判断选中的数量和总数据是否一致，如果一致，则全选按钮选中
    //$("#selectAll").prop("checked", $chk.length == $(":checkbox[name=chk]:checked").length);
    // $("#selectAll").prop("checked", $chk.length == $chk.filter(":checked").length);
    $("#selectAll").prop("checked", !$chk.is(":not(:checked)"));
});
```

#### 方式2

```
//单击全选按钮
$("#selectAll").click(function () {
    $("#checkTable input[type='checkbox']").prop('checked', $("#selectAll").prop('checked'))
})

//给所有复选框添加单击事件
var arrayObj = $("#checkTable input[type='checkbox']")
arrayObj.splice(0, 1)
// console.log(arrayObj)
arrayObj.click(function () {
    if ($("#selectAll").prop('checked')) {
        if (!($("#checkTable input[type='checkbox']:checked").size() == ($("#checkTable input[type='checkbox']").size()))) {
            $("#selectAll").prop('checked', false)
        }
    } else {
        if ($("#checkTable input[type='checkbox']:checked").size() == ($("#checkTable input[type='checkbox']").size() - 1)) {
            $("#selectAll").prop('checked', true)
        }
    }
})
```

### 老师笔记

1、js截取字符串：
str.substr(startIndex,length);
str.substr(startIndex);//从下标为startIndex的字符开始截取，截取到字符串的最后，得到目标字符串。
var str="abcdef";
str.substr(2);//===>cdef

str.substring(startIndex,endIndex);

js中获取指定字符在字符串中的下标：str.indexOf(".")
js中获取指定的最后一个字符在字符串中的下标：str.lastIndexOf(".")
js中把字符串中所有的字母统一转化成大写字母： str.toUpperCase();
2、ajax发送异步请求：
data:向后台提交参数，三种写法：
1)、{
name1:value2,
name2:value2,
.....
}
只能用于提交文本数据
2)、name1=value1&name2=value2&....
只能用于提交文本数据
3)、FormData对象：FormData是ajax提供的一个接口，可以模拟name1-value1键值对向后台提交参数；
FormData最大的优势是不但能提交文本数据，还能提交二进制数据。
var formData=new FormData();
formData.append("activityFile",activityFile);
formData.append("username","zhangsan");
3、在页面中给元素添加事件的方式：
1)、使用事件属性：<input type="button" onclick="test()">
2)、使用jquery的事件函数：<input type="button" id="myId">
选择器.click(function(){
//.....
});
只能对固有元素添加事件，不能对动态生成的元素添加事件。
在页面加载过程中产生的元素，叫固有元素;
在页面加载完成之后产生的元素，叫动态生成的元素。
3)、使用jquery的on函数：
不但能给固有元素添加事件，还能给动态生成的元素添加事件，通常用来给动态生成的元素添加事件。
父选择器.on(事件类型,目标选择器,function(){
//....
});

```
   父选择器：要求父选择器必须是固有元素，不一定是直接父元素，但是要求必须是固有的。
            <div id="div2">//固有 
             <div id="div1">
                <input type="button">
     </div>
     </div>

     $("#div2").on();
 事件类型:跟jquery的事件函数一致。
 目标选择器：在父选择器基础之上查找目标元素。
            <div id="div2">//固有 
             <div id="div1">
                <input type="button">
     </div>
     </div>
     <input type="button">
            $("#div2").on("click","input[type='button']");
 function:回调函数。
            $("#div2").on("click","input[type='button']",function(){......});
```

### bs分页

```
容器.bs_pagination('getOption', 'rowsPerPage')
```

```
//分页插件
let totalPages = 0;
if (data.data.totalRows % pageSize == 0) {
    totalPages = data.data.totalRows / pageSize
} else {
    totalPages = parseInt(data.data.totalRows / pageSize) + 1
}
$("#demo_pag").bs_pagination({
    currentPage: pageNo,//当前页
    rowsPerPage: pageSize,//每有显示条数
    totalRows: data.data.totalRows,//总条数
    totalPages: totalPages,//总页数

    visiblePageLinks: 5,//显示的翻页卡片数

    showGoToPage: true,//是否显示“跳转到第几页”
    showRowsPerPage: true,//是否显示“每页显示条数”
    showRowsInfo: true,//是否显示记录的信息

    //每次切换页号对会触发函数，函数那返回切换后的页号和每页显示条数
    onChangePage: (e, pageObj) => {
        findTransaction(pageObj.currentPage, pageObj.rowsPerPage)
    }
})
```

## jquery
### 获取标签的元素值

选择器.attr(""); 获取不是true|false

选择器.prop(""); 获取是true|false

### jquery转换为dom对象

var j = $("#id")

方式1：var is = j[0];

方式2：var id = j.get[0];

### dom转换为jquery对象

$(dom对象)

### 父子选择器

```
$("#tBody input[type='checkbox']")
//tBody(id)选择器下的所有input标签
[]表示input标签所有的type='checkbox'属性
```

空格和>都可以表示父子选择器

空格表示标签下的所有标签

小于号(>)只能表示标签下的子标签(直接子标签)，孙子标签的表示不了

#### []表示根据属性过滤

### 实现复选框全选和取消全选

```
//实现全选和取消全选
$("#chkedAll").click(function () {
    //让列表中所有的复选框属性值和全选按钮的属性值一样
    $("#tBody input[type='checkbox']").prop("checked", $("#chkedAll").prop("checked"))
})
//给所有的复选框添加单击事件
$("#tBody input[type='checkbox']").click(function () {
    if ($("#tBody input[type='checkbox']").size() == $("#tBody input[type='checkbox']:checked").size()) {
        $("#chkedAll").prop("checked", true)
    } else {
        $("#chkedAll").prop("checked", false)
    }
})
```

### 遍历数组

```
$.each(需要遍历的数组, function (index(下标), object(每次遍历的内容)) {

})
//遍历一次执行一次function
```

### 删除元素

```
数组.splice(下标,数量)
```

## equals
[参考](https://www.cnblogs.com/javastack/p/13023716.html)
