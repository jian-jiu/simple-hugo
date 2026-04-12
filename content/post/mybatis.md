---
title: mybatis
date: 2026-04-02
slug: mybatis
categories:
    - mybatis
tags:
    - mybatis
    - mybatis plus
---

## mysql null count
[参考](https://cloud.tencent.com/developer/article/1658064)

### count(1)、count(*)、count（字段） 推荐count(*)
[参考](https://www.cnblogs.com/hider/p/11726690.html)

## mybatis plus 热更新
[参考](https://www.cnblogs.com/oskyhg/p/8587701.html)
[参考](https://blog.csdn.net/hanghangaidoudou/article/details/99301625)
[参考](https://blog.csdn.net/sinat_36106456/article/details/88875355)

## 备份恢复xbstream
```sh
yum -y install https://repo.percona.com/yum/percona-release-latest.noarch.rpm
yum -y  install percona-xtrabackup-24
yum -y  install qpress
innobackupex -version
# 解压
xbstream -x -C ./simple/ < **.xbstream 
# 再次解压
innobackupex --decompress --parallel=6 --compress-threads=6 ./simple/
# 然后新建数据库 表 
alter table xxx DISCARD TABLESPACE;
alter table xxx IMPORT TABLESPACE;
# 放入ibd 完成！
```

## typeHandler问题
### JsonArrayTypeHandler
```java
import com.baomidou.mybatisplus.core.toolkit.Assert;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JavaType;
import com.fasterxml.jackson.databind.type.TypeFactory;
import com.simple.common.json.utils.JsonUtils;
import lombok.extern.slf4j.Slf4j;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;
import org.springframework.core.ResolvableType;

import java.io.IOException;
import java.lang.reflect.Type;

/**
 * 自定义json类型处理类(支持泛型)
 *
 * @author Simple
 */
@Slf4j
@MappedTypes({Object.class})
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonArrayTypeHandler<T> extends AbstractJsonTypeHandler<T> {

    private final JavaType javaType;

    public JsonArrayTypeHandler() {
        ResolvableType resolvableType = ResolvableType.forClass(getClass());
        Type type = resolvableType.as(JsonArrayTypeHandler.class).getGeneric().getType();
        javaType = constructType(type);
    }

    public static JavaType constructType(Type type) {
        Assert.notNull(type, "[Assertion failed] - type is required; it must not be null");
        return TypeFactory.defaultInstance().constructType(type);
    }

    @Override
    protected T parse(String json) {
        try {
            System.out.println(javaType);
            return JsonUtils.getObjectMapper().readValue(json, javaType);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    protected String toJson(T obj) {
        try {
            return JsonUtils.getObjectMapper().writeValueAsString(obj);
        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }
    }

}

```

### 使用
```java
import com.baomidou.mybatisplus.extension.handlers.JsonArrayTypeHandler;
import x.domain.x;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.util.ArrayList;
import java.util.List;

/**
 * @author Simple
 */
@MappedJdbcTypes(JdbcType.VARCHAR)
@MappedTypes(ArrayList.class)
public class xTypeHandler extends JsonArrayTypeHandler<List<x>> {

}

```

## Mysql数据库导出导入中GTID引发的问题
[参考](https://blog.csdn.net/qq411139073/article/details/130235445)

# 修改密码
[参考](https://cloud.tencent.com/developer/article/2337586)

# 环境变量
## lower_case_table_names(推荐1)
[参考](https://zhuanlan.zhihu.com/p/606877902)
[参考](https://blog.csdn.net/wangkun_j/article/details/82190815)

# UML图 StarUML汉化版
[参考](https://github.com/raccoon666666/StarUML-chinese)
## explicit_defaults_for_timestamp
[参考](https://www.cnblogs.com/wqbin/p/12017308.html)
# 安装
①安装服务：mysqld --install



②初始化：　mysqld --initialize --console



③开启服务：net start mysql



④关闭服务：net stop mysql



⑤登录mysql：mysql -u root -p



Enter PassWord：(密码)



⑥修改密码：alter user 'root'@'localhost' identified by 'root';(by 接着的是密码)



⑦标记删除mysql服务：sc delete mysql
```
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[mysqld]
# 设置3306端口
port = 3306 
# 设置mysql的安装目录
basedir=E:\must\mysql-8.0.30-winx64
# 设置mysql数据库的数据的存放目录
datadir=E:\must\mysql-8.0.30-winx64\data
# 允许最大连接数
max_connections=200
# 设置mysql服务端默认字符集
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB 
```

# 8个 character_set 变量说明
[参考文档](https://blog.csdn.net/sun8112133/article/details/79921734)

# Mysql引擎概述+索引+sql优化
[详情](https://www.cnblogs.com/jameszheng/p/10245455.html)

# MySql 重置/修改表自增ID值
[详情](https://blog.csdn.net/qq_22241923/article/details/106158667)
# mysql 缓存
[详情](https://www.cnblogs.com/applelife/p/11576295.html)
查询缓存功能在 MySQL 8.0 中已经被删除，have_query_cache 的值永远都是 NO，将来会被删除
[删除原因](https://blog.csdn.net/weixin_42527464/article/details/113295342)

# proxysql
[官网](https://proxysql.com/documentation/)
[使用](https://zhuanlan.zhihu.com/p/110733834)

# 通用属性
```sql
  `create_by` bigint unsigned NOT NULL COMMENT '创建者',
  `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_by` bigint unsigned DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  `status` int unsigned NOT NULL COMMENT '状态',
  `remark` varchar(500) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '备注',
  `del_flag` tinyint unsigned NOT NULL DEFAULT '0' COMMENT '删除标志（0没有，1已删除）',
```

# 修改表方法
[参考](https://www.cnblogs.com/zsg88/p/7818684.html)
[参考](http://c.biancheng.net/view/2433.html)




## MySQL

树化的条件是数组长度大于64并且，链表节点到达8个才会树化，否则是扩容

红黑树的平均查找长度是log(n)，如果长度为8，平均查找长度为log(8)=3，链表的平均查找长度为n/2，当长度为8时，平均查找长度为8/2=4，红黑树的查找效率更高，这才有转换成树的必要；
链表长度如果是小于等于6，6/2=3，而log(6)=2.6，虽然速度也很快的，但是转化为树结构和生成树的时间并不会太短

# 缓存

一级缓存区域是根据SqlSession为单位划分的

SqlSession执行insert、update、delete等操作commit后会清空该SQLSession缓存

二级缓存是mapper级别的。Mybatis默认是没有开启二级缓存

如果调用相同namespace（命名空间）下的mapper映射文件中的增删改SQL，并执行了commit操作。此时会清空该namespace下的二级缓存

一对一映射 association

一对多映射 collection

## mysql语句详细信息

EXPLAIN

id select查询的序列号，包含一组数字，表示查询中执行select子句或操作表的顺序


### 进入数据库

mysql --hip -P端口 u名字 -p密码

## 强制使用索引

表名 force index(字段)

### 命令分类

- 对库操作：	DDL

- 增删改操作：	DML

- 查询操作：	MQL

- 事务操作：	TCL

- 权限控制：	DCL

### 数据库外操作

创建库：				create database 库名字
删除库：				drop database 库名字
进入库：				use 库名字
查看所有库：		show databases
查看当前库表：	show tables
添加本地表（导入sql需要先创建库进入库）：		source 路径
显示表结构：		desc 表名称
查看表数据：		select 字段名称{,字段名称} from 表名称

### 优先级

**where和having只能存在一个**
查询：	select
表：	from

筛选：	where
分组：	group by
筛选：	having
条件：	order by
分页：	limit

### 运行顺序

from
where
group by
having
select
order by
limit

having必须和分组group by 一起出现
where是分组之前进行筛选
having是分组之后进行筛选

### 增加删除修改

- **查询：select * from 表名**
- **增加：insert into 表名（，，，）values（，，，），（，，，）**
- **修改：update 表名 set 字段=值，，， {where条件}**
- **删除：delete from 表名 {where条件}**
- **insert into 表名 select * 表2 where**

### 函数

位置：select having后面
最大值：	max
最小值：	min
和：	sum
平均数：	avg
统计数量：count

转换大写：		lower
转换小写：		upper
取子串(数据，下标，长度)：	substr
取长度：			length
去空格：			trim
字符串转换日期：		str_to_dare
格式化日期：		data_format
设置千分位：(数据，需要长度)	format
四舍五入：(数据，需要长度)	round09
生成随机数：(种子)		rand
把null转换为具体值：(数据,0)	ifnull
获取当前时间：		now
返回一个不小于原数据的整数：	ceil



字符串转换成为数字cast(字符串 as signed integer)



### 条件查询

**from 表名 where 条件！**
and	并且
or	或者
between … and …. 两个值之间
is null	不为空where后面
in() 	包含
like	模糊查询
(%表示0到多个)如：like '%a%'
(_下划线表示1个字符)

排序：from 表名 ordre by 排序字段
默认正序asc
倒序desc

分组：		   group by
分组之后进行条件筛选：having

### 连接有关

内连接：inner join on() | join
外连接：
左外连接：left outer join on
右外连接：right outer jion on

全连接：union 不去重复：union all

distinct 去除重复的记录

结论：如果外键不能为空，使用内连接;
如果外键即使可以为空，但是要查询的数据要求在两张表中都要有相对应的记录，也使用内连接；
如果外键可以为空，又要把某一张表中所有符合条件的记录都查出来，即使在另一张表中没有相对应的记录，使用外间接。

### 主键

不为空：not null
不重复：unique

主键：primary

### 事务

- commit 提交

- rollback 回滚

start transaction 临时性
set auto_commit = true/false no/yes 永久性

#### 特性

	A：原子性
		说明事务是最小的工作单元。不可再分。
	
	C：一致性
		所有事务要求，在同一个事务当中，所有操作必须同时成功，或者同时失败，
		以保证数据的一致性。
	
	I：隔离性
		A事务和B事务之间具有一定的隔离。
		教室A和教室B之间有一道墙，这道墙就是隔离性。
		A事务在操作一张表的时候，另一个事务B也操作这张表会那样？？？
	
	D：持久性
		事务最终结束的一个保障。事务提交，就相当于将没有保存到硬盘上的数据
		保存到硬盘上！
#### 隔离性

查看；SELECT @@tx_isolation

读未提交：read uncommitted（最低的隔离级别）《没有提交就读到了》
什么是读未提交？
事务A可以读取到事务B未提交的数据。
这种隔离级别存在的问题就是：
脏读现象！(Dirty Read)
我们称读到了脏数据。
这种隔离级别一般都是理论上的，大多数的数据库隔离级别都是二档起步！

读已提交：read committed《提交之后才能读到》
什么是读已提交？
事务A只能读取到事务B提交之后的数据。
这种隔离级别解决了什么问题？
解决了脏读的现象。
这种隔离级别存在什么问题？
不可重复读取数据。
什么是不可重复读取数据呢？
在事务开启之后，第一次读到的数据是3条，当前事务还没有
结束，可能第二次再读取的时候，读到的数据是4条，3不等于4
称为不可重复读取。

这种隔离级别是比较真实的数据，每一次读到的数据是绝对的真实。
oracle数据库默认的隔离级别是：read committed

可重复读：repeatable read《提交之后也读不到，永远读取的都是刚开启事务时的数据》
什么是可重复读取？
事务A开启之后，不管是多久，每一次在事务A中读取到的数据
都是一致的。即使事务B将数据已经修改，并且提交了，事务A
读取到的数据还是没有发生改变，这就是可重复读。
可重复读解决了什么问题？
解决了不可重复读取数据。
可重复读存在的问题是什么？
可以会出现幻影读。
每一次读取到的数据都是幻象。不够真实！

早晨9点开始开启了事务，只要事务不结束，到晚上9点，读到的数据还是那样！
读到的是假象。不够绝对的真实。

mysql中默认的事务隔离级别就是这个！！！！！！！！！！！

序列化/串行化：serializable（最高的隔离级别）
这是最高隔离级别，效率最低。解决了所有的问题。
这种隔离级别表示事务排队，不能并发！
synchronized，线程同步（事务同步）
每一次读取到的数据都是最真实的，并且效率是最低的。

## 存储引擎

show engines \G

它管理的表具有以下特征：
使用三个文件表示每个表：
格式文件 — 存储表结构的定义（mytable.frm）
数据文件 — 存储表行的内容（mytable.MYD）
索引文件 — 存储表上索引（mytable.MYI）：索引是一本书的目录，缩小扫描范围，提高查询效率的一种机制。
可被转换为压缩、只读表来节省空间

		提示一下：
			对于一张表来说，只要是主键，
			或者加有unique约束的字段上会自动创建索引。
	
		MyISAM存储引擎特点：
			可被转换为压缩、只读表来节省空间
			这是这种存储引擎的优势！！！！
		
		MyISAM不支持事务机制，安全性低。

### InnoDB存储引擎？

​	这是mysql默认的存储引擎，同时也是一个重量级的存储引擎。
​	InnoDB支持事务，支持数据库崩溃后自动恢复机制。
​	InnoDB存储引擎最主要的特点是：非常安全。

适用场景：

1）经常更新的表，适合处理多重并发的更新请求。

2）支持事务。

3）可以从灾难中恢复（通过bin-log日志等）。

4）外键约束。只有他支持外键。

5）支持自动增加列属性auto_increment。

这里顺便提一下事务的特点(ACID)：

A 事务的原子性(Atomicity)：指一个事务要么全部执行,要么不执行.也就是说一个事务不可能只执行了一半就停止了.比如你从取款机取钱,这个事务可以分成两个步骤:1划卡,2出钱.不可能划了卡,而钱却没出来.这两步必须同时完成.要么就不完成.

C 事务的一致性(Consistency)：指事务的运行并不改变数据库中数据的一致性.例如,完整性约束了a+b=10,一个事务改变了a,那么b也应该随之改变.

I 独立性(Isolation）:事务的独立性也有称作隔离性,是指两个以上的事务不会出现交错执行的状态.因为这样可能会导致数据不一致.

D 持久性(Durability）:事务的持久性是指事务执行成功以后,该事务所对数据库所作的更改便是持久的保存在数据库之中，不会无缘无故的回滚.

	它管理的表具有下列主要特征：
		– 每个 InnoDB 表在数据库目录中以.frm 格式文件表示
		– InnoDB 表空间 tablespace 被用于存储表的内容（表空间是一个逻辑名称。表空间存储数据+索引。）
	
		– 提供一组用来记录事务性活动的日志文件
		– 用 COMMIT(提交)、SAVEPOINT 及ROLLBACK(回滚)支持事务处理
		– 提供全 ACID 兼容
		– 在 MySQL 服务器崩溃后提供自动恢复
		– 多版本（MVCC）和行级锁定
		– 支持外键及引用的完整性，包括级联删除和更新
	
	InnoDB最大的特点就是支持事务：
		以保证数据的安全。效率不是很高，并且也不能压缩，不能转换为只读，
		不能很好的节省存储空间。

### MyIsam内存引擎

MyIASM是MySQL默认的引擎，但是它没有提供对数据库事务的支持，也不支持行级锁和外键，因此当INSERT(插入)或UPDATE(更新)数据时即写操作需要锁定整个表，效率便会低一些。

MyIsam 存储引擎独立于操作系统，也就是可以在windows上使用，也可以比较简单的将数据转移到linux操作系统上去。

适用场景：

1）不支持事务的设计，但是并不代表着有事务操作的项目不能用MyIsam存储引擎，可以在service层进行根据自己的业务需求进行相应的控制。

2）不支持外键的表设计。

3）查询速度很快，如果数据库insert和update的操作比较少的话比较适用。

4）整天 对表进行加锁的场景。

5）MyISAM极度强调快速读取操作。

6）MyIASM中存储了表的行数，于是SELECT COUNT(*) FROM TABLE时只需要直接读取已经保存好的值而不需要进行全表扫描。如果表的读操作远远多于写操作且不需要数据库事务的支持，那么MyIASM也是很好的选择。

### MEMORY存储引擎？

使用存在内存中的内容来创建表。每个MEMORY表只实际对应一个磁盘文件。MEMORY类型的表访问非常得快，因为它的数据是放在内存中的，并且默认使用HASH索引。

但是一旦服务关闭，表中的数据就会丢失掉。

适用场景：

1）那些内容变化不频繁的代码表，或者作为统计操作的中间结果表，便于高效地堆中间结果进行分析并得到最终的统计结果。

2）目标数据比较小，而且非常频繁的进行访问，在内存中存放数据，如果太大的数据会造成内存溢出。可以通过参数max_heap_table_size控制Memory表的大小，限制Memory表的最大的大小。

3）数据是临时的，而且必须立即可用得到，那么就可以放在内存中。

4）存储在Memory表中的数据如果突然间丢失的话也没有太大的关系。

​	使用 MEMORY 存储引擎的表，其数据存储在内存中，且行的长度固定，
​	这两个特点使得 MEMORY 存储引擎非常快。

MEMORY 存储引擎管理的表具有下列特征：
– 在数据库目录内，每个表均以.frm 格式的文件表示。
– 表数据及索引被存储在内存中。（目的就是快，查询快！）
– 表级锁机制。
– 不能包含 TEXT 或 BLOB 字段。

MEMORY 存储引擎以前被称为HEAP 引擎。

MEMORY引擎优点：查询效率是最高的。不需要和硬盘交互。
MEMORY引擎缺点：不安全，关机之后数据消失。因为数据和索引都是在内存当中。

## 索引

目前索引方式有HASH和BTREE索引，MyISAM存储引擎支持FULLTEXT索引。

索引分类

普通索引：MySQL中基本索引类型，没有什么限制，允许在定义索引的列中插入重复值和空值。

唯一索引：索引列中的值必须是唯一的，但是允许为空值。

主键索引：是一种特殊的唯一索引，不允许有空值。

组合索引：在表中的多个字段组合上创建的索引，只有在查询条件中使用了这些字段的左边字段时，索引才会被使用，使用组合索引时遵循最左前缀集合。

全文索引：全文索引，只有在MyISAM引擎上才能使用，只能在CHAR,VARCHAR,TEXT类型字段上使用全文索引。

空间索引：空间索引是对空间数据类型的字段建立的索引，MySQL中的空间数据类型有四种，GEOMETRY、POINT、LINESTRING、POLYGON。在创建空间索引时，使用SPATIAL关键字。要求，引擎为MyISAM，创建空间索引的列，必须将其声明为NOT NULL。

3.索引优化

1）NULL值的列：只要列中包含有NULL值都将不会被包含在索引中，复合索引中只要有一列含有NULL值，那么这一列对于此复合索引就是无效的。所以我们在数据库设计时不要让字段的默认值为NULL。

2）索引列排序：MySQL查询只使用一个索引，因此如果where子句中已经使用了索引的话，那么order by中的列是不会使用索引的。因此数据库默认排序可以符合要求的情况下不要使用排序操作；尽量不要包含多个列的排序，如果需要最好给这些列创建复合索引。

3）like语句：like “%aaa%” 不会使用索引，而like “aaa%”可以使用索引，建议没有必要的话尽量不要使用like。

4）避免在列上使用函数或运算：

5）组合索引顺序：对多列进行索引(组合索引)，列的顺序非常重要，MySQL仅能对索引最左边的前缀进行有效的查找。例如：
假设存在组合索引indexc1c2(c1,c2)，查询语句select * from t1 where c1=1 and c2=2能够使用该索引。查询语句select * from t1 where c1=1也能够使用该索引。但是，查询语句select * from t1 where c2=2不能够使用该索引。

6) 值重复少的字段不适合建索引，例：性别。

7）用or分割的条件，如果or前的条件中的列有索引，而后面的列没有索引，那么涉及的索引都不会用到。

<,<=,=,>,>=,between,in,以及不以通配符%或_开头的like才会使用索引。索引也不是越多越好，索引会占用空间和内存，多了维护成本也很高，更新数据时也会慢。

4.其他sql方面的优化

避免使用select *。

用count(1) 或count(列)代替count(*)

避免用or，可以用in或union代替。**
**

用join代替子查询。

用exists代替in。

尽量不用not in，<>，避免扫描全表。

使用批量插入。

避免数据类型不一致。

## 批量插入更新
[详情](https://www.jianshu.com/p/df14fa887b85)
[全局变量](https://blog.csdn.net/mashangzhifu/article/details/122845644)
![方法说明](https://cdn.242499.xyz/2026/04/12/30fec343871a97bbd2b692f214cdd9c4.png)

## 数据库中 PK FK UK CK DF 的意思
PK 主键 constraint primary key
FK 主外键关系 constraint foreign references
UK 唯一约束 constraint unique key
CK 检查约束 constraint check
DF 约束默认 constraint default for
