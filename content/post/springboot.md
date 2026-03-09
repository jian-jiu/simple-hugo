---
title: springboot
date: 2026-03-09
slug: springboot
categories:
    - springboot
tags:
    - springboot
---

## 把接口参数中的空白值替换为null值
[参考文档](https://zhuanlan.zhihu.com/p/344190594)

## xxx-job分布式定时器
[详情](https://www.jianshu.com/p/fa7186bea84b)

## Velocity java代码生成
[基本用法](https://blog.csdn.net/nengyu/article/details/6671904)

## 后台接口自动生成
- 注解侵入性严重
  [Swagger详情](https://www.jianshu.com/p/12f4394462d5)
  [knife4j详情](https://blog.csdn.net/weixin_35681869/article/details/104809867) 比Swagger好

- 无注解侵入性严重
  [smart-doc](https://blog.csdn.net/weixin_42543046/article/details/114931230)


## 事件发布以及注册
[参考](https://www.cnblogs.com/juncaoit/p/13275339.html)

# 注意事项：
1、如果2个事件之间是继承关系，会先监听到子类事件，处理完再监听父类。

2、监听器方法一定要try-catchy异常，否则会造成发布事件（有事务的）的方法进行回滚

3、可以使用@Order注解控制多个监听器的执行顺序，@Order 传入的值越小，执行顺序越高

## 缓存注解@Cacheable、@CacheEvict、@CachePut使用
[详情](https://www.cnblogs.com/fashflying/p/6908028.html)


## spring整个Bean初始化中的执行顺序：
Constructor(构造方法) -> @Autowired(依赖注入) -> @PostConstruct(注释的方法)
## @PostConstruct
该注解被用来修饰一个void（）方法。被@PostConstruct修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器执行一次。PostConstruct在构造函数之后执行，init（）方法之前执行

## @Scope（@Scope("prototype")）
单例变原型
## spring bean作用域有以下5个：
- singleton：单例模式，当spring创建applicationContext容器的时候，spring会欲初始化所有的该作用域实例，加上lazy-init就可以避免预处理；

- prototype：原型模式，每次通过getBean获取该bean就会新产生一个实例，创建后spring将不再对其管理；

（下面是在web项目下才用到的）

- request：搞web的大家都应该明白request的域了吧，就是每次请求都新产生一个实例，和prototype不同就是创建后，接下来的管理，spring依然在监听；

- session：每次会话，同上；

- global session：全局的web域，类似于servlet中的application。

## 定时器
@Scheduled(fixedRate = 1000)
@Scheduled(cron = "30 * * * * ?")
@EnableScheduling