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
