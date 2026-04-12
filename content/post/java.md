---
title: java
description: java
date: 2026-03-09
slug: java
categories:
    - java
tags:
    - java
---

## 日志
### XSLF4J
[参考](https://blog.csdn.net/weweeeeeeee/article/details/112985672)

### p6Spy sql日志
这个有损性能
[详情](https://blog.csdn.net/javaYouCome/article/details/114360586)

## private、protected、public和default的区别
[参考](https://www.cnblogs.com/jingmengxintang/p/5898900.html)

## gradle
[排除依赖包](https://blog.csdn.net/xktemp/article/details/120670250)

## VO、DTO、BO、PO、DO、POJO 的概念、区别和用处
[https://blog.csdn.net/zjrbiancheng/article/details/6253232](https://blog.csdn.net/zjrbiancheng/article/details/6253232)

[图文](https://lux-sun.blog.csdn.net/article/details/113695259)

[原文](http://www.blogjava.net/johnnylzb/archive/2010/05/27/321968.html)

## @Order、@Priority、@Primary 三个注解和 Orderd 接口
# @orderd
@orderd接口，实现Oderd接口的话要实现int getOrder();这个方法，返回一个整数值，值越小优先级越高。

# @Order
@Order里面存储了一个值，默认为Integer的最大值，同样值越小优先级越高。要注意@Order只能控制组件的加载顺序，不能控制注入的优先级。但是能控制List 里面存放的XXX的顺序，原因是当通过构造函数或者方法参数注入进某个List时，Spring的DefaultListableBeanFactory类会在注入时调用AnnotationAwareOrderComparator.sort(listA)帮我们去完成根据@Order或者Ordered接口序值排序。@Order更加适用于集合注入的排序。

# @Priority
@Priority与@Order类似，@Order是Spring提供的注解，@Priority是JSR 250标准，同样是值越小优先级越高。但是两者还是有一定却别，@Priority能够控制组件的加载顺序，因此@Priority侧重于单个注入的优先级排序。此外@Priority优先级比@Order更高，两者共存时优先加载@Priority。

@Primary是优先级最高的，如果同时有@Primary以及其他几个的话，@Primary注解的Bean会优先加载。

## @Inherited注解的作用
[@Inherited注解的作用](https://www.jianshu.com/p/7f54e7250be3)

## 随机数加快
-Djava.security.egd=file:/dev/urandom 作用

[参考](https://codingdict.com/questions/121478)

[参考](https://blog.51cto.com/leo01/1795447)

## 警告 @SuppressWarnings注解
[详情](https://blog.csdn.net/Mr_Zhang____/article/details/101939732)

## 前端参数判断
[官网](https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/#validator-gettingstarted-createproject)

[字符串判断](https://www.cnblogs.com/yickel/p/14500222.html)
[详细介绍](https://blog.csdn.net/wangjiangongchn/article/details/86477386)
[详细介绍1 (里面有快速失败返回模式) ](https://www.cnblogs.com/leeego-123/p/10820099.html)

## 利用Filter和springAOP实现记录URL和方法的运行时间
### 记录URL运行时间
#### filter配置类
```java
import lombok.Data;
import lombok.RequiredArgsConstructor;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * Filter配置
 */
@Data
@Configuration
@RequiredArgsConstructor
public class FilterConfig {

    @Bean
    @SuppressWarnings({"rawtypes", "unchecked"})
    public FilterRegistrationBean timeMonitorFilterRegistration() {
        FilterRegistrationBean registration = new FilterRegistrationBean();
        registration.addUrlPatterns("/*");
        registration.setName("timeMonitorFilter");
        registration.setFilter(new TimeMonitorFilter());
        registration.setOrder(FilterRegistrationBean.HIGHEST_PRECEDENCE);
        return registration;
    }
}
```
#### 过滤器
```java
import cn.hutool.core.date.StopWatch;
import lombok.extern.slf4j.Slf4j;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

/**
 * 时间监视器过滤器
 * 可以获取URL的运行时间
 */
@Slf4j
public class TimeMonitorFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        String uri = ((HttpServletRequest) request).getRequestURI();
        try {
            chain.doFilter(request, response);
        } finally {
            stopWatch.stop();
            log.info("访问网址 [{}], 花费时间 [{}] ms [{}] ns )", uri, stopWatch.getLastTaskTimeMillis(), stopWatch.getLastTaskTimeNanos());
        }
    }

}
```

### 方法运行时间
#### 注解
```java
import java.lang.annotation.*;

/**
 * 运行时间,方法使用此注解,可以查看方法的运行时间
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface RumTime {
}
```
#### AOP类
```java
import cn.hutool.core.date.StopWatch;
import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.core.Ordered;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

/**
 * 运行时间切面
 * 可以获取方法的运行时间
 */
@Slf4j
@Aspect
@Component
@Order(Ordered.HIGHEST_PRECEDENCE)
public class TimeAspect {

    @Pointcut("@annotation(com.**.RumTime)")
    public void timePointCut() {
    }

    /**
     * 环绕通知
     */
    @Around("timePointCut()")
    public Object doBefore(ProceedingJoinPoint joinPoint) throws Throwable {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        Object result = joinPoint.proceed();
        stopWatch.stop();
        log.info("执行的类名 [{}] 方法名为 [{}], 花费时间 [{}] ms [{}] ns )"
                , joinPoint.getTarget().getClass().getName(), joinPoint.getSignature().getName()
                , stopWatch.getLastTaskTimeMillis(), stopWatch.getLastTaskTimeNanos());
        return result;
    }

}
```

## 音频流，视频流
```java
public void test(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //创建连接对象
        URL url = new URL("http://媒体文件url");
        URLConnection conn = url.openConnection();
        //设置超时
        conn.setConnectTimeout(1000);
        conn.setReadTimeout(5000);
        //发起连接
        conn.connect();
        //获取流
        InputStream inputStream = conn.getInputStream();

        //流转换
        IOUtils.copy(inputStream,response.getOutputStream());
        //设置返回类型
        response.addHeader("Content-Type", "audio/mpeg;charset=utf-8");

        response.flushBuffer();

    }
```
### html
<audio src="http://127.0.0.1:8766/iodemo/test" controls="controls">
您的浏览器不支持音频元素
</audio>

## 计时器
```java
public static void main(String[] args) {
  StopWatch clock = new StopWatch();
 
  clock.start("开始任务一");
  doSomeThing();
  clock.stop();
 
  clock.start("开始任务二");
  doSomeThing();
  clock.stop();
 
  System.out.println(clock.prettyPrint());
}
```

## java类的生命周期
### 类加载顺序

(祖)(父)(自己)类静态代码块 --> (祖)(父)(自己)实例化方法然后构造方法

如果调用静态方法

(祖)(父)类静态代码块 --> (祖)(父)(自己)实例化方法然后构造方法 --> (自己)类静态代码块

结论：如果调用静态方法，静态方法最后执行
### 注解
注解被用来修饰一个非静态的void（）方法
Constructor(构造方法) -> @Autowired(依赖注入) -> @PostConstruct(注释的方法)
###     @PostConstruct
被修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器执行一次
通常用来初始化一些操作
```java
public void destroy(){
    System.out.println("类开始加载");
}
```
### @PreDestroy
被修饰的方法会在服务器销毁Servlet的时候运行，并且只会被服务器执行一次
通常用来处理一些关闭操作
```java
public void destroy(){
    System.out.println("类即将销毁");
}
```

## 缓冲队列BlockingQueue
### ArrayBlockingQueue（int i）
规定大小的BlockingQueue，其构造必须指定大小。其所含的对象是FIFO顺序排序的
### LinkedBlockingQueue（）或者（int i）
大小不固定的BlockingQueue，若其构造时指定大小，生成的BlockingQueue有大小限制，不指定大小，其大小有Integer.MAX_VALUE来决定。其所含的对象是FIFO顺序排序的。
### PriorityBlockingQueue（）或者（int i）
类似于LinkedBlockingQueue，但是其所含对象的排序不是FIFO，而是依据对象的自然顺序或者构造函数的Comparator决定。
### SynchronizedQueue（）
特殊的BlockingQueue，对其的操作必须是放和取交替完成

## 线程状态
### New(初始化状态)
### Runnable(就绪状态)
### Running(运行状态)
### Blocked(阻塞状态)
sleep(Thread)(自动醒)、wait(Object)(需要唤醒，等待其它的业务场景)
### Terminated(终止状态)

## 线程池
[参考](https://editor.csdn.net/md?articleId=121132400)
### 四种线程池
#### newCacheThreadPool
可缓存线程池，先查看池中有没有以前建立的线程，如果有，就直接使用。如果没有，就建一个新的线程加入池中，缓存型池子通常用于执行一些生存期很短的异步型任务

线程池为无限大，当执行当前任务时上一个任务已经完成，会复用执行上一个任务的线程，而不用每次新建线程
#### newFixedThreadPool
创建一个可重用固定个数的线程池，以共享的无界队列方式来运行这些线程。

因为线程池大小为3，每个任务输出打印结果后sleep 2秒，所以每两秒打印3个结果。
定长线程池的大小最好根据系统资源进行设置。如Runtime.getRuntime().availableProcessors()
#### newScheduledThreadPool
创建一个定长线程池，支持定时及周期性任务执行
```java
//创建一个定长线程池，支持定时及周期性任务执行——延迟执行
                 ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(5);
                 //延迟1秒执行
                 /*scheduledThreadPool.schedule(new Runnable() {
                     public void run() {
                        System.out.println("延迟1秒执行");
                     }
                 }, 1, TimeUnit.SECONDS);*/
                 
                 
                 //延迟1秒后每3秒执行一次
                 scheduledThreadPool.scheduleAtFixedRate(new Runnable() {
                     public void run() {
                         System.out.println("延迟1秒后每3秒执行一次");
                     }
                }, 1, 3, TimeUnit.SECONDS);
```
#### newSingleThreadExecutor
创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。

### 线程池 ThreadPoolExecutor
[文章](https://www.jianshu.com/p/f030aa5d7a28)
[文章](https://www.jianshu.com/p/c41e942bcd64)
| 名称 | 类型 | 含义 |
|--|--|--|
|corePoolSize	|int	|核心线程池大小|
|maximumPoolSize	|int|	最大线程池大小|
|keepAliveTime	|long	|线程最大空闲时间|
|	unit|	TimeUnit|	时间单位|
|	workQueue	|BlockingQueue<Runnable>	|线程等待队列|
|	threadFactory|	ThreadFactory	|线程创建工厂|
|	handler	|RejectedExecutionHandler|	拒绝策略|

## Java 17
[参考](https://blog.csdn.net/m0_50180963/article/details/120833102)

### 运行时错误
Unable to make protected final java.lang.Class java.lang.ClassLoader.defineClass
[文章1](https://monkeywie.cn/2021/11/18/java17-compatibility/)
[文章2](https://blog.csdn.net/m0_52155263/article/details/120867149)
-Dfile.encoding=UTF-8 // 设置文件编码
```java
-Dfile.encoding=UTF-8
--add-opens java.base/java.math=ALL-UNNAMED
--add-opens java.base/java.lang=ALL-UNNAMED
--add-opens java.base/java.lang.reflect=ALL-UNNAMED
--add-opens java.base/java.lang.invoke=ALL-UNNAMED
--add-opens java.base/java.util=ALL-UNNAMED
--add-opens java.base/java.nio=ALL-UNNAMED
--add-opens java.base/sun.nio.ch=ALL-UNNAMED
```

## private、protected、public和default的区别
[参考](https://www.cnblogs.com/jingmengxintang/p/5898900.html)

## 随机数加快
-Djava.security.egd=file:/dev/urandom 作用
[参考](https://codingdict.com/questions/121478)
[参考](https://blog.51cto.com/leo01/1795447)

## 开发格式规范
### 设计模式

工厂：Factory

代理：Proxy

观察；Observe

### mapper

选择：select

插入：input

更新：update

删除：delete

### 其他

详细：Detail

可选择的：optional

选择性的：selective

多个：Multiple

### Dao 接口命名

这里的方法名字最好对应着sql语句，这是最直接的。然后表示条件用By作为介词，表示查询列表用list前缀。

insert ：插入
batchInsert ：批量插入
selectOne ：查询一个数据
selectById ：查询通过xx条件
count ：计数
selectList ：查询多个数据
update ：更新
deleteById ：删除，通过某些条件

### Service 接口命名

这里用的是解决人类的思维，查询就是find，添加是add，删除就是remove，修改是modify。然后条件也用by，大量用list后缀。

add ：添加数据
findById ：查找，也可以用query
findByXXX ：查找
findXXXList ：批量查找
modify :修改
remove : 简单删除

### Service/DAO层命名规约

```php
        1.获取单个对象的方法用get做前缀
        2.获取多个对象的方法用list做前缀，如：listObjects
        3.获取统计值的方法用count做前缀
        4.插入方法用save/insert做前缀
        5.删除方法用delete/remove做前缀
        6.修改方法用update做前缀
```

### 其他命名规范

1. 项目名全部小写.
2. 包名全部小写.
3. 类名:大驼峰，例如：UpperCamelCase
4. 变量名,方法名：小驼峰：lowerCamelCase
5. 常量名全部大写: public static final int REQUEST_KEY_CODE =1;
6. 所有命名规则必须遵循以下规则 :

- 名称只能由字母、数字、下划线、$符号组成.
- 不能以数字开头.
- 名称不能使用Java中的关键字.
- 坚决不允许出现中文及拼音命名.

## 排序算法
### 冒泡排序的进行对比，然后交换

```
public static void BubbleSort(int[] arr) {
    //定义一个临时变量
    int temp;
    //冒泡趟数
    for (int i = 0; i < arr.length - 1; i++) {
        for (int j = 0; j < arr.length - i - 1; j++) {
            if (arr[j + 1] < arr[j]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
        System.out.println("第" + (i + 1) + "次：" + Arrays.toString(arr));
    }
}

public static void main(String[] args) {
    int arr[] = new int[]{1, 6, 2, 2, 5};
    冒泡排序.BubbleSort(arr);
    System.out.println(Arrays.toString(arr));
}
```

### 一: 区别

1.冒泡排序是比较相邻位置的两个数，而选择排序是按顺序比较，找最大值或者最小值；

2.冒泡排序每一轮比较后，位置不对都需要换位置，选择排序每一轮比较都只需要换一次位置；

3.冒泡排序是通过数去找位置，选择排序是给定位置去找数；

### 二: 冒泡排序优缺点

1.优点:比较简单,空间复杂度较低，是稳定的；

2.缺点:时间复杂度太高，效率慢；

### 三: 选择排序优缺点

1.优点：一轮比较只需要换一次位置；

2.缺点：效率慢，不稳定（举个例子5，8，5，2，9 我们知道第一遍选择第一个元素5会和2交换，那么原序列中2个5的相对位置前后顺序就破坏了）。

## jdk代理和CGLIB代理的区别
一、简单来说：

JDK动态代理`只能对实现了接口的类`生成代理，而`不能针对类`，它的实现原理是通过InvocationHandler.invoke方法实现对实现类方法的调用（InvocationHandler实例已经持有了对实现类对象的引用了），然后实现方法前后的拦截。

CGLIB是`针对类实现代理，主要是对指定的类生成一个子类`，覆盖其中的方法（`继承`），然后通过MethodIntercept.intercept方法来实现在调用父类方法的前后执行拦截。
　　
jdk代理是通过持有实现类的引用来实现对实现类方法的调用的，而CGLIB是通过调用父类的方法来实现对被代理类的方法调用的。

### 二、Spring在选择用JDK还是CGLiB的依据：

- (1)当Bean`实现接口`时，Spring就会`用JDK的动态代理`。
- (2)当Bean`没有实现接口`时，Spring`使用CGlib`实现。
- (3)可以强制使用CGlib（在spring配置中加入<aop:aspectj-autoproxy proxy-target-class=“true”/>）

### 三、CGlib比JDK快？

(1)使用CGLib实现动态代理，CGLib底层采用ASM字节码生成框架，使用字节码技术生成代理类，比使用Java反射效率要高。唯一需要注意的是，CGLib`不能对声明为final的方法进行代理`，因为CGLib原理是动态生成被代理类的子类。

(2)在对JDK动态代理与CGlib动态代理的代码实验中看，1W次执行下，JDK7及8的动态代理性能比CGlib要好20%左右。

*SpringBoot在*1.4版本后默认使用的*是cglib*动态代理,

选择排序的选择最大的，然后放到最后

```
public class 选择排序 {

    static int[] array = {3, 2, 4, 1, 5, 0};

    public static void chooseSort(int[] a) {

        int max = 0;
        int index = 0;

//外层循环，控制选择的次数，数组长度为6，一共需要选择5次

        for (int i = 0; i < a.length - 1; i++) {

            for (int j = 0; j < a.length - i; j++) {

                if (max < a[j]) {

                    max = a[j];

                    index = j;
                }
            }

//每次选择完成后，max中存放的是该轮选出的最大值

//将max指向位置的元素和数组最后一个元素位置互换
            int temp = a[a.length - i - 1];
            a[a.length - i - 1] = max;
            a[index] = temp;

//清空max和index，便于下次
            max = 0;
            index = 0;

            System.out.println("经过第" + (i + 1) + "轮选择后，数组为" + Arrays.toString(a));
        }
    }
    public static void main(String[] args) {
        chooseSort(array);
    }
}
```

## 配置环境变量
JAVA_HOME ——> +安装路径

**path**

%JAVA_HOME%\bin

%JAVA_HOME%\jre\bin
## 八大数据类型

- ##### byte 字节型

- ##### short 短整型

- ##### int 整型

- ##### long 长整型


- ##### float 单精度

- ##### double 双精度


- ##### boolean 布尔型

- ##### char 字符型

## 反射获取class
- 类名.class
- Class.forName(类名)
- 通过类对象获取
- 通过类加载器获取

volatile 可见性 禁止指令重排序优化
## 双亲委派机制的工作流程：
1. 当前ClassLoader首先从自己已经加载的类中查询是否此类已经加载，如果已经加载则直接返回原来已经加载的类。

每个类加载器都有自己的加载缓存，当一个类被加载了以后就会放入缓存，等下次加载的时候就可以直接返回了。

2. 当前classLoader的缓存中没有找到被加载的类的时候，委托父类加载器去加载，父类加载器采用同样的策略，首先查看自己的缓存，然后委托父类的父类去加载，一直到bootstrp ClassLoader.

3. 当所有的父类加载器都没有加载的时候，再由当前的类加载器加载，并将其放入它自己的缓存中，以便下次有加载请求的时候直接返回。
### 总结：

“双亲委派”机制只是Java推荐的机制，并不是强制的机制。

我们可以继承java.lang.ClassLoader类，实现自己的类加载器。如果想保持双亲委派模型，就应该重写findClass(name)方法；如果想破坏双亲委派模型，可以重写loadClass(name)方法

类装载线程安全

## 泛型
泛型的本质是参数化类型，这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型接口、泛型方法。
在 Java SE 1.5 之前没有泛型的情况的下只能通过对类型 Object 的引用来实现参数的任意化，其带来的缺点是要做显式强制类型转换，而这种强制转换编译期是不做检查的，容易把问题留到运行时，
所以 泛型的好处是在编译时检查类型安全，并且所有的强制转换都是自动和隐式的，提高了代码的重用率，避免在运行时出现 ClassCastException。
## 类加载顺序
(祖)(父)(自己)类静态代码块 --> (祖)(父)(自己)实例化方法然后构造方法

如果调用静态方法

(祖)(父)类静态代码块 --> (祖)(父)(自己)实例化方法然后构造方法 --> (自己)类静态代码块

结论：如果调用静态方法，静态方法最后执行

## 设计模式描述
### 本质
维护性，通用性，扩展性，降低复杂度
### 七大原则
#### 单一职责原则
一个类负责一个功能
#### 接口隔离原则
接口细化
#### 依赖到转（到置）原则
依赖关系传递的三种方式
接口传递
构造方法传递
setter方式传递
#### 里氏替换原则
尽量不要重写夫类方法
#### 开闭原则（ocp）原则
对扩展方开放（提供方），对使用方关闭（消费方）使用接口，抽象
#### 迪米特法则原则
直接朋友
成员变量，方法参数，方法返回值
一个类不要有其他朋友的直接逻辑代码
把逻辑代码写到该类中（单一职责）
#### 合成复用原则
尽量不要用继承
使用方法传参（依赖）
类内部变量使用set方法（聚合）
类内部变量并创建对象（组合）
### 三种类型，共23种
#### 创建型（5种）
##### 单例模式（8种写法）
###### 饿汉式（两种）
写法简单
如果不用会浪费内存

- 静态常量
- 静态代码块
###### 懒汉式（三种）
- 线程不安全
- 线程安全（同步方法）
- 线程安全（同步代码块）
###### 双重检查
###### 静态内部类
###### 枚举
##### 抽象工厂模式（Factory）
逻辑判断代码，创建对象代码和执行方法分离
抽象工厂类创建抽象方法，由子类实现
执行方法分离抽象工厂类，调用抽象工厂方法
产品分离，工厂分离
##### 原型模式（克隆）
###### 浅拷贝
实现接口Cloneable从写cloue()方法
属性里面有引用对象不会拷贝对象
会直接引用原类里引用对象的地址
###### 深拷贝
实现接口Cloneable从写cloue()方法
手写代码克隆引用对象 引用对象类需要实现克隆
属性里面有引用对象会重新拷贝一份对象
###### 序列化
实现接口Serializable

代码实现
//创建字节输入流
bos = new ByteArrayOutputStream();
//创建对象输入流
oop = new ObjectOutputStream(bos);
//写入对象
oop.writeObject(this);
//创建字节输出流
bis = new ByteArrayInputStream(bos.toByteArray());
//创建对象输出流获取对象
oip = new ObjectInputStream(bis);
##### 建造者模式（产品差异性不大）
###### 默认
执行流程和建造过程在同一个类中
###### 实现
分离执行流程和建造流程

##### 工厂模式
###### 默认
逻辑判断代码，创建对象代码以及执行方法在一起
###### 简单
逻辑判断代码，创建对象代码和执行方法分离
方法调用或者静态方法调用
###### 方法
逻辑判断代码，创建对象代码和执行方法分离
工厂类创建执行方法
逻辑判断代码，创建对象代码由子类实现
执行方法分离工厂类，调用工厂方法
产品分离，工厂不分离


#### 结构型（7种）
##### 适配器模式
###### 类适配器
就是继承，然后调用方法
###### 对象适配器
构造函数传递类，调用类方法
###### 接口适配器
创建一个接口，写出方法
创建一个抽象类，重写方法
new一个抽象类，根据需要重写实现方法
##### 桥接模式（Bridge）
品牌手机：手机品牌，手机样式
手机接口：
多个手机品牌实现手机接口
手机样式抽象类：
组合手机接口，多个手机样式继承手机样式抽象类
创建手机样式，传递手机品牌，返回手机

1）对于那些不希望使用继承或因为多层次继承导致系统类的个数急剧增加的系统，桥接模式无为适用.
2)常见的应用场景:
-JDBc驱动程序
-银行转账系统
转账分类:网上转账,柜台转账，AMT车转专账
转账用户类型:普通用户，银卡用户,金卡用户..
-消息管理
消息类型:即时消息,延时消息
消息分类:手机短信，邮件消息，QQ消息...
##### 装饰模式
装饰者包含被装饰者
动态的将新功能附加到对象上
##### 组合模式
##### 外观模式
##### 享元模式
##### 代理模式
#### 行为型（11种）
##### 模版方法模式
##### 命令模式
##### 访问者模式
##### 迭代器模式

## 万能时间格式化
```java
    public static void main(String[] args) {
        String pattern ="[[yyyy][yy][-][/][][MM][M][-][/][][dd][d][['T'][ ][HH][H][:][mm][m][:][ss][s][.SSSSSSSz][.SSS[XXX][X]]]]";
[[yyyy][yy][-][/][.][][MM][M][-][/][.][][dd][d][['T'][ ][HH][H][:][mm][m][:][ss][s][.SSSSSSS][.SSSSSSSz][.SSS[XXX][X]]]]
        String timeSample ="2018-5-04 3:";
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern(pattern);
        TemporalAccessor accessor = formatter.parse(timeSample);
        System.out.println(LocalDateTime.from(accessor).format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
    }
```

## Accessors
// @Accessors(chain = true) // 导入不允许使用 会找不到set方法

# 配置环境变量
JAVA_HOME ——> +安装路径

**path**

%JAVA_HOME%\bin

%JAVA_HOME%\jre\bin
# 八大数据类型

- ##### byte 字节型

- ##### short 短整型

- ##### int 整型

- ##### long 长整型


- ##### float 单精度

- ##### double 双精度


- ##### boolean 布尔型

- ##### char 字符型

# 反射获取class
- 类名.class
- Class.forName(类名)
- 通过类对象获取
- 通过类加载器获取
# @Inherited注解的作用
[@Inherited注解的作用](https://www.jianshu.com/p/7f54e7250be3)

# JAVA

volatile 可见性 禁止指令重排序优化

# 双亲委派机制的工作流程：

1. 当前ClassLoader首先从自己已经加载的类中查询是否此类已经加载，如果已经加载则直接返回原来已经加载的类。

每个类加载器都有自己的加载缓存，当一个类被加载了以后就会放入缓存，等下次加载的时候就可以直接返回了。

2. 当前classLoader的缓存中没有找到被加载的类的时候，委托父类加载器去加载，父类加载器采用同样的策略，首先查看自己的缓存，然后委托父类的父类去加载，一直到bootstrp ClassLoader.

3. 当所有的父类加载器都没有加载的时候，再由当前的类加载器加载，并将其放入它自己的缓存中，以便下次有加载请求的时候直接返回。

------

总结：

“双亲委派”机制只是Java推荐的机制，并不是强制的机制。

我们可以继承java.lang.ClassLoader类，实现自己的类加载器。如果想保持双亲委派模型，就应该重写findClass(name)方法；如果想破坏双亲委派模型，可以重写loadClass(name)方法

类装载线程安全

# 泛型

泛型的本质是参数化类型，这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型接口、泛型方法。
在 Java SE 1.5 之前没有泛型的情况的下只能通过对类型 Object 的引用来实现参数的任意化，其带来的缺点是要做显式强制类型转换，而这种强制转换编译期是不做检查的，容易把问题留到运行时，
所以 泛型的好处是在编译时检查类型安全，并且所有的强制转换都是自动和隐式的，提高了代码的重用率，避免在运行时出现 ClassCastException。

## 上传文件问题
StandardMultipartHttpServletRequest
parseRequest
文件上传慢

## java 混淆
[allatori](https://allatori.com/)
