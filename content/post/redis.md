---
title: redis
date: 2026-03-09
slug: redis
categories:
    - redis
tags:
    - redis
---

## docker 中数据持久化配置
[详情](https://www.cnblogs.com/fuhai0815/p/12766485.html)

## 实现分布式锁和等待序列
[详情](https://blog.csdn.net/LMY413231718/article/details/95244258)

# win软件

## AnotherRedisDesktopManager
[github](https://github.com/qishibo/AnotherRedisDesktopManager/)
## Tiny RDM
[官网](https://redis.tinycraft.cc/zh/)
[github](https://github.com/tiny-craft/tiny-rdm)


# 安装
## 下载安装
[网页](https://www.runoob.com/redis/redis-install.html)
## win加入自启动运行命令
```shell
redis-server --service-install redis.windows-service.conf --loglevel verbose
```
执行之后重启或者去我的电脑哪里启动一次
## 其他命令
```shell
常用的redis服务命令。

卸载服务：redis-server --service-uninstall

开启服务：redis-server --service-start

停止服务：redis-server --service-stop
```
# 配置文件
[参考](https://www.cnblogs.com/fuhai0815/p/12766485.html)
# 数据类型
## String
最大512M）（可以是任意值数据类型、可以是Object）
通常场景为：存储结果集（Hash可以取代），存储UUID（分布式锁ID）、计数器（点赞）
特殊功能：
SETNX（判断是否可以进KEY，如果有KEY存在就不保存）
INCR、DECR（自增、自减）
SETRANGE （偏移量，就是截取字符串）
MSET（批量设置）
## List
列表、堆栈、队列、可重复元素
通常场景为：新闻列表、各种列表、模拟队列、模拟堆栈、数据表的主键存储（与hash做关联关系）
特殊功能：（L+L和R+R都是堆栈、L+R是队列）（批量操作元素以空格隔开）
LPUSH、LPOP （头部插入、弹出）
RPUSH、RPOP （尾部插入、弹出）
BLPOP、BLPOP（阻塞头或尾弹出【了解即可】）
LRANGE（范围查找）

## Hash
hashTable
通常场景为：数据库每行数据的记录（数据集记录）
特殊功能：
HSETNX（判断是否可以进KEY，如果有KEY存在就不保存）
HMSET（批量插入）
HKEYS（获取所有当前key中的所有字段）
HVALS（获取所有当前key中的所有字段值）
HGETALL（获取所有当前key中的所有字段和值）
HSCAN（迭代哈希键中的键值对）
## Set
集合、不能重复、无序
通常场景为：投票（去重不能重复投票）、找共同好友（利用集合的交差并集）
特殊功能：
SINTER（交集）
SUNION（并集）
SDIFF（差集）
SSCAN（迭代）
## ZSet
集合、不能重复（重复的数据加分值而已）、有序
通常场景为：排行榜
特殊功能：
ZSCORE （返回分值）
ZRANGE （返回范围数据，按照分值从小到大排序）
ZREVRANGE （返回范围数据，按照分值从大到小排序）
ZRANK（返回排名具体指，按照分值从小到大排序）
ZREVRANK（返回排名具体指，按照分值从大到小排序）
ZUNIONSTORE（并集）
ZINTERSTORE（交集）
ZSCAN（迭代）
# 失效策略
## 定期删除
1——每个设置过期时间的key放到独立的hash，默认每秒定时遍历这个hash而不是整个空间
1-1——从过期key字典中，随机找20个key。
1-2——删除20个key中过期的key，过期的key超过25%，则再随机找20个KEY，每次处理时间都不会超过25MS
## 惰性删除
1-1——客户端使用KEY时候，先检查如果过期就删除
1-2——惰性策略是对定时策略的补充，因为定时策略不会删除所有过期的key
## 内存满
FIFO——最先进入的数据被首先清理
LFU——一直以来最少使用的元素,清理hit值最小的（需要设置元素的hit值）
LRU——最近最少使用的（缓存的元素有一个时间戳）时间戳离当前时间最远将清理。
# 持久化
##	 RDB（默认开）快照
快照，通过fork子进程完成持久化）
设计时间范围内N个key变动，则持久化【会丢数据】
快照机制：
1)Redis开fork子进程。
2)子进程将快照写入到临时文件
3)临时文件替换老的文件(dump.rdb)
系统默认快照策略：key的数量值代表至少有N个改变
save 900 1
save 300 10
save 60 10000
口诀：(15分钟->1个、5分钟->10个、1分钟->1万个)
## AOF（默认关）日志
每执行一条指令就会记录，因此AOF文件会变大，则遇到阈值会压缩AOF文件【不会丢数据，但是存储空间大】
三个同步策略（就是什么时候持久化）：
* 每秒同步【默认】
* 每修改同步
* 不同步
文件过大策略（rewrite机制，就是压缩文件）
写入日志的时候宕机问题（redis重启，然后使用redis-check-aof修复即可）

appendfsync always    #每次有数据修改发生时都会写入AOF文件。
appendfsync everysec #每秒钟同步一次，该策略为AOF的缺省策略。
appendfsync no         #从不同步。高效但是数据不会被持久化。
> RDB和AOF都开只会按照AOF恢复
# Redis与数据库一致性设计模式有四种
## Cache aside
【读缓存，写数据库】【读不一致】【应该程序要与缓存和数据库打交道】【无需任何设计】
1（读取）——应用程序从redis取数据,没有得到,则从数据库中取数据,成功后，放到缓存中。【引发穿透、击穿】
2（使用）——应用程序从redis取数据，取到后返回应用
3（更新）——先把数据存到数据库中，成功后，再让缓存失效。

## Read through
【需要维护数据库和redis】【读不一致】【应该程序只需与缓存打交道】【需要设计redis与mysql数据一致性服务程序（读数据服务）】
流程与Cache aside一样，仅仅是读数据需要隔开设置一个redis读数据库数据服务

## Write through
【读写都是缓存】【数据强一致】【应该程序只需与缓存打交道】【需要设计redis与mysql数据一致性服务程序（实时）】【银行系统】
程序只和缓存交互且只能通过缓存写数据。
每次向缓存中写数据，缓存把数据持久到数据库，且这两个操作在同一个事务。只有两次都写成功了才算成功（为了保证数据一致性）

## Write behind caching
【读写都是缓存】【数据不一致】【需要设计redis与mysql数据一致性服务程序（非实时）】
Write behind caching的写数据库不是实时的。【缓存写和数据库写是异步的】

# redis使用lua的目的
Lua要么都执行要么都不执行，因此是为了原子性（有点类似事务）

# Redis（分布式技术栈）

主线程(使用Epoll同步io模型 非阻塞)持续监听客户端

# SDS

使用 len 数据长度判断字符串长度

内存预分配 弹性扩容 free(剩余空间)   (len+addlen)*2 超过1024 每次扩容1024

## string类型最大存储512m

string和hash区别，string是一个整体

## 启动关闭连接

启动 redis-server【配置文件】 &

关闭 shutdown

连接 redis-cli -h ip地址 -p 端口

## redis锁

```
if (!(redisTemplate.opsForValue().setIfAbsent("lockKsy", "", Duration.ofSeconds(60)))) {
    while (true) {
        if (redisTemplate.opsForValue().setIfAbsent("lockKsy", "", Duration.ofSeconds(60))) {
            break;
        }
    }
}
业务代码
redisTemplate.delete("lockKsy");
```

## 查看类型

object encoding key

测试

redis-benchmark

查看详细

info

## 查看长度

strlen key

## 切换库(select 下标)操作

查看当前数据库实例中所有key的数量：dbsize
查看当前数据库实例中所有的key：keys *
清空数据库实例：flushdb
清空所有的数据库实例：flushall
查看redis中所有的配置信息：config get *
查看redis中的指定的配置信息：config get parameter

## key操作

### 匹配

keys * 0~n

keys ? 1

ksys []  里面任意一个

### 判断是否存在

exists key

### 移动

move key 库（0-15）

### 设置生存时间

expire key 时间（秒）

### 查看生存时间

ttl key

### 查看类型

type key

## 重命名

rename key 新名

## 删除（返回删除数量）

del key

## 数据操作

### 字符串

设置 set 数据 值

获取 get key

追加 append 数据 值

加1运算 incr

减一运算 decr

加运算 incrby 数据 加什么

减运算 decrby 数据 减什么

闭区间获取字符串 getrange key 开始下标 结束下标

下标自左至右，从0开始，依次往后，最后一个字符的下标是字符串长多-1；
字符串中每一个下标也可以是负数，负下标表示自右至左，从-1开始，依次往前，最右边一个字符的下标是-1

用value覆盖从下标为startIndex开始的字符串，能覆盖几个字符就覆盖几个字符：setrange key 开始下标 值

设置字符串数据的同时，设置它最大生命周期 setex key 时间  值

当key不存在时设置成功，否则，则放弃设置：setnx key value

批量设置 mset 键1 值1 键2 值2 ....

批量获取 mget 键1 键2 键3...

所有key都不存在时设置成功，否则(只要有一个已经存在！！！)，则全部放弃设置 msetnx 键1 值1 键2 值2 .....

### list

将一个或者多个值依次插入到列表的表头(左侧)：lpush key value [value value .....]
lpush list01 1 2 3  结果：3 2 1
lpush list01 4 5     结果：5 4 3 2 1

获取指定列表中指定下标区间的元素：lrange key startIndex endIndex
lrange list01 1 3  结果：4 3 2
lrange list01 1 -2 结果: 4 3 2
lrange list01 0 -1 结果：5 4 3 2 1

将一个或者多个值依次插入到列表的表尾(右侧)：rpush key value [value value .....]
rpush list02 a b c 结果：a b c
rpush list02 d e   结果：a b c d e
lpush list02 m n   结果: n m a b c d e

从指定列表中移除并且返回表头元素：lpop key
lpop list02

从指定列表中移除并且返回表尾元素：rpop key
rpop list02

获取指定列表中指定下标的元素：lindex key index
lindex list01 2 结果：3

获取指定列表的长度：llen key
llen list01

根据count值移除指定列表中跟value相等的数据：lrem key count value
count>0：从列表的左侧移除count个跟value相等的数据；
count<0：从列表的右侧移除count个跟vlaue相等的数据；
count=0：从列表中移除所有跟value相等的数据

截取指定列表中指定下标区间的元素组成新的列表，并且赋值给key：ltrim key startIndex endIndex
lpush list04 1 2 3 4 5  结果：5 4 3 2 1
ltrim list04 1 3
lrange list04 0 -1      结果：4 3 2

将指定列表中指定下标的元素设置为指定值 lset key 下标 新值

将value插入到指定列表中位于pivot元素之前/之后的位置： linsert key before/after pivot vlaue
linsert list04 before 10 50
linsert list04 after 10 60

### set

redis中有关set类型数据的操作命令：单key-多无序value
一个key对应多个vlaue；
value之间没有顺序，并且不能重复；
通过业务数据直接操作集合。
a)将一个或者多个元素添加到指定的集合中：sadd key value [value value ....]
*如果元素已经存在，则会忽略。
*返回成功加入的元素的个数
sadd set01 a b c a  结果：a b c
sadd set01 b d e
b)获取指定集合中所有的元素：smembers key
smembers set01
c)判断指定元素在指定集合中是否存在:sismember key member
*存在，返回1
*不存在，返回0
sismember set01 f
sismember set01 a
d)获取指定集合的长度：scard key
scard set01
e)移除指定集合中一个或者多个元素：srem key member [member .....]
*不存在的元素会被忽略
*返回成功成功移除的个数
srem set01 b d m
f)随机获取指定集合中的一个或者多个元素：srandmember key [count]
|->count>0：随机获取的多个元素之间不能重复
|->count<0: 随机获取的多个元素之间可能重复
sadd set02 1 2 3 4 5 6 7 8
srandmember set02
srandmember set02 3
srandmember set02 -3
g)从指定集合中随机移除一个或者多个元素：spop key [count]
spop set02
h)将指定集合中的指定元素移动到另一个元素:smove source dest member
smove set01 set02 a
i)获取第一个集合中有、但是其它集合中都没有的元素组成的新集合：sdiff key key [key key ....]
sdiff set01 set02 set03
j)获取所有指定集合中都有的元素组成的新集合：sinter key key [key key ....]
sinter set01 set02 set03
k)获取所有指定集合中所有元素组成的大集合：sunion key key [key key .....]
sunion set01 set02 set03

### hash

单key:field-value
field-value
.....
studentzs:id-1001
name-zhangsan
age-20
a)将一个或者多个field-vlaue对设置到哈希表中：hset key filed1 value1 [field2 value2 ....]
*如果key field已经存在，把value会把以前的值覆盖掉
hset stu1001 id 1001
hset stu1001 name zhangsan age 20
b)获取指定哈希表中指定field的值：hget key field
hget stu1001 id
hget stu1001 name
c)批量将多个field-value对设置到哈希表中： hmset key filed1 value1 [field2 value2 ....]
hmset stu1002 id 1002 name lisi age 20
d)批量获取指定哈希表中的field的值：hmget key field1 [field2 field3 ....]
hmget stu1001 id name age
e)获取指定哈希表中所有的field和value：hgetall key
hgetall stu1002
f)从指定哈希表中删除一个或者多个field：hdel key field1 [field2 field3 ....]
hdel stu1002 name age
g)获取指定哈希表中所有的filed个数：hlen key
hlen stu1001
hlen stu1002
h)判断指定哈希表中是否存在某一个field：hexists key field
hexists stu1001 name
hexists stu1002 name
i)获取指定哈希表中所有的field列表：hkeys key
hkeys stu1001
hkeys stu1002
j)获取指定哈希表中所有的value列表：hvals key
hvals stu1001
hvals stu1002
k)对指定哈希表中指定field值进行整数加法运算：hincrby key field int
hincrby stu1001 age 5
l)对指定哈希表中指定field值进行浮点数加法运算：hincrbyfloat key field float
hset stu1001 score 80.5
hincrbyfloat stu1001 score 5.5
m)将一个field-vlaue对设置到哈希表中，当key-field已经存在时，则放弃设置；否则，设置file-value：hsetnx key field value
hsetnx stu1001 age 30

### zset

有序集合
本质上是集合，所有元素不能重复；
每一个元素都关联一个分数，redis会根据分数对元素进行自动排序；
分数可以重复；
既然有序集合中每一个元素都有顺序，那么也都有下标；
有序集合中元素的排序规则又列表中元素的排序规则不一样。
a)将一个或者多个member及其score值加入有序集合：zadd key score member [score member ....]
*如果元素已经存在，则把分数覆盖
zadd zset01 20 z1 30 z2 50 z3 40 z4
zadd zset01 60 z2
b)获取指定有序集合中指定下标区间的元素：zrange key startIndex endIndex [withscores]
zrange zset01 0 -1
zrange zset01 0 -1 withscores
c)获取指定有序集合中指定分数区间(闭区间)的元素：zrangebyscore key min max [withscores]
zrangebyscore zset01 30 50 withscores
d)删除指定有序集合中一个或者多个元素：zrem key member [member......]
zrem zset01 z3 z4
e)获取指定有序集合中所有元素的个数：zcard key
zcard zset01
f)获取指定有序集合中分数在指定区间内的元素的个数：zcount key min max
zcount zset01 20 50
g)获取指定有序集合中指定元素的排名(排名从0开始)： zrank key member
zrank zset01 z4  ===>2
h)获取指定有序集合中指定元素的分数：zscore key member
zscore zset01 z4
i)获取指定有序集合中指定元素的排名(按照分数从大到小的排名):zrevrank key member
zrevrank zset01 z4  ===>1

## 事务

watch保证一致性

watch 某个key：执行过程中监控某个key的值发生变化，结束放弃执行

unwatch 放弃监控key

开启：multi

执行：exec

取消：discard

## 集群

info Replication

设从不设主

slaveof 127.0.0.1 6379

让从机上位

断开关系：slaveof no one

### 消息订阅（简单了解）

subscribe 订阅消息

publis 发布消息

psubcribe 支持通配符



### 工具类
[文章](https://blog.csdn.net/mengxiangxingdong/article/details/88419976)
```java
package com.wf.ew.core.utils;

import org.springframework.data.redis.connection.DataType;
import org.springframework.data.redis.core.Cursor;
import org.springframework.data.redis.core.ScanOptions;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.core.ZSetOperations.TypedTuple;

import java.util.Collection;
import java.util.Date;
import java.util.List;
import java.util.Map;
import java.util.Map.Entry;
import java.util.Set;
import java.util.concurrent.TimeUnit;

/**
 * Redis工具类
 * 
 * @author WangFan
 * @date 2018-02-24 下午03:09:50
 * @version 1.1 (GitHub文档: https://github.com/whvcse/RedisUtil )
 */
public class RedisUtil {
	private StringRedisTemplate redisTemplate;

	public void setRedisTemplate(StringRedisTemplate redisTemplate) {
		this.redisTemplate = redisTemplate;
	}

	public StringRedisTemplate getRedisTemplate() {
		return this.redisTemplate;
	}

	/** -------------------key相关操作--------------------- */

	/**
	 * 删除key
	 * 
	 * @param key
	 */
	public void delete(String key) {
		redisTemplate.delete(key);
	}

	/**
	 * 批量删除key
	 * 
	 * @param keys
	 */
	public void delete(Collection<String> keys) {
		redisTemplate.delete(keys);
	}

	/**
	 * 序列化key
	 * 
	 * @param key
	 * @return
	 */
	public byte[] dump(String key) {
		return redisTemplate.dump(key);
	}

	/**
	 * 是否存在key
	 * 
	 * @param key
	 * @return
	 */
	public Boolean hasKey(String key) {
		return redisTemplate.hasKey(key);
	}

	/**
	 * 设置过期时间
	 * 
	 * @param key
	 * @param timeout
	 * @param unit
	 * @return
	 */
	public Boolean expire(String key, long timeout, TimeUnit unit) {
		return redisTemplate.expire(key, timeout, unit);
	}

	/**
	 * 设置过期时间
	 * 
	 * @param key
	 * @param date
	 * @return
	 */
	public Boolean expireAt(String key, Date date) {
		return redisTemplate.expireAt(key, date);
	}

	/**
	 * 查找匹配的key
	 * 
	 * @param pattern
	 * @return
	 */
	public Set<String> keys(String pattern) {
		return redisTemplate.keys(pattern);
	}

	/**
	 * 将当前数据库的 key 移动到给定的数据库 db 当中
	 * 
	 * @param key
	 * @param dbIndex
	 * @return
	 */
	public Boolean move(String key, int dbIndex) {
		return redisTemplate.move(key, dbIndex);
	}

	/**
	 * 移除 key 的过期时间，key 将持久保持
	 * 
	 * @param key
	 * @return
	 */
	public Boolean persist(String key) {
		return redisTemplate.persist(key);
	}

	/**
	 * 返回 key 的剩余的过期时间
	 * 
	 * @param key
	 * @param unit
	 * @return
	 */
	public Long getExpire(String key, TimeUnit unit) {
		return redisTemplate.getExpire(key, unit);
	}

	/**
	 * 返回 key 的剩余的过期时间
	 * 
	 * @param key
	 * @return
	 */
	public Long getExpire(String key) {
		return redisTemplate.getExpire(key);
	}

	/**
	 * 从当前数据库中随机返回一个 key
	 * 
	 * @return
	 */
	public String randomKey() {
		return redisTemplate.randomKey();
	}

	/**
	 * 修改 key 的名称
	 * 
	 * @param oldKey
	 * @param newKey
	 */
	public void rename(String oldKey, String newKey) {
		redisTemplate.rename(oldKey, newKey);
	}

	/**
	 * 仅当 newkey 不存在时，将 oldKey 改名为 newkey
	 * 
	 * @param oldKey
	 * @param newKey
	 * @return
	 */
	public Boolean renameIfAbsent(String oldKey, String newKey) {
		return redisTemplate.renameIfAbsent(oldKey, newKey);
	}

	/**
	 * 返回 key 所储存的值的类型
	 * 
	 * @param key
	 * @return
	 */
	public DataType type(String key) {
		return redisTemplate.type(key);
	}

	/** -------------------string相关操作--------------------- */

	/**
	 * 设置指定 key 的值
	 * @param key
	 * @param value
	 */
	public void set(String key, String value) {
		redisTemplate.opsForValue().set(key, value);
	}

	/**
	 * 获取指定 key 的值
	 * @param key
	 * @return
	 */
	public String get(String key) {
		return redisTemplate.opsForValue().get(key);
	}

	/**
	 * 返回 key 中字符串值的子字符
	 * @param key
	 * @param start
	 * @param end
	 * @return
	 */
	public String getRange(String key, long start, long end) {
		return redisTemplate.opsForValue().get(key, start, end);
	}

	/**
	 * 将给定 key 的值设为 value ，并返回 key 的旧值(old value)
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public String getAndSet(String key, String value) {
		return redisTemplate.opsForValue().getAndSet(key, value);
	}

	/**
	 * 对 key 所储存的字符串值，获取指定偏移量上的位(bit)
	 * 
	 * @param key
	 * @param offset
	 * @return
	 */
	public Boolean getBit(String key, long offset) {
		return redisTemplate.opsForValue().getBit(key, offset);
	}

	/**
	 * 批量获取
	 * 
	 * @param keys
	 * @return
	 */
	public List<String> multiGet(Collection<String> keys) {
		return redisTemplate.opsForValue().multiGet(keys);
	}

	/**
	 * 设置ASCII码, 字符串'a'的ASCII码是97, 转为二进制是'01100001', 此方法是将二进制第offset位值变为value
	 * 
	 * @param key
	 * @param postion
	 *            位置
	 * @param value
	 *            值,true为1, false为0
	 * @return
	 */
	public boolean setBit(String key, long offset, boolean value) {
		return redisTemplate.opsForValue().setBit(key, offset, value);
	}

	/**
	 * 将值 value 关联到 key ，并将 key 的过期时间设为 timeout
	 * 
	 * @param key
	 * @param value
	 * @param timeout
	 *            过期时间
	 * @param unit
	 *            时间单位, 天:TimeUnit.DAYS 小时:TimeUnit.HOURS 分钟:TimeUnit.MINUTES
	 *            秒:TimeUnit.SECONDS 毫秒:TimeUnit.MILLISECONDS
	 */
	public void setEx(String key, String value, long timeout, TimeUnit unit) {
		redisTemplate.opsForValue().set(key, value, timeout, unit);
	}

	/**
	 * 只有在 key 不存在时设置 key 的值
	 * 
	 * @param key
	 * @param value
	 * @return 之前已经存在返回false,不存在返回true
	 */
	public boolean setIfAbsent(String key, String value) {
		return redisTemplate.opsForValue().setIfAbsent(key, value);
	}

	/**
	 * 用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始
	 * 
	 * @param key
	 * @param value
	 * @param offset
	 *            从指定位置开始覆写
	 */
	public void setRange(String key, String value, long offset) {
		redisTemplate.opsForValue().set(key, value, offset);
	}

	/**
	 * 获取字符串的长度
	 * 
	 * @param key
	 * @return
	 */
	public Long size(String key) {
		return redisTemplate.opsForValue().size(key);
	}

	/**
	 * 批量添加
	 * 
	 * @param maps
	 */
	public void multiSet(Map<String, String> maps) {
		redisTemplate.opsForValue().multiSet(maps);
	}

	/**
	 * 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在
	 * 
	 * @param maps
	 * @return 之前已经存在返回false,不存在返回true
	 */
	public boolean multiSetIfAbsent(Map<String, String> maps) {
		return redisTemplate.opsForValue().multiSetIfAbsent(maps);
	}

	/**
	 * 增加(自增长), 负数则为自减
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long incrBy(String key, long increment) {
		return redisTemplate.opsForValue().increment(key, increment);
	}

	/**
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Double incrByFloat(String key, double increment) {
		return redisTemplate.opsForValue().increment(key, increment);
	}

	/**
	 * 追加到末尾
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Integer append(String key, String value) {
		return redisTemplate.opsForValue().append(key, value);
	}

	/** -------------------hash相关操作------------------------- */

	/**
	 * 获取存储在哈希表中指定字段的值
	 * 
	 * @param key
	 * @param field
	 * @return
	 */
	public Object hGet(String key, String field) {
		return redisTemplate.opsForHash().get(key, field);
	}

	/**
	 * 获取所有给定字段的值
	 * 
	 * @param key
	 * @return
	 */
	public Map<Object, Object> hGetAll(String key) {
		return redisTemplate.opsForHash().entries(key);
	}

	/**
	 * 获取所有给定字段的值
	 * 
	 * @param key
	 * @param fields
	 * @return
	 */
	public List<Object> hMultiGet(String key, Collection<Object> fields) {
		return redisTemplate.opsForHash().multiGet(key, fields);
	}

	public void hPut(String key, String hashKey, String value) {
		redisTemplate.opsForHash().put(key, hashKey, value);
	}

	public void hPutAll(String key, Map<String, String> maps) {
		redisTemplate.opsForHash().putAll(key, maps);
	}

	/**
	 * 仅当hashKey不存在时才设置
	 * 
	 * @param key
	 * @param hashKey
	 * @param value
	 * @return
	 */
	public Boolean hPutIfAbsent(String key, String hashKey, String value) {
		return redisTemplate.opsForHash().putIfAbsent(key, hashKey, value);
	}

	/**
	 * 删除一个或多个哈希表字段
	 * 
	 * @param key
	 * @param fields
	 * @return
	 */
	public Long hDelete(String key, Object... fields) {
		return redisTemplate.opsForHash().delete(key, fields);
	}

	/**
	 * 查看哈希表 key 中，指定的字段是否存在
	 * 
	 * @param key
	 * @param field
	 * @return
	 */
	public boolean hExists(String key, String field) {
		return redisTemplate.opsForHash().hasKey(key, field);
	}

	/**
	 * 为哈希表 key 中的指定字段的整数值加上增量 increment
	 * 
	 * @param key
	 * @param field
	 * @param increment
	 * @return
	 */
	public Long hIncrBy(String key, Object field, long increment) {
		return redisTemplate.opsForHash().increment(key, field, increment);
	}

	/**
	 * 为哈希表 key 中的指定字段的整数值加上增量 increment
	 * 
	 * @param key
	 * @param field
	 * @param delta
	 * @return
	 */
	public Double hIncrByFloat(String key, Object field, double delta) {
		return redisTemplate.opsForHash().increment(key, field, delta);
	}

	/**
	 * 获取所有哈希表中的字段
	 * 
	 * @param key
	 * @return
	 */
	public Set<Object> hKeys(String key) {
		return redisTemplate.opsForHash().keys(key);
	}

	/**
	 * 获取哈希表中字段的数量
	 * 
	 * @param key
	 * @return
	 */
	public Long hSize(String key) {
		return redisTemplate.opsForHash().size(key);
	}

	/**
	 * 获取哈希表中所有值
	 * 
	 * @param key
	 * @return
	 */
	public List<Object> hValues(String key) {
		return redisTemplate.opsForHash().values(key);
	}

	/**
	 * 迭代哈希表中的键值对
	 * 
	 * @param key
	 * @param options
	 * @return
	 */
	public Cursor<Entry<Object, Object>> hScan(String key, ScanOptions options) {
		return redisTemplate.opsForHash().scan(key, options);
	}

	/** ------------------------list相关操作---------------------------- */

	/**
	 * 通过索引获取列表中的元素
	 * 
	 * @param key
	 * @param index
	 * @return
	 */
	public String lIndex(String key, long index) {
		return redisTemplate.opsForList().index(key, index);
	}

	/**
	 * 获取列表指定范围内的元素
	 * 
	 * @param key
	 * @param start
	 *            开始位置, 0是开始位置
	 * @param end
	 *            结束位置, -1返回所有
	 * @return
	 */
	public List<String> lRange(String key, long start, long end) {
		return redisTemplate.opsForList().range(key, start, end);
	}

	/**
	 * 存储在list头部
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long lLeftPush(String key, String value) {
		return redisTemplate.opsForList().leftPush(key, value);
	}

	/**
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long lLeftPushAll(String key, String... value) {
		return redisTemplate.opsForList().leftPushAll(key, value);
	}

	/**
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long lLeftPushAll(String key, Collection<String> value) {
		return redisTemplate.opsForList().leftPushAll(key, value);
	}

	/**
	 * 当list存在的时候才加入
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long lLeftPushIfPresent(String key, String value) {
		return redisTemplate.opsForList().leftPushIfPresent(key, value);
	}

	/**
	 * 如果pivot存在,再pivot前面添加
	 * 
	 * @param key
	 * @param pivot
	 * @param value
	 * @return
	 */
	public Long lLeftPush(String key, String pivot, String value) {
		return redisTemplate.opsForList().leftPush(key, pivot, value);
	}

	/**
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long lRightPush(String key, String value) {
		return redisTemplate.opsForList().rightPush(key, value);
	}

	/**
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long lRightPushAll(String key, String... value) {
		return redisTemplate.opsForList().rightPushAll(key, value);
	}

	/**
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long lRightPushAll(String key, Collection<String> value) {
		return redisTemplate.opsForList().rightPushAll(key, value);
	}

	/**
	 * 为已存在的列表添加值
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long lRightPushIfPresent(String key, String value) {
		return redisTemplate.opsForList().rightPushIfPresent(key, value);
	}

	/**
	 * 在pivot元素的右边添加值
	 * 
	 * @param key
	 * @param pivot
	 * @param value
	 * @return
	 */
	public Long lRightPush(String key, String pivot, String value) {
		return redisTemplate.opsForList().rightPush(key, pivot, value);
	}

	/**
	 * 通过索引设置列表元素的值
	 * 
	 * @param key
	 * @param index
	 *            位置
	 * @param value
	 */
	public void lSet(String key, long index, String value) {
		redisTemplate.opsForList().set(key, index, value);
	}

	/**
	 * 移出并获取列表的第一个元素
	 * 
	 * @param key
	 * @return 删除的元素
	 */
	public String lLeftPop(String key) {
		return redisTemplate.opsForList().leftPop(key);
	}

	/**
	 * 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止
	 * 
	 * @param key
	 * @param timeout
	 *            等待时间
	 * @param unit
	 *            时间单位
	 * @return
	 */
	public String lBLeftPop(String key, long timeout, TimeUnit unit) {
		return redisTemplate.opsForList().leftPop(key, timeout, unit);
	}

	/**
	 * 移除并获取列表最后一个元素
	 * 
	 * @param key
	 * @return 删除的元素
	 */
	public String lRightPop(String key) {
		return redisTemplate.opsForList().rightPop(key);
	}

	/**
	 * 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止
	 * 
	 * @param key
	 * @param timeout
	 *            等待时间
	 * @param unit
	 *            时间单位
	 * @return
	 */
	public String lBRightPop(String key, long timeout, TimeUnit unit) {
		return redisTemplate.opsForList().rightPop(key, timeout, unit);
	}

	/**
	 * 移除列表的最后一个元素，并将该元素添加到另一个列表并返回
	 * 
	 * @param sourceKey
	 * @param destinationKey
	 * @return
	 */
	public String lRightPopAndLeftPush(String sourceKey, String destinationKey) {
		return redisTemplate.opsForList().rightPopAndLeftPush(sourceKey,
				destinationKey);
	}

	/**
	 * 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止
	 * 
	 * @param sourceKey
	 * @param destinationKey
	 * @param timeout
	 * @param unit
	 * @return
	 */
	public String lBRightPopAndLeftPush(String sourceKey, String destinationKey,
			long timeout, TimeUnit unit) {
		return redisTemplate.opsForList().rightPopAndLeftPush(sourceKey,
				destinationKey, timeout, unit);
	}

	/**
	 * 删除集合中值等于value得元素
	 * 
	 * @param key
	 * @param index
	 *            index=0, 删除所有值等于value的元素; index>0, 从头部开始删除第一个值等于value的元素;
	 *            index<0, 从尾部开始删除第一个值等于value的元素;
	 * @param value
	 * @return
	 */
	public Long lRemove(String key, long index, String value) {
		return redisTemplate.opsForList().remove(key, index, value);
	}

	/**
	 * 裁剪list
	 * 
	 * @param key
	 * @param start
	 * @param end
	 */
	public void lTrim(String key, long start, long end) {
		redisTemplate.opsForList().trim(key, start, end);
	}

	/**
	 * 获取列表长度
	 * 
	 * @param key
	 * @return
	 */
	public Long lLen(String key) {
		return redisTemplate.opsForList().size(key);
	}

	/** --------------------set相关操作-------------------------- */

	/**
	 * set添加元素
	 * 
	 * @param key
	 * @param values
	 * @return
	 */
	public Long sAdd(String key, String... values) {
		return redisTemplate.opsForSet().add(key, values);
	}

	/**
	 * set移除元素
	 * 
	 * @param key
	 * @param values
	 * @return
	 */
	public Long sRemove(String key, Object... values) {
		return redisTemplate.opsForSet().remove(key, values);
	}

	/**
	 * 移除并返回集合的一个随机元素
	 * 
	 * @param key
	 * @return
	 */
	public String sPop(String key) {
		return redisTemplate.opsForSet().pop(key);
	}

	/**
	 * 将元素value从一个集合移到另一个集合
	 * 
	 * @param key
	 * @param value
	 * @param destKey
	 * @return
	 */
	public Boolean sMove(String key, String value, String destKey) {
		return redisTemplate.opsForSet().move(key, value, destKey);
	}

	/**
	 * 获取集合的大小
	 * 
	 * @param key
	 * @return
	 */
	public Long sSize(String key) {
		return redisTemplate.opsForSet().size(key);
	}

	/**
	 * 判断集合是否包含value
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Boolean sIsMember(String key, Object value) {
		return redisTemplate.opsForSet().isMember(key, value);
	}

	/**
	 * 获取两个集合的交集
	 * 
	 * @param key
	 * @param otherKey
	 * @return
	 */
	public Set<String> sIntersect(String key, String otherKey) {
		return redisTemplate.opsForSet().intersect(key, otherKey);
	}

	/**
	 * 获取key集合与多个集合的交集
	 * 
	 * @param key
	 * @param otherKeys
	 * @return
	 */
	public Set<String> sIntersect(String key, Collection<String> otherKeys) {
		return redisTemplate.opsForSet().intersect(key, otherKeys);
	}

	/**
	 * key集合与otherKey集合的交集存储到destKey集合中
	 * 
	 * @param key
	 * @param otherKey
	 * @param destKey
	 * @return
	 */
	public Long sIntersectAndStore(String key, String otherKey, String destKey) {
		return redisTemplate.opsForSet().intersectAndStore(key, otherKey,
				destKey);
	}

	/**
	 * key集合与多个集合的交集存储到destKey集合中
	 * 
	 * @param key
	 * @param otherKeys
	 * @param destKey
	 * @return
	 */
	public Long sIntersectAndStore(String key, Collection<String> otherKeys,
			String destKey) {
		return redisTemplate.opsForSet().intersectAndStore(key, otherKeys,
				destKey);
	}

	/**
	 * 获取两个集合的并集
	 * 
	 * @param key
	 * @param otherKeys
	 * @return
	 */
	public Set<String> sUnion(String key, String otherKeys) {
		return redisTemplate.opsForSet().union(key, otherKeys);
	}

	/**
	 * 获取key集合与多个集合的并集
	 * 
	 * @param key
	 * @param otherKeys
	 * @return
	 */
	public Set<String> sUnion(String key, Collection<String> otherKeys) {
		return redisTemplate.opsForSet().union(key, otherKeys);
	}

	/**
	 * key集合与otherKey集合的并集存储到destKey中
	 * 
	 * @param key
	 * @param otherKey
	 * @param destKey
	 * @return
	 */
	public Long sUnionAndStore(String key, String otherKey, String destKey) {
		return redisTemplate.opsForSet().unionAndStore(key, otherKey, destKey);
	}

	/**
	 * key集合与多个集合的并集存储到destKey中
	 * 
	 * @param key
	 * @param otherKeys
	 * @param destKey
	 * @return
	 */
	public Long sUnionAndStore(String key, Collection<String> otherKeys,
			String destKey) {
		return redisTemplate.opsForSet().unionAndStore(key, otherKeys, destKey);
	}

	/**
	 * 获取两个集合的差集
	 * 
	 * @param key
	 * @param otherKey
	 * @return
	 */
	public Set<String> sDifference(String key, String otherKey) {
		return redisTemplate.opsForSet().difference(key, otherKey);
	}

	/**
	 * 获取key集合与多个集合的差集
	 * 
	 * @param key
	 * @param otherKeys
	 * @return
	 */
	public Set<String> sDifference(String key, Collection<String> otherKeys) {
		return redisTemplate.opsForSet().difference(key, otherKeys);
	}

	/**
	 * key集合与otherKey集合的差集存储到destKey中
	 * 
	 * @param key
	 * @param otherKey
	 * @param destKey
	 * @return
	 */
	public Long sDifference(String key, String otherKey, String destKey) {
		return redisTemplate.opsForSet().differenceAndStore(key, otherKey,
				destKey);
	}

	/**
	 * key集合与多个集合的差集存储到destKey中
	 * 
	 * @param key
	 * @param otherKeys
	 * @param destKey
	 * @return
	 */
	public Long sDifference(String key, Collection<String> otherKeys,
			String destKey) {
		return redisTemplate.opsForSet().differenceAndStore(key, otherKeys,
				destKey);
	}

	/**
	 * 获取集合所有元素
	 * 
	 * @param key
	 * @param otherKeys
	 * @param destKey
	 * @return
	 */
	public Set<String> setMembers(String key) {
		return redisTemplate.opsForSet().members(key);
	}

	/**
	 * 随机获取集合中的一个元素
	 * 
	 * @param key
	 * @return
	 */
	public String sRandomMember(String key) {
		return redisTemplate.opsForSet().randomMember(key);
	}

	/**
	 * 随机获取集合中count个元素
	 * 
	 * @param key
	 * @param count
	 * @return
	 */
	public List<String> sRandomMembers(String key, long count) {
		return redisTemplate.opsForSet().randomMembers(key, count);
	}

	/**
	 * 随机获取集合中count个元素并且去除重复的
	 * 
	 * @param key
	 * @param count
	 * @return
	 */
	public Set<String> sDistinctRandomMembers(String key, long count) {
		return redisTemplate.opsForSet().distinctRandomMembers(key, count);
	}

	/**
	 * 
	 * @param key
	 * @param options
	 * @return
	 */
	public Cursor<String> sScan(String key, ScanOptions options) {
		return redisTemplate.opsForSet().scan(key, options);
	}

	/**------------------zSet相关操作--------------------------------*/
	
	/**
	 * 添加元素,有序集合是按照元素的score值由小到大排列
	 * 
	 * @param key
	 * @param value
	 * @param score
	 * @return
	 */
	public Boolean zAdd(String key, String value, double score) {
		return redisTemplate.opsForZSet().add(key, value, score);
	}

	/**
	 * 
	 * @param key
	 * @param values
	 * @return
	 */
	public Long zAdd(String key, Set<TypedTuple<String>> values) {
		return redisTemplate.opsForZSet().add(key, values);
	}

	/**
	 * 
	 * @param key
	 * @param values
	 * @return
	 */
	public Long zRemove(String key, Object... values) {
		return redisTemplate.opsForZSet().remove(key, values);
	}

	/**
	 * 增加元素的score值，并返回增加后的值
	 * 
	 * @param key
	 * @param value
	 * @param delta
	 * @return
	 */
	public Double zIncrementScore(String key, String value, double delta) {
		return redisTemplate.opsForZSet().incrementScore(key, value, delta);
	}

	/**
	 * 返回元素在集合的排名,有序集合是按照元素的score值由小到大排列
	 * 
	 * @param key
	 * @param value
	 * @return 0表示第一位
	 */
	public Long zRank(String key, Object value) {
		return redisTemplate.opsForZSet().rank(key, value);
	}

	/**
	 * 返回元素在集合的排名,按元素的score值由大到小排列
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Long zReverseRank(String key, Object value) {
		return redisTemplate.opsForZSet().reverseRank(key, value);
	}

	/**
	 * 获取集合的元素, 从小到大排序
	 * 
	 * @param key
	 * @param start
	 *            开始位置
	 * @param end
	 *            结束位置, -1查询所有
	 * @return
	 */
	public Set<String> zRange(String key, long start, long end) {
		return redisTemplate.opsForZSet().range(key, start, end);
	}

	/**
	 * 获取集合元素, 并且把score值也获取
	 * 
	 * @param key
	 * @param start
	 * @param end
	 * @return
	 */
	public Set<TypedTuple<String>> zRangeWithScores(String key, long start,
			long end) {
		return redisTemplate.opsForZSet().rangeWithScores(key, start, end);
	}

	/**
	 * 根据Score值查询集合元素
	 * 
	 * @param key
	 * @param min
	 *            最小值
	 * @param max
	 *            最大值
	 * @return
	 */
	public Set<String> zRangeByScore(String key, double min, double max) {
		return redisTemplate.opsForZSet().rangeByScore(key, min, max);
	}

	/**
	 * 根据Score值查询集合元素, 从小到大排序
	 * 
	 * @param key
	 * @param min
	 *            最小值
	 * @param max
	 *            最大值
	 * @return
	 */
	public Set<TypedTuple<String>> zRangeByScoreWithScores(String key,
			double min, double max) {
		return redisTemplate.opsForZSet().rangeByScoreWithScores(key, min, max);
	}

	/**
	 * 
	 * @param key
	 * @param min
	 * @param max
	 * @param start
	 * @param end
	 * @return
	 */
	public Set<TypedTuple<String>> zRangeByScoreWithScores(String key,
			double min, double max, long start, long end) {
		return redisTemplate.opsForZSet().rangeByScoreWithScores(key, min, max,
				start, end);
	}

	/**
	 * 获取集合的元素, 从大到小排序
	 * 
	 * @param key
	 * @param start
	 * @param end
	 * @return
	 */
	public Set<String> zReverseRange(String key, long start, long end) {
		return redisTemplate.opsForZSet().reverseRange(key, start, end);
	}

	/**
	 * 获取集合的元素, 从大到小排序, 并返回score值
	 * 
	 * @param key
	 * @param start
	 * @param end
	 * @return
	 */
	public Set<TypedTuple<String>> zReverseRangeWithScores(String key,
			long start, long end) {
		return redisTemplate.opsForZSet().reverseRangeWithScores(key, start,
				end);
	}

	/**
	 * 根据Score值查询集合元素, 从大到小排序
	 * 
	 * @param key
	 * @param min
	 * @param max
	 * @return
	 */
	public Set<String> zReverseRangeByScore(String key, double min,
			double max) {
		return redisTemplate.opsForZSet().reverseRangeByScore(key, min, max);
	}

	/**
	 * 根据Score值查询集合元素, 从大到小排序
	 * 
	 * @param key
	 * @param min
	 * @param max
	 * @return
	 */
	public Set<TypedTuple<String>> zReverseRangeByScoreWithScores(
			String key, double min, double max) {
		return redisTemplate.opsForZSet().reverseRangeByScoreWithScores(key,
				min, max);
	}

	/**
	 * 
	 * @param key
	 * @param min
	 * @param max
	 * @param start
	 * @param end
	 * @return
	 */
	public Set<String> zReverseRangeByScore(String key, double min,
			double max, long start, long end) {
		return redisTemplate.opsForZSet().reverseRangeByScore(key, min, max,
				start, end);
	}

	/**
	 * 根据score值获取集合元素数量
	 * 
	 * @param key
	 * @param min
	 * @param max
	 * @return
	 */
	public Long zCount(String key, double min, double max) {
		return redisTemplate.opsForZSet().count(key, min, max);
	}

	/**
	 * 获取集合大小
	 * 
	 * @param key
	 * @return
	 */
	public Long zSize(String key) {
		return redisTemplate.opsForZSet().size(key);
	}

	/**
	 * 获取集合大小
	 * 
	 * @param key
	 * @return
	 */
	public Long zZCard(String key) {
		return redisTemplate.opsForZSet().zCard(key);
	}

	/**
	 * 获取集合中value元素的score值
	 * 
	 * @param key
	 * @param value
	 * @return
	 */
	public Double zScore(String key, Object value) {
		return redisTemplate.opsForZSet().score(key, value);
	}

	/**
	 * 移除指定索引位置的成员
	 * 
	 * @param key
	 * @param start
	 * @param end
	 * @return
	 */
	public Long zRemoveRange(String key, long start, long end) {
		return redisTemplate.opsForZSet().removeRange(key, start, end);
	}

	/**
	 * 根据指定的score值的范围来移除成员
	 * 
	 * @param key
	 * @param min
	 * @param max
	 * @return
	 */
	public Long zRemoveRangeByScore(String key, double min, double max) {
		return redisTemplate.opsForZSet().removeRangeByScore(key, min, max);
	}

	/**
	 * 获取key和otherKey的并集并存储在destKey中
	 * 
	 * @param key
	 * @param otherKey
	 * @param destKey
	 * @return
	 */
	public Long zUnionAndStore(String key, String otherKey, String destKey) {
		return redisTemplate.opsForZSet().unionAndStore(key, otherKey, destKey);
	}

	/**
	 * 
	 * @param key
	 * @param otherKeys
	 * @param destKey
	 * @return
	 */
	public Long zUnionAndStore(String key, Collection<String> otherKeys,
			String destKey) {
		return redisTemplate.opsForZSet()
				.unionAndStore(key, otherKeys, destKey);
	}

	/**
	 * 交集
	 * 
	 * @param key
	 * @param otherKey
	 * @param destKey
	 * @return
	 */
	public Long zIntersectAndStore(String key, String otherKey,
			String destKey) {
		return redisTemplate.opsForZSet().intersectAndStore(key, otherKey,
				destKey);
	}

	/**
	 * 交集
	 * 
	 * @param key
	 * @param otherKeys
	 * @param destKey
	 * @return
	 */
	public Long zIntersectAndStore(String key, Collection<String> otherKeys,
			String destKey) {
		return redisTemplate.opsForZSet().intersectAndStore(key, otherKeys,
				destKey);
	}

	/**
	 * 
	 * @param key
	 * @param options
	 * @return
	 */
	public Cursor<TypedTuple<String>> zScan(String key, ScanOptions options) {
		return redisTemplate.opsForZSet().scan(key, options);
	}
}
```