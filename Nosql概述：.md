### Nosql概述：

##### 	NoSQL的发展：

```html
单机Mysql时代：APP --> DAL --> Mysql
```

大多数网站都是静态的，服务器压力不大。

这种情况下，网站的瓶颈：1.数据量太大，一个机器放不下 2.数据的索引（B+ Tree），一个机器放不下	3.访问量（读写混合），一个服务器受不了。

> 2.Memcached（缓存）+Mysql +垂直拆分（读写分离）App --> DAL --> Cache 1-->n  Mysql(i)

网站大多数数据访问为读，每次去查数据库十分麻烦。希望减轻数据库压力，就用到了缓存.

发展过程：优化数据结构和索引 --> 文件缓存（IO） --> Memcached

![image-20210125161323058](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210125161323058.png)

>3.分库分表 + 水平拆分（mysql集群）

技术和业务在发展的同时，对人的要求也越来越高！

==本质：读 + 写==

早些年MyISAM：表锁，十分影响效率！高并发下出现严重的锁问题

转战innodb：行锁

逐渐...使用分库分表来解决写的压力！Mysql在那个年代推出了表分区！并没有多少人用。

![image-20210125161530440](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210125161530440.png)

> 4.如今的时代

技术爆炸，mysql等关系型数据库不够用了，数据量大，变化快

Mysql有的用来存储一些较大的文件，博客，图片。数据表变大，效率变低！如果有一种数据库来专门处理这种数据，MySQL压力就会变小（如何处理这些问题）大数据的IO压力下，表几乎没法更大

> 5.目前一个基本的互联网项目

<img src="C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210125162903230.png" alt="image-20210125162903230" style="zoom:70%;" />

> 为什么要用NoSQL!

用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志爆发式增长。

这时候我们就需要用NoSQL数据库，可以很好的处理上述问题！

##### 为什么要用NoSQL：

> NoSQL

NoSQL = Not Only SQL

关系型数据库：表格，行，列

泛指非关系型数据库的，伴随web2.0互联网诞生！传统的关系型数据库很难对付！尤其是超大规模的高并发的社区！

很多的数据类型如用户的个人信息，社交网络等等，这些数据类型的存储不需要一个固定的格式。！不需要过多的操作就可以横向扩展的！Map<string,Object>使用键值对来控制

> NoSQL的特点

1.方便扩展（数据之间没有关系，很好扩展）

2.大数据高性能（Redis一秒可以写8w次，读11w次NOSQL的缓存记录级，是一种细粒度的缓存，性能会比较高！！）

3.数据类型是多样性的。（不需要事先设计数据库，随取随用）

4.传统的RDBSM和NOSQL

```
传统的RDNSM
-结构化组织
-SQL
-数据和关系都存在单独的表中
-操作，数据定义语言
-严格的一致性
-......
```

```python
Nosql
- 不仅仅是数据
- 没有固定的查询语言
- 键值对存储，列存储，文档存储，图形数据库（社交关系）
- 最终一致性
- CAP定理和BASE（异地多活）初级架构师！
-高性能、高可用、高可扩
- ......
```

真正在公司中的实践：NoSQL + RDBMS 一起使用！

##### 阿里巴巴架构演进分析

<img src="C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210127102232763.png" alt="image-20210127102232763" style="zoom:67%;" />

> ```bash
> 1.商品的基本信息
> 名称、价格、商家信息：关系型数据库解决！MySQL/Oracle（淘宝早年去IOE）
> 2.商品的评论、描述（文字多）
> 文档型数据库，MongoDB
> 3.图片
> 分布式文件系统 FastDFS
> --淘宝 TFS
> --Google GFS
> --Hadoop HDFS
> --阿里云 oss
> 4.商品的关键字（搜索）
> -搜索引擎 solr elastisearch
> -ISearch：
> 5.商品热门的波段信息
> --内存数据库
> --Redis Tair Memache
> 6.商品交易
> --三方应用
> ```

大型互联网公司应用问题：

- 数据类型太多
- 数据源繁多、经常重构
- 数据要改造，大面积改造

解决问题：

<img src="C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210127103619493.png" alt="image-20210127103619493" style="zoom:67%;" />

<img src="C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210127104012078.png" alt="image-20210127104012078" style="zoom:67%;" />

##### NoSQL的四大分类

**KV键值对**

- ​	新浪：Redis
- ​    美团：Redis + Tair
- ​    百度、阿里：Redis + memecache

**文档性数据库（bosh格式和json一样）**

- MongoDB
  - ​		基于分布式文件储存的数据库，C++编写。用来处理大量的文档
  - ​		是一个介于关系型数据库和非关系型数据库中间的产品！MongoDB是非关系型数据库中功能最丰富，最像关系型数据库

**列存储数据库**

- Hbase
- 分布式文件系统

**图关系数据库**

- 他不是存图片，放的是关系，比如：朋友圈社交网络，广告推荐！
- Neo4j  infoGrid

**四种数据库对比**

<img src="C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210127105041948.png" alt="image-20210127105041948" style="zoom:67%;" />





### Redis入门

##### 概述

> Redis是什么？

Redis（**R**emote **D**ictionary **S**erver )，即远程字典服务。

是一个开源的使用ANSI [C语言](https://baike.baidu.com/item/C语言)编写、支持网络、可基于内存亦可持久化的日志型、Key-Value[数据库](https://baike.baidu.com/item/数据库/103728)，并提供多种语言的API

> redis能干嘛？

1、内存存储。持久化，内存中是断电即失、所以持久化很重要

2、效率高、可以用于高速缓存

3、发布订阅系统

4、地图信息分析

5、计时器，定时器（浏览量！）

6、Others

> Redis基于linux

##### 性能测试

redis-benchmark是一个压力测试工具 

<img src="C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210127172619844.png" alt="image-20210127172619844" style="zoom:67%;" />

**例子**：redis-benchmark -h 127.0.0.1 -p 6379 -t set,lpush -n 10000 -q

以上实例中主机为 127.0.0.1，端口号为 6379，执行的命令为 set,lpush，请求数为 10000，通过 -q 参数让结果只显示每秒执行的请求数。

**如何查看这些性能测试？**

![image-20210127173003666](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210127173003666.png)



##### 基础知识

redis默认有16个数据库，默认是第0个

可以用select进行切换

```bash
127.0.0.1:6379> select 3		#选择数据库
OK
127.0.0.1:6379[3]> select 0	
OK	
127.0.0.1:6379> DBSIZE		#查看数据库大小
(integer) 0
127.0.0.1:6379> set name test	#添加
OK
127.0.0.1:6379> DBSIZE		#查看数据库大小
(integer) 1
```



```bash
127.0.0.1:6379> keys *			#查看所有可以
1) "name"
127.0.0.1:6379> get name		#查看key相对应的value
"test"
```

```bash
127.0.0.1:6379> flushdb				#清空当前数据库
OK
127.0.0.1:6379> keys *			#查看当前keys【select 0】
(empty array)
127.0.0.1:6379> flushall				#清空所有数据库
```



> Redis单线程！

官方表示，redis是基于内存操作的，cpu不是Redis的瓶颈，Redis的瓶颈是很具机器的内存和网络带宽，既然可以使用单线程来实现，那就使用单线程了~

Redis是C语言写的，完全不比同样使用key-value的Memecache差！！

> Redis为什么单线程更快？

1、误区1：高性能服务器一定是多线程的？

2、误区2：多线程（CPU上下文会切换！）一定比单线程效率高？

了解CPU>内存>硬盘（速度）

核心：redis是将全部数据放在内存中，所以使用单线程去操作效率就是最高的，多线程（CPU会切换消耗时间），对于内存系统，如果没有上下文切换效率就是最高的！多次读写都是在一个CPU上的，在内存情况下，这个就是最佳！

### Redis五大数据类型

> 官方文档

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作==数据库==、==缓存==和==消息中间件MQ==。 它支持多种类型的数据结构，如 [字符串（strings）](http://redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://redis.cn/topics/lru-cache.html)，[事务（transactions）](http://redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

> 以下命令很重要

##### Redis-Key

```bash
127.0.0.1:6379> keys *		#查看当前所有的key
1) "age"
2) "name"
127.0.0.1:6379> exists name		#判断是否存在某个key
(integer) 1					#存在
127.0.0.1:6379> exists name1		#判断是否存在某个key
(integer) 0					#不存在
127.0.0.1:6379> move name 1		#数据库之间移动  0-->1
(integer) 1
127.0.0.1:6379> select 1	#选择数据库
OK
127.0.0.1:6379[1]> keys *  #查看【bd_1】当前所有的key
1) "name"
```

```bash
127.0.0.1:6379> keys * 		#查看当前所有的key
1) "age"
2) "name"
127.0.0.1:6379> expire name 10 		#设置剩余时间
(integer) 1
127.0.0.1:6379> ttl name  #查看剩余秒数
(integer) 7
127.0.0.1:6379> ttl name
(integer) 5
127.0.0.1:6379> get name		#查看当前所有的key
1) nil			#name已经没了~
```

> 遇到不会的命名，在官网查看：

<img src="C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210127192515938.png" alt="image-20210127192515938" style="zoom:50%;" />

##### string（字符串）

string类似的使用场景：value处理是我们的字符串还可以是我们的数字！！！

- 计数器
- 统计多单位的数量 uid:91513551513:follow 0 incr
- 对象缓存存储
- 粉丝数...

```bash
127.0.0.1:6379> set key1 v1  #set key value
OK
127.0.0.1:6379> append key1 "hello"		# 在value后面增加hello
(integer) 7
127.0.0.1:6379> get key1
"v1hello"
127.0.0.1:6379> append key1 hi		# 在value后面增加hi
(integer) 9
127.0.0.1:6379> get key1		#查看key--value
"v1hellohi"
127.0.0.1:6379> append kry1 "hello"   #如果当前key不存在，就相当于set key
(integer) 5
```

```bash
127.0.0.1:6379> set view 0   #set key value
OK
127.0.0.1:6379> incr view	#自动增加1
(integer) 1
127.0.0.1:6379> get view   #查看增加后的结果
"1"
127.0.0.1:6379> decr view   #自动减1
(integer) 0
127.0.0.1:6379> get view	#查看减少后的结果
"0"
######增加/减少--步长#############################################
127.0.0.1:6379> incrby view 10			#自动增加10   view+=10
(integer) 10
127.0.0.1:6379> get view
"10"
127.0.0.1:6379> decrby view 10      #自动减10  view-=10
(integer) 0
127.0.0.1:6379> decrby view 10   #自动减10  view-=10
(integer) -10
127.0.0.1:6379> get view
"-10"                     #0+10-10-10=-10
```

**########字符串范围(getrange)###################**

```bash
127.0.0.1:6379> getrange key1 0 5   #getrange  key stat end  [0,5]
"hello,"
127.0.0.1:6379> getrange key1 0 -1   #-1 查看全部
"hello,wkd"
```

**#########字符串替换(setrange)###############**

```bash
127.0.0.1:6379> setrange key1 6 wwkkdd   #设置key1从index=6开始的值
(integer) 12
127.0.0.1:6379> get key1           #结果
"hello,wwkkdd"
127.0.0.1:6379> setrange key1 6 wkd  #设置key1从index=6开始的值
(integer) 12
127.0.0.1:6379> get key1    #结果，更改了从6开始的！！后面字符串不对等的range没有被修改/消除
"hello,wkdkdd"
```

**#######setex（当前值存在  set with expire  设置过期时间）###############**

**#######setnx（当前值不存在 set if not exist 不存在再设置（没有值的时候才能成功）  在分布式锁中会常常使用）###############**

```bash
127.0.0.1:6379> setex key3 5 hello   #set key3 --> hello  同时  五秒后失效（key3存在不存在都可以）
OK
127.0.0.1:6379> setnx view 8     #set view --> 8 ×  因为view已经存在，所以创建失败
(integer) 0			#设置失败
127.0.0.1:6379> setnx viewx 8    #set viewx --> 8  因为viewx不存在，所以可以创建
(integer) 1         #设置成功
```

**############mset（一次设置多个）########################**

**############mget（一次获取多个）########################**

```bash
127.0.0.1:6379> keys *      #查看所有键值对
(empty array)       #空空如也
127.0.0.1:6379> mset key1 value1 k2 v2			#一次性设置两组  key1 --> value1 k2 --> v2
OK				#设置成功
127.0.0.1:6379> keys *	     #查看所有键值对（上一步成功）
1) "k2"
2) "key1"
127.0.0.1:6379> mget k2 key1  #一次获取两个键值对  mget v1 v2
1) "v2"
2) "value1"
127.0.0.1:6379> msetnx k2 v2 k3 v3     #msetnx是一个原子操作，要么一起成功，要么一起失败
(integer) 0       #失败
```

**##########对象########################**

```bash
#key user:1 value {name:wkd,age:1}   (Json)
127.0.0.1:6379> set user:1 {name:wkd,age:1}
OK
127.0.0.1:6379> get user:1
"{name:wkd,age:1}"
#这里的key是一个巧妙的设计：user:{id}:{filed}
127.0.0.1:6379> mset user:1:name wkd user:1:age 3   #一次多个设置
OK
127.0.0.1:6379> mget user:1:name user:1:age  #一次多个获取
1) "wkd"
2) "3"
```

**############getset（先get然后set）#####################**

```bash
127.0.0.1:6379> getset key8 redis   #刚开始，并没有key8这个key，所以get->nil，然后进行了设置
(nil)
127.0.0.1:6379> getset key8 mogodb  #存在key8获取当前值，并设置当前值
"redis"
127.0.0.1:6379> get key8
"mogodb"
```



##### List

##### Set

##### Hash

##### Zset

### 三种特殊数据类型

##### geospatial

##### hyperloglog

##### bitmaps