---
title: maven
date: 2026-04-02
slug: maven
categories:
    - maven
tags:
    - maven
---

## 使用idea 无法下载源码
[解决](https://www.cnblogs.com/dasusu/p/11825786.html)
Sources not found for:
解决方案：在对应的pom.xml 文件中打开 terminal，执行 mvn命令：
```shell
mvn dependency:sources
mvn dependency:resolve -Dclassifier=javadoc
```
## 下载源配置
[idea maven 参考](https://blog.csdn.net/CringKong/article/details/89414445)
修改idea 默认maven配置
```
安装目录下 IntelliJ IDEA\plugins\maven\lib\maven3\conf
```
```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
<!--  本地仓库配置  -->
<localRepository>D:\BianCheng\Repositories\Maven</localRepository>
<pluginGroups> </pluginGroups>
<proxies> </proxies>
<servers> </servers>
<mirrors>
<!--  nexus-aliyun 首选，放第一位,有不能下载的包，再去做其他镜像的选择   -->
<!--  阿里云镜像 -->
<mirror>
<id>nexus-aliyun</id>
<mirrorOf>central</mirrorOf>
<name>Nexus aliyun</name>
<url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
<!--  maven官方2号镜像 -->
<mirror>
<id>repo2</id>
<name>Mirror from Maven Repo2</name>
<url>http://repo2.maven.org/maven2/</url>
<mirrorOf>central</mirrorOf>
</mirror>
<!--  maven的UK镜像 -->
<mirror>
<id>ui</id>
<name>Mirror from UK</name>
<url>http://uk.maven.org/maven2/</url>
<mirrorOf>central</mirrorOf>
</mirror>
<!--  JBoss 镜像 -->
<mirror>
<id>jboss-public-repository-group</id>
<mirrorOf>central</mirrorOf>
<name>JBoss Public Repository Group</name>
<url>http://repository.jboss.org/nexus/content/groups/public</url>
</mirror>
<mirror>
<id>mirrorId</id>
<mirrorOf>central</mirrorOf>
<name>Human Readable Name for this Mirror.</name>
<url>http://central.maven.org/maven2/</url>
</mirror>
</mirrors>
<profiles> </profiles>
</settings>
```
## scope各种取值详解
|scope取值	|有效范围（compile, runtime, test）|	依赖传递|	例子|
|--|--|--|--|
|compile|	all|	是	|spring-core|
|provided	|compile, test|	否	|servlet-api|
|runtime	|runtime, test|	是	|JDBC驱动|
|test	test	|否	|JUnit|
|system	|compile, test	|是|

compile ：为默认的依赖有效范围。如果在定义依赖关系的时候，没有明确指定依赖有效范围的话，则默认采用该依赖有效范围。此种依赖，在编译、运行、测试时均有效。

provided ：在编译、测试时有效，但是在运行时无效。例如：servlet-api，运行项目时，容器已经提供，就不需要Maven重复地引入一遍了。

runtime ：在运行、测试时有效，但是在编译代码时无效。例如：JDBC驱动实现，项目代码编译只需要JDK提供的JDBC接口，只有在测试或运行项目时才需要实现上述接口的具体JDBC驱动。

test ：只在测试时有效，例如：JUnit。

system ：在编译、测试时有效，但是在运行时无效。和provided的区别是，使用system范围的依赖时必须通过systemPath元素显式地指定依赖文件的路径。由于此类依赖不是通过Maven仓库解析的，而且往往与本机系统绑定，可能造成构建的不可移植，因此应该谨慎使用。systemPath元素可以引用环境变量。

排除jar
[参考1](https://zhuanlan.zhihu.com/p/345268828)
