### Nosql概述：

###### 	NoSQL的发展：

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



##### List（列表）

基本的数据类型，列表

在Redis，我们课以把list实现为队列、栈，串。

> 所有的List命令

```bash
##############向list头部添加值（lpush）########################
127.0.0.1:6379> lpush list one             #压入one -> list
(integer) 1		#成功 
127.0.0.1:6379> lpush list two			   #压入two -> list
(integer) 2     #成功
127.0.0.1:6379> lrange list 0 1				#查看所有list，相当于栈
1) "two"
2) "one"
127.0.0.1:6379> lrange list 0 0				#list[0] = two 
1) "two"
127.0.0.1:6379> lrange list 1 1			#list[1]=one
1) "one"
##############向list尾部添加值（rpush）########################
127.0.0.1:6379> rpush list 1			#向尾部添加一个  rpush key element
(integer) 3
127.0.0.1:6379> lrange list 0 -1		
1) "two"	
2) "one"
3) "1"
127.0.0.1:6379> rpush list 2 3 4			#同时添加多个   rpush key element1，element2，element3.....
(integer) 6
```

向左向右移除（==LPOP==  || ==RPOP==）

```bash
127.0.0.1:6379> lpop list        #移除最左边的元素
"two"
127.0.0.1:6379> lrange list 0 -1		#查看所有
1) "one"
2) "1"
3) "2"
4) "3"
5) "4"
127.0.0.1:6379> rpop list		#移除最右边元素
"4"
127.0.0.1:6379> lrange list 0 -1	#查看所有
1) "one"
2) "1"
3) "2"
4) "3"
```

移除list中的具体值 ==lrem==

```bash
127.0.0.1:6379> lrange list 0 -1         #查看当前数据库中的值
1) "one"
2) "1"
3) "2"
4) "3"
5) "3"
127.0.0.1:6379> lrem list 1 one     #lrem key count element   移除list中的一个one
(integer) 1
127.0.0.1:6379> lrange list 0 -1		#移除后的list
1) "1"
2) "2"
3) "3"
4) "3"
127.0.0.1:6379> lrem list 2 3		#移除list中的两个3
(integer) 2	
127.0.0.1:6379> lrange list 0 -1	#移除后的list
1) "1"
2) "2"
```

截取list中的==list[star,stop]==，关键字==ltrim==，移除剩余的

```bash
127.0.0.1:6379> lrange list 0 -1      #查看当前list
1) "h5"
2) "h4"
3) "h3"
4) "h2"
5) "h1"
6) "h0"
127.0.0.1:6379> ltrim list 0 2		#截取[0,2]，其他的被抛弃
OK
127.0.0.1:6379> lrange list 0 -1
1) "h5"
2) "h4"
3) "h3"
```

移除==当前数组===最右边的值，并添加到==另一个数组==的最左边（==rpoplpush==）

```bash
127.0.0.1:6379> lrange list 0 -1            #查看当前list
1) "h5" 
2) "h4"
3) "h3"
127.0.0.1:6379> rpoplpush list otherlist		#将移除list最右边的值，并将其添加到otherlist的最左边
"h3"
127.0.0.1:6379> lrange otherlist 0 -1  		#查看otherlist
1) "h3"
127.0.0.1:6379> lrange list 0 -1		#查看list
1) "h5"
2) "h4"
```

==更新操作==，改变指定下标元素值==lset==

```bash
127.0.0.1:6379> lrange list 0 -1
1) "h5"
2) "h4"
127.0.0.1:6379> lset list 0 h1    #lset key index newValue
OK
127.0.0.1:6379> lrange list 0 -1
1) "h1"
2) "h4"
127.0.0.1:6379> lset list 3 h1       #超出list范围报错
(error) ERR index out of range
```

==插入操作==，插入到指定元素前面/后面（==before/after==)，==linsert==

```bash
127.0.0.1:6379> linsert list before h1 h888      #在h1前面before插入  h888
(integer) 3
127.0.0.1:6379> lrange list 0 -1	#查看当前list
1) "h888"
2) "h1"
3) "h4"
127.0.0.1:6379> linsert list after h1 h888   #在h1后面after插入  h888
(integer) 4
127.0.0.1:6379> lrange list 0 -1		#查看当前list
1) "h888"
2) "h1"
3) "h888"
4) "h4"
```

查看index = num 上的值==lindex==

```bash
127.0.0.1:6379> lindex list 1   #lindex key index  查看下标为1的值
"1" 
```

查看list的length==llen==

```bash
127.0.0.1:6379> llen list  #Llist key 查看list的长度
(integer) 4
```

> 小结

- 他实际上是一个链表，before Node after，left，right都可以插入值
- 如果key不存在，创建新的链表
- 如果key存在，新增内容
- 如果移除了所有值，空链表，也代表不存在！
- 在两边插入或者改动值，效率最高！中间元素，相对来说效率会低一点-

##### Set（集合）

set中的值是不能被重复的！！！

向set中==添加==值，==sadd==

```bash
127.0.0.1:6379> sadd myset hello         #添加值
(integer) 1
127.0.0.1:6379> sadd myset wkd         #添加值
(integer) 1
127.0.0.1:6379> sadd myset gogogo         #添加值
(integer) 1
127.0.0.1:6379> smembers myset         #查看set全部元素
1) "hello"
2) "wkd"
3) "gogogo"
127.0.0.1:6379> sadd myset hello         #添加**已存在**值
(integer) 0						#错误，添加失败
```

查看set中是否==存在==某个值，==SISMEMBER key member==

```bash
127.0.0.1:6379> sismember myset hello          #查看已存在值
(integer) 1								#存在
127.0.0.1:6379> sismember myset helloo			#查看不存在之
(integer) 0							#不存在
```

获取当前==set的length==，scard key

```bash
127.0.0.1:6379> scard myset       #查看当前set的长度
(integer) 3			#3
127.0.0.1:6379> sadd myset world	#添加
(integer) 1
127.0.0.1:6379> scard myset			#查看添加后的长度
(integer) 4				#4
```

移除set中的指定元素  ==srem key element==

```bash
127.0.0.1:6379> smembers myset   #查看当前set中的值
1) "hello"
2) "world"
3) "wkd"
4) "gogogo"
127.0.0.1:6379> srem myset gogogo  #移除set中的gogogo元素
(integer) 1		#成功
127.0.0.1:6379> srem myset gogogo	#再移除
(integer) 0		#失败，因为已经没有了
127.0.0.1:6379> smembers myset    #查看移除后的set
1) "hello"
2) "world"
3) "wkd"
```

从set中==随机抽取==元素，SRANDMEMBER key [count]/可选

```bash
127.0.0.1:6379> SRANDMEMBER myset    #随机抽取一个元素
"wkd"
127.0.0.1:6379> SRANDMEMBER myset    #随机抽取一个元素
"hello"
127.0.0.1:6379> SRANDMEMBER myset    #随机抽取一个元素
"hello"
127.0.0.1:6379> SRANDMEMBER myset 2    #随机抽取两个个元素
1) "hello"
2) "wkd"
```

移除随机的elemeber ==spop key==

```bash
127.0.0.1:6379> spop myset
"hello"
127.0.0.1:6379> spop myset
"wkd"
127.0.0.1:6379> SMEMBERS myset
1) "world"
```

将一个指定元素==移动到另一个se==t中, ==smove source destination member==

```bash
127.0.0.1:6379> SMEMBERS myset
1) "hello"
2) "world"
127.0.0.1:6379> SMOVE myset otherset hello
(integer) 1
127.0.0.1:6379> keys *
1) "myset"
2) "otherset"
127.0.0.1:6379> SMEMBERS myset
1) "world"
127.0.0.1:6379> SMEMBERS otherset
1) "hello"
```

差集（==SDIFF==），并集（==SUNION==），交集（==SINTER==）

```bash
127.0.0.1:6379> SMEMBERS otherset
1) "hello"
127.0.0.1:6379> SMEMBERS myset
1) "hello"
2) "world"
127.0.0.1:6379> SDIFF myset otherset       #myset与otherset的差集
1) "world"
127.0.0.1:6379> SINTER myset otherset	   #myset与otherset的交集
1) "hello"
127.0.0.1:6379> SUNION myset otherset		#myset与otherset的并集
1) "hello"
2) "world"
```

##### Hash（哈希）

Map集合，key-map！这时候这个值是一个map集合！本质和string没有本质的区别，还是一个简单的key-value

给hash赋值  ==hset key field value==

```bash
127.0.0.1:6379> HSET myhash field value1		#设置myhash -- field -- value1
(integer) 1
127.0.0.1:6379> HSET myhash field value2		#设置myhash -- field -- value2，被覆盖掉
(integer) 0
127.0.0.1:6379> HGET myhash field
"value2"
127.0.0.1:6379> HSET myhash field value1    
(integer) 0
127.0.0.1:6379> HGET myhash field		#获取hash值
"value1"
127.0.0.1:6379> HGETALL myhash			#获取key中的全部hash值
1) "field"
2) "value1"
3) "field2"
4) "value3"
127.0.0.1:6379> HMSET myset field1 value1 field2 value2    #一次性设置多个
OK   #设置成功
127.0.0.1:6379> HMGET myset field1 field2		#一次性获取多个
1) "value1"
2) "value2"
```

==删除==某个field，==HDEL key field==

```bash
127.0.0.1:6379> HGETALL myhash  #查看删除前
1) "field1"
2) "value1"
3) "field2"
4) "value2"
127.0.0.1:6379> HDEL myhash field1   #删除myhash 中的 field1
(integer) 1
127.0.0.1:6379> HGETALL myhash    #查看删除后
1) "field2"
2) "value2"
```

获取hash的==字段数量==，==HLEN key==

```bash
127.0.0.1:6379> HLEN myhash        #获取当前length
(integer) 1
127.0.0.1:6379> hSET myhash field1 value1       #添加一个
(integer) 1
127.0.0.1:6379> HLEN myhash			#再次获取
(integer) 2
```

判断hash中的==某个field是否存在==   ，==HEXISIT key field==

```bash
127.0.0.1:6379> HEXISTS myhash field1
(integer) 1
127.0.0.1:6379> HEXISTS myhash field3
(integer) 0
```

只获得所有的field   ==HKEYS==

只获取所有的value，==HVALS==

```bash
127.0.0.1:6379> HKEYS myhash
1) "field2"
2) "field1"
127.0.0.1:6379> HVALS myhash
1) "value2"
2) "value1"
```

当value为int时候，==增加或者减少==，==HINCRBY key field increment==

```bash
127.0.0.1:6379> HSET myhash field3 5
(integer) 1
127.0.0.1:6379> HINCRBY myhash field3 2
(integer) 7
127.0.0.1:6379> HGET myhash field3
"7"
```

如果不存在则创建，存在则报错 ==hsetnx key field value==

```bash
127.0.0.1:6379> HSETNX myhash field3 88       #field3 存在
(integer) 0
127.0.0.1:6379> HSETNX myhash field4 88		#field4不存在
(integer) 1
127.0.0.1:6379> HGET myhash field4    #查看field4
"88"
```

>hash变更的数据user name age，尤其是是用户信息之类的，经常变动的信息！hash更适合于对象的存储，String更加适合字符串存储！

##### Zset（有序集合）（Sorted set）

> 在set的基础上，增加了一个值，set k1 v1 ---  zset k1 sort v1
>
> 官网命令地址：http://redis.cn/commands.html#sorted_set

```bash
##########增加值############
#ZADD key [NX|XX] [CH] [INCR] score member [score member ...]
127.0.0.1:6379> zadd zset 1 one              #添加新成员
(integer) 1
127.0.0.1:6379> zadd zset 2 two
(integer) 1
127.0.0.1:6379> zadd zset 5 five
(integer) 1
127.0.0.1:6379> zadd zset 4 four
(integer) 1
127.0.0.1:6379> ZRANGE zset 0 -1 withscores   #展示全部
1) "one"
2) "1"
3) "two"
4) "2"
5) "four"
6) "4"
7) "five"
8) "5"
```

**排序**（返回有序集合中指定分数score区间内的成员，分数由低到高）==ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]==

==ZREVRANGEBYSCORE==高到低排序

```bash
127.0.0.1:6379> ZRANGEBYSCORE zset 1 4     #展示score属于[1,4]elements
1) "one"
2) "two"
3) "four"
#以正无穷、负无穷为边界  +INF  -INF
127.0.0.1:6379> ZRANGEBYSCORE zset -inf +inf      #展示score属于[-oo,+oo]elements
1) "one"
2) "two"
3) "four"
4) "five"
```

移除某个元素   ==ZREM key member [member ...]==

```bash
127.0.0.1:6379> ZRANGEBYSCORE zset -inf +inf         #查看移除前的zset
1) "one"
2) "two"
3) "four"
4) "five"
127.0.0.1:6379> ZREM zset five             #移除value为five的element
(integer) 1
127.0.0.1:6379> ZCARD zset      #移除后len-1
(integer) 3
```

返回分数范围内的成员数量  ==ZCOUNT key min max==

```bash
127.0.0.1:6379> ZRANGEBYSCORE zset -inf +inf         #查看当前所有值
1) "one"
2) "two"
3) "four"
127.0.0.1:6379> ZCOUNT zset 1 3          #返回[1,3]中的值的数量
(integer) 2
127.0.0.1:6379> ZCOUNT zset (1 3		  #返回(1,3]中的值的数量
(integer) 1
```

```bash
#`ZLEXCOUNT` 命令用于计算有序集合中指定成员之间的成员数量。 ZLEXCOUNT key min max
127.0.0.1:6379> ZLEXCOUNT myzset [b [d   
(integer) 3
127.0.0.1:6379> ZLEXCOUNT myzset [b (d
(integer) 2
127.0.0.1:6379> ZADD myzset 1 c        #将value=c的score重置为1
(integer) 0
127.0.0.1:6379> ZLEXCOUNT myzset [b (d        #返回结果中不再包括C，因为c的score不再是0
(integer) 1
```

> zset案例思路
>
> set排序，存储版继承季报，工资表排序
>
> 普通消息 ，1；重要消息，2。带权重进行判断

### 三种特殊数据类型

##### geospatial（地理位置）

>- 有效的经度从-180度到180度。
>- 有效的纬度从-85.05112878度到85.05112878度。
>- ==底层zset==！！！！利用zset的命令来删除、查看所有

定位

<img src="C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210201190559594.png" alt="image-20210201190559594" style="zoom:87%;" />

==添加==一个或多个地理位置到sorted set，==GEOADD key longitude latitude member==。规则：两级无法直接添加

==获取经纬度==，==GEOPOS key member==

==获取两地之间的距离==，==GEODIST key member1 member2==

```bash
127.0.0.1:6379> GEOADD sicily 13.361389 38.115556 "Palermo"          #添加地点“Palermo”的经纬度
(integer) 1
127.0.0.1:6379> GEOADD sicily 15.087269 37.502669 "Catania"	        #添加地点“Catania”的经纬度
(integer) 1
127.0.0.1:6379> GEODIST sicily Palermo Catania            #计算两地之间的距离
"166274.1516"
127.0.0.1:6379> GEOpos sicily Palermo        #获取某地的经纬度
1) 1) "13.36138933897018433"
   2) "38.11555639549629859"
```

返回一个标准的==Gohash字符串==，==GEOHASH key member==

```bash
127.0.0.1:6379> GEOHASH sicily Catania
1) "sqdtr74hyu0"
```

查询==某个**经纬点**指定半径内所有的地理空间元素==的集合，

==GEORADIUS key longitude latitude radius(半径) m|km|ft|mi(单位) [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count]==

![image-20210201193821254](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210201193821254.png)

>- `WITHDIST`: 在返回位置元素的同时， 将位置元素与中心之间的距离也一并返回。 距离的单位和用户给定的范围单位保持一致。
>- `WITHCOORD`: 将位置元素的经度和维度也一并返回。
>- `WITHHASH`: 以 52 位有符号整数的形式， 返回位置元素经过原始==geohash== 编码的有序集合分值。 这个选项主要用于底层应用或者调试， 实际中的作用并不大。
>- `ASC`: 根据中心的位置， 按照从近到远的方式返回位置元素。
>- `DESC`: 根据中心的位置， 按照从远到近的方式返回位置元素。
>- **COUNT `<count>`** 选项去获取前 N 个匹配元素，在对一个非常大的区域进行搜索时， 即使只使用 `COUNT` 选项去获取少量元素， 命令的执行速度也可能会非常慢。 但是从另一方面来说， 使用 `COUNT` 选项去减少需要返回的元素数量， 对于减少带宽来说仍然是非常有用的。

```bash
127.0.0.1:6379> GEORADIUS sicily 15 37 200 km WITHDIST ASC          #由近到远返回将(15,37)方圆200km内的地点，并标明两地的距离
1) 1) "Catania"
   2) "56.4413"
2) 1) "Palermo"
   2) "190.4424"
127.0.0.1:6379> GEORADIUS sicily 15 37 200 km withcoord			#由远到近(默认)返回(15,37)方圆200km内的地点，并标明地点的经纬度
1) 1) "Palermo"
   2) 1) "13.36138933897018433"
      2) "38.11555639549629859"
2) 1) "Catania"
   2) 1) "15.08726745843887329"
      2) "37.50266842333162032" 
127.0.0.1:6379> GEORADIUS sicily 15 37 200 km WITHDIST count 3   #获取(15,37)方圆200km内的最多三个地点，并标明直线距离
1) 1) "Catania"
   2) "56.4413"
2) 1) "Palermo"
   2) "190.4424"
```

查询==某个**地点**指定半径内所有的地理空间元素==的集合，

==GEORADIUSBYMEMBER key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count]==

> GEORADIUSBYMEMBER 大体命名与上面的GEORADIUS相同

```bash
127.0.0.1:6379> GEORADIUSBYMEMBER sicily Agrigento 100 km       #查看“Agrigento”方圆100km的地点
1) "Agrigento"
2) "Palermo"
```

==查看所有==,-----> zset

```bash
127.0.0.1:6379> ZRANGE sicily 0 -1
1) "Agrigento"
2) "Palermo"
3) "Catania"
```



##### hyperloglog（基数）

> 什么是基数？

A{1,2,3,4,5,4}   B{1,2,3,4,5}

基数（不重复的元素） = 5，可以接受误差

> 简介

Redis2.8.9更新了hyperloglog数据结构！

Redis Hyperloglog 基数统计算法！

优点：占用内存是固定的，2^64不同的元素的基数，只需要12kb内存！如果从内存角度比较，首选！！

**网页的UV（一个人访问一个网站多次，但是还是算作一个人！）**

传统方式：set保存用户id，然后保存set中的元素数量

这个方式会消耗大量内存空间，我们是为了技术==计数，不是为了保存id。

Hyperloglog  ，0.81%错误率是可以接受的！若是不允许出错，就用set

> 基本操作命令！！！

==添加==元素，==PFADD key element [element ...]==

==查看基数==，==PFCOUNT key==

==合并==，PFMERGE key1 key2 key3

```bash
127.0.0.1:6379> PFADD mykey a b c d e r f g d  #添加元素
(integer) 1
127.0.0.1:6379> PFCOUNT mykey      #添加了9个，但是d有两个，基数是8
(integer) 8
127.0.0.1:6379> PFADD mykey2 a oo pp   #想mykey2中添加元素
(integer) 1
127.0.0.1:6379> PFMERGE mykey2 mykey	#将key，key2合并到key2中去
OK
127.0.0.1:6379> PFCOUNT mykey2     #统计key2的基数， a b c d e r f g oo pp
(integer) 10
```

##### bitmaps（位存储）

> 0 1 0 1 1 0 0

Bitmaps 位图，数据结构！都是操作二进制来进行记录，就只有0 1 两个状态……

> 测试

```bash
#SETBIT key offset value(0/1)
127.0.0.1:6379> SETBIT sign 1 0       #添加新纪录
(integer) 0
127.0.0.1:6379> SETBIT sign 0 1      #添加新纪录
(integer) 0
127.0.0.1:6379> SETBIT sign 2 1      #添加新纪录
(integer) 0
127.0.0.1:6379> SETBIT sign 3 0      #添加新纪录
(integer) 0
127.0.0.1:6379> GETBIT sign 1      #查看offset=1的value
(integer) 0
127.0.0.1:6379> GETBIT sign 0      #添加offset=0的value
(integer) 1
```

==统计操作==value=1，==BITCOUNT key==

```bash
127.0.0.1:6379> BITCOUNT sign
(integer) 3
```

### 事务

##### 事务执行

原子性：要么同时成功，要么同时失败！（关系型数据库）

> **Redis单条命令保证原子性，但是事务不保证原子性！！**
>
> Redis事务：一组命令的集合！一个食物中的所有命令会被序列化，在事务执行过程中，会按照顺序（队列）执行！
>
> 一次性、顺序性、排他性---执行一系列命令

==Redis没有隔离级别的概念==，所有命令在事务中，并没有直接被执行！只有发起执行命令的时候才会执行！Exec

```bash
#隔离级别
SQL标准定义了4类隔离级别，包括了一些具体规则，用来限定事务内外的哪些改变是可见的，哪些是不可见的。低级别的隔离级一般支持更高的并发处理，并拥有更低的系统开销。
1、Read Uncommitted（读取未提交内容）
2、Read Committed（读取提交内容）
3、Repeatable Read（可重读）
4、Serializable（可串行化）
```

在MySQL中，实现了这四种隔离级别，分别有可能产生问题如下所示：

![img](https://img2018.cnblogs.com/blog/1646034/201904/1646034-20190430095830286-1397235000.png)

Redis的事务：

开启事务（multi）

命令入队

执行事务（exec）

> 正常执行事务

```bash
127.0.0.1:6379> MULTI              #开启事务
OK
127.0.0.1:6379> set k1 v1		#添加元素
QUEUED	     #加入队列
127.0.0.1:6379> set k2 v2		#添加元素
QUEUED   #加入队列
127.0.0.1:6379> set k3 v3		#添加元素
QUEUED   #加入队列
127.0.0.1:6379> GET k2		#获取key=k2的value
QUEUED   #加入队列
127.0.0.1:6379> set k3 vv		#更新kv
QUEUED   #加入队列
127.0.0.1:6379> exec      #执行事务
1) OK
2) OK
3) OK
4) "v2"
5) OK
```

> 放弃事务（DISCARD）

```bash
127.0.0.1:6379> MULTI              #开启事务
OK
127.0.0.1:6379> set k1 v1		#添加元素
QUEUED   #加入队列
127.0.0.1:6379> set k2 v2		#添加元素
QUEUED   #加入队列
127.0.0.1:6379> EXEC		  #执行事务
1) OK
2) OK
127.0.0.1:6379> MULTI            #开启事务
OK
127.0.0.1:6379> set k3 v3		#添加元素
QUEUED
127.0.0.1:6379> DISCARD		#取消上面的事务
OK
127.0.0.1:6379> EXEC		#出错，表明上面的multl被取消了
(error) ERR EXEC without MULTI
```

> 编译型异常（代码有问题！命令有错！），事务中所有的命名都不会执行

```bash
127.0.0.1:6379> MULTI            #开启事务
OK
127.0.0.1:6379> set k1 v1		#添加元素
QUEUED
127.0.0.1:6379> set k2 v2		#添加元素
QUEUED
127.0.0.1:6379> getset k2 v2        #getset key value
QUEUED
127.0.0.1:6379> getset k2      #错误命令
(error) ERR wrong number of arguments for 'getset' command     #报错
127.0.0.1:6379> getset k3 v3   		#添加元素
QUEUED
127.0.0.1:6379> EXEC        #执行
(error) EXECABORT Transaction discarded because of previous errors.  #报错
127.0.0.1:6379> get k1        #错误前面的命令也没有被执行
(nil)
```

> 运行时异常（1/0），如果事务队列中存在语法性，那么执行命令的时候，其他命令可以正常执行！，错误命令抛出异常

```bash
127.0.0.1:6379>  MULTI            #开启事务
OK
127.0.0.1:6379> set k1 v1		#添加元素
QUEUED
127.0.0.1:6379> set k2 v2		#添加元素
QUEUED
127.0.0.1:6379> INCR k1       #k1--value++ ,因为v1不是数字，所以执行时会报错。但是不是语法错误
QUEUED
127.0.0.1:6379> set k3 v3 		#在运行错误之后继续添加元素
QUEUED
127.0.0.1:6379> MGET k1 k3    #获得k1,k3的值
QUEUED
127.0.0.1:6379> EXEC          #执行
1) OK                
2) OK
3) (error) ERR value is not an integer or out of range         #第三步报错
4) OK
5) 1) "v1"
   2) "v3"

```

##### 事务监控

> **悲观锁**：
>
> - 认为什么时候都会出问题，无论做什么都会加锁



> **乐观锁**：
>
> - 很乐观，认为什么时候都不会出问题，所以不会上锁！更新数据的时候去判断一下，在此期间是否有人修改过这个数据。
> - 获取version
> - 更新时候比较version



==正常执行成功！==

```bash
127.0.0.1:6379> set money 100           #设置有100元
OK	
127.0.0.1:6379> set out 0		#设置花掉0元
OK
127.0.0.1:6379> WATCH money     #监控money
OK
127.0.0.1:6379> MULTI		#开启事务，正常情况下，没有其他线程对money，out进行操作。
OK
127.0.0.1:6379> DECRBY money 20		#money减少20
QUEUED
127.0.0.1:6379> INCRBY out 20		#out增加20
QUEUED
127.0.0.1:6379> exec          #执行
1) (integer) 80
2) (integer) 20
```



==在执行期间，另一个线程对value进行改变==

```bash
127.0.0.1:6379> set money 100               #设置有100元
OK
127.0.0.1:6379> set out 0          #设置花费0元
OK
127.0.0.1:6379> MULTI      #开启事务
OK
###
###在事务中间另一个线程改变了money的值，set money 1000
###
127.0.0.1:6379> DECRBY money 10
QUEUED
127.0.0.1:6379> INCRBY out 10
QUEUED
127.0.0.1:6379> exec       #执行，发现money按照1000，进行改变
1) (integer) 990
2) (integer) 10
```

在事务之前加上==监视==

```bash
127.0.0.1:6379> WATCH money        #进行监视
OK
127.0.0.1:6379> MULTI	     #开启事务
OK
###
###在事务中间另一个线程改变了money的值，set money 1000
###
127.0.0.1:6379> DECRBY money 10
QUEUED
127.0.0.1:6379> INCRBY out 10
QUEUED
127.0.0.1:6379> exec         #执行，发现输出为空，说明事务没有被执行
(nil)
```

加锁导致事务失败

![image-20210203153841980](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210203153841980.png)



### Jedis

使用java操作Redis

> 什么是jredis，是redis官方推荐的java连接开发工具

> 测试

1、导入相应的依赖

```xml
  <dependencies>
<!--    导入JRedis包-->
      <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.2.0</version>
        </dependency>


<!--        fastjson-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.62</version>
        </dependency>

    </dependencies>
```

2、编码测试

- 连接数据库

```java
Jedis jedis = new Jedis("121.36.0.84",6379);
System.out.println(jedis);
//redis.clients.jedis.Jedis@2ff4acd0
```

- 操作命令(Jedis的所有指令就是我们之前学习的所有指令)

```java
//获取全部keys
Set<String> keys = jedis.keys("*");
System.out.println("当前全部keys: \n"+keys);
```

- 断开连接

```java
//断开连接
jedis.close();
```

##### 常用API

> 就是上面对应的操作 

- string
- list
- set
- hash
- zset

> 事务

### SpringBoot整合

SpringBoot操作数据：spring-data  jpa  jdbc  mongodb  redis

> **整合测试**

说明：在springboot2.x之后，原来是用的==jedis换成了lettuce==

jedis：采用的是直连，多个线程操作的话，是不安全的。如果避免不安全，使用jedis pool连接池！Bio模式！

lettuce：采用netty，实例可以在多个线程中进行共享，不存在线程不安全的情况！ 可以减少线程的数量，Nio模式

源码分析：

```java
public class RedisAutoConfiguration {
    public RedisAutoConfiguration() {
    }

    @Bean
    @ConditionalOnMissingBean(
        name = {"redisTemplate"}
    )   //我们可以自定义一个redisTemplate来进行替换这个默认的！
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
        //默认的RedisTemplate没有过多的设置，redis对象都是需要序列化！
        //两个泛行都是Object，Object类型，我们后面使用需要强制转化<string,Object>
        RedisTemplate<Object, Object> template = new RedisTemplate();
        template.setConnectionFactory(redisConnectionFactory);
        return template;
    }

    @Bean
    @ConditionalOnMissingBean    //由于string是最常用的，所以单独提出一个bean
    public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
        StringRedisTemplate template = new StringRedisTemplate();
        template.setConnectionFactory(redisConnectionFactory);
        return template;
    }
}

```

##### 导入依赖

```java
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

##### 配置连接

```java
spring.redis.host=121.36.0.84
spring.redis.port=6379
```

##### 测试

```java
    @Test
    void contextLoads() {
        //redisTemplate
        //opsForValue()   操作字符串  类似String
        //opsForList()    操作List

        //除了基本的操作，我们常用的操作都可以直接通过RedisTemplate进行实现，比如事务和基本的CRUD

        //获取连接对象
        RedisConnection connection = Objects.requireNonNull(redisTemplate.getConnectionFactory()).getConnection();
//        connection.flushAll();
//        connection.flushDb();
        redisTemplate.opsForValue().set("k1","一二三");
        System.out.println(redisTemplate.opsForValue().get("k1"));
    }
```

##### 序列化

![image-20210217221906215](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210217221906215.png)



![image-20210217222034403](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210217222034403.png)

==**直接传输对象会序列化的错误**==

![image-20210217225042600](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210217225042600.png)

##### 自定义序列化

```java
 @Test
    public void TestSeria() throws JsonProcessingException {
        User user = new User("王柯栋", 18);
        //将对象序列化为json
        String userJson = new ObjectMapper().writeValueAsString(user);
        //将对象写入redis
        redisTemplate.opsForValue().set("user",userJson);
        System.out.println(redisTemplate.opsForValue().get("user"));
    }
```

> 未使用自定义序列化

![image-20210217233320580](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210217233320580.png)

> 使用序列化

![image-20210217233336827](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210217233336827.png)



### Redis.conf详解

启动的时候通过配置文件来启动！（配置详细讲解：https://www.cnblogs.com/coder-lzh/p/9614244.html）

> 单位，对大小写不敏感

![image-20210218102055133](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210218102055133.png)

> 包含多个配置文件；include~~import

![image-20210218102202693](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210218102202693.png)

> 网络



```bash
bind 127.0.0.1          #绑定的ip
protected-mode yes      #保护模式
port 6379 			#端口设置
```

> 通用配置

```bash
daemonize yes #以守护进程的方式运行，默认是no
pidfile /var/run/redis_6379.pid      #如果以后台的方式运行，我们就需要指定一个pid文件！
#日志
#Specify the server verbosity level.
#This can be one of：
#debug（a lot of information，useful for development/testing） 测试/开发
#verbose（many rarely useful info，but not a mess like the debug level）
#notice（moderately verbose，what you want in production probably）   生产环境
#warning（only very important/critical messages are logged）
loglevel notice
logfile “”		#日志文件地址 
databases 16  #默认的数据库数量 
```

> 快照

 持久化，在规定的时间内执行了多少次操作 ，则会持久化到文件rdb.aof

redis是内存数据库，如果没有持久化，那会数据断电即失！

```bash
#如果900s内，如果至少有一个1key进行了修改，我们及进行持久化操作
save 900 1
#如果300s内，如果至少10key进行了修改，我们及进行持久化操作
save 300 10
#如果60s内，如果至少10000key进行了修改，我们及进行持久化操作
save 60 10000
#我们之后学习持久化，会自己定义这个测试

stop-writes-on-bgsave-error yes  #持久化如果出错，是否还需要继续工作

rdbcompression yes  #是否压缩rdb文件，需要消耗一些cpu资源

rdbchecksum yes#保存rdb文件的时候，进行错误的检查校验！

dir./#rdb文件保存的目录！
```

> REPLICATION  复制



docker run -v /home/userwkd/redis/data:/data -v /home/userwkd/redis/conf/redis.conf:/etc/redis/redis.conf -d -p 6379:6379 redis redis-server --appendonly yes --requirepass "wang0510"

docker exec -it ed18 redis-cli



> 安全|SECURITY 

```bash
#在配置文件中设置密码
# requirepass foobared
requirepass 123456
#在命令行中设置密码
config set requirepass "123456"
#获取redis命令
config get requirepass
```

> 限制|CLIENTS

```bash
# maxclients 10000     #最大10000客户端数
```

> 内存设置

```bash
 maxmemory <bytes>   #最大内存容量
 # maxmemory-policy noeviction  #内存达到上限后的处理策略
    1、volatile-1ru：只对设置了过期时间的key进行LRU（默认值）
    2、allkeys-1ru：删除1ru算法的key
    3、volatile-random：随机删除即将过期key
    4、allkeys-random：随机删除5、volatile-tt1：删除即将过期的6、noeviction：永不过期，返回错误
```

> APPEND ONLY MODE 模式  |  aof配置

```bash
appendonly no  #默认是不开启的，默认使用rdb持久化
appendfilename "appendonly.aof"   #持久化文件名字
# appendfsync always  #每次修改斗湖同步sync
appendfsync everysec  #每秒执行一次同步sync，可能会丢失这1s的数据
# appendfsync no    #不执行同步sync，这个时候操作系统自己同步数据，速度最快
```

具体的配置，在持久化中进行

### Redis持久化

Redis是内存数据库，如果不见内存中的数据状态保存到磁盘，那么一但服务器进程退出，服务器的数据库状态也会消失。所以Redis提供了持久化功能。

在主从复制中，rdb就是备用了！

##### RDB(Redis DataBase)

> 什么是RDB

![ ](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210218115046855.png)



在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里。

Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何I0操作的。这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF 方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。我们默认的就是RDB，一般情况下不修改这个配置。

有时候，在生产环境中，我们会对其进行备份。

==rdb保存的默认文件是dump.rdb==

==aof保存的默认文件时appendonly.aof==



> 开启rdb持久化

```bash
127.0.0.1:6379> save
OK
127.0.0.1:6379> Set rdb rdb
OK
```

![image-20210218121615983](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210218121615983.png)

> RDB快照保存策略

![image-20210218223616845](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210218223616845.png)

> 触发机制

 1、save的规则满足的情况下，会自动触发rdb规则

2、执行flushall命令，也会触发rdb规则

3、退出redis，也会产生rdb文件！

备份就会自动生成**dump.rdb**

> 如何恢复rdb文件！

 1、只需将rdb文件放在我们redis启动目录就可以，redis启动时就会自动检查dump.rdb，恢复其中的数据

2、查看需要存在的位置

```bash
127.0.0.1:6379> CONFIG GET dir
1) "dir"
2) "/etc"          #如果在这个目录下存在dump.rdb文件，启动就会自动恢复其中的数据。
```

> 优点/缺点

优点：

- 适合大规模的数据恢复！
- 如果对数据的完整行要求不高！

缺点：

- 需要一定的时间间隔进程操作！如果redis意外宕机了，这个最后一次修改的数据就没了。
- fork进程的时候，会占用一定的内存空间

##### AOF(Append Only File)

将我们的所有命令记录下来，history，恢复的时候就把这个文件全部执行一遍！

> AOF是什么？

![image-20210218225747282](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210218225747282.png)



以日志的形式来记录每个写操作，将Redis执行过的所有指令记录下来（**读操作不记录**），只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

==Aof保存的是appendonly.aof文件==

![image-20210218230242630](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210218230242630.png)

```bash
appendonly 默认是不开启的。需要手动开启
重启，redis 就可以生效了！
如果这个aof文件有错位，这时候redis是启动不起来的吗，我们需要修复这个aof文件
redis给我们提供了一个工具--redis-check-aof  --fix
```

> 优点|缺点

优点：

- 每一次修改，都同步，文件完整性会更好
- 每秒同步一次，可能会丢失一秒的数据
- 从不同步，效率是最高的

缺点：

- 相对于数据文件来说，aof远远大于rdb，修复的速度也慢与rdb。
- aof运行效率也要比rdb慢，所以默认是rdb持久化

> 重写规则

aof默认就是文件的无限追加。文件越来越大

![image-20210218232709614](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210218232709614.png)

##### 扩展

1、RDB持久化方式能够在指定的时间间隔内对你的数据进行快照存储

2、AOF持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以Redis协议追加保存每次写的操作到文件未尾，Redis还能对AOF文件进行后台重写，使AOF文件的体积不至于过大。

3、只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化

4、同时开启两种持久化方式

- 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整。
- RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢？作者建议不要，因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，留着作为一个万一的手段。

5、性能建议

- 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留 **save 900 1**这条规则。
- 如果Enable AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了，代价一是带来了持续的IO，二是AOF rewrite的最后将 rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。
- 如果不Enable AOF，仅靠Master-Slave Repllcation 实现高可用性也可以，能省掉一大笔IO，也减少了rewrite时带来的系统波动。代价是如果Master/Slave同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个Master/Slave中的RDB文件，载入较新的那个，微博就是这种架构。



### Redis发布订阅

Redis发布订阅(pub/sub)是一种消息通信模式“发送者（pub）发送消息，订阅者（sub）接收消息。

Redis客户端可以订阅任意数量的频道。

订阅/发布消息图：

第一个：消息发送者，第二个：频道，第三个：消息订阅者

![image-20210219110021940](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210219110021940.png)

> **订阅**

```bash
#订阅频道    SUBSCRIBE channelName
127.0.0.1:6379> SUBSCRIBE wkdchannel
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "wkdchannel"
3) (integer) 1
1) "message"
2) "wkdchannel"
3) "Hello!!!"
1) "message"
2) "wkdchannel"
3) "comcomcom"
```



```bash
#订阅频道    SUBSCRIBE channelName
127.0.0.1:6379> SUBSCRIBE wkdchannel 
Reading messages... (press Ctrl-C to quit)
1) "subscribe"        #订阅
2) "wkdchannel"			#频道wkdchannel
3) (integer) 1
1) "message"		#消息提示
2) "wkdchannel"		#频道
3) "comcomcom"		#内容
```



```bash
#发布信息   PUBLISH channel message
127.0.0.1:6379> PUBLISH wkdchannel Hello!!!
(integer) 1
127.0.0.1:6379> PUBLISH wkdchannel comcomcom
(integer) 2
```



> 使用场景

- 实时消息系统
- 实时聊天（频道当做聊天室，将信息回显给所有人）
- 订阅，关注系统都是可以的
- 稍微负责点的，我们可以用消息中间件来实现

### Redis主从复制

##### 概念

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点（master/leader），后者称为从节点（slave/follower）；

数据的==复制是单向==的，只能由主节点到从节点。==aster以写为主，Slave以读为主==。默认情况下，每台Redis服务器都是主节点；且一个主节点可以有多个从节点（或没有从节点），但一个从节点只能有一个主节点。
主从复制的==作用主要==包括：
1、数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。
2、故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。
3、负载均衡：在主从复制的基础上，配合==读写分离==，可以由**主节点提供写服务，由从节点提供读服务**（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。
4、高可用（集群）基石：除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。



一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的（宕机），原因如下：
1.从==结构==上，第个Redis服务器会发生单点故赠，并且一台服务然需要处理所有的请求负载，压力较大；
2.从==容量==上，单个Redis服务器内存容量有限，就第一台Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，单台Redis最大使用内存不应该超过206。
电商网站上的商品，一般都一次上传，无数次浏览的，说专业点也就是“多读少写”。

对于这种场原，我们可以使如下这种架构：

<img src="C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210220114948798.png" alt="image-20210220114948798" style="zoom:67%;" />

主从复制，读写分离！80%的情况下都是在进行读操作！缓解服务器压力，架构中经常使用，一主二从。



##### 环境配置

只配置从库，不用配置主库（因为redis默认自己就是主库）。

```bash
#查看当前库的信息
127.0.0.1:6379> INFO replication
# Replication
role:master   #角色
connected_slaves:0  #没有从机
master_replid:70b81b37b11c7c8c0c2c2e4700ec43051f275bfa
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```



![image-20210220122540568](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210220122540568.png)



##### 一主二从

默认情况下，每台Redis服务器都是主节点；一般情况下我们只要配置从机就好了。

认老大！主（79）从（80/81）

==**由从机上进行配置（SLAVEOF）**==

```bash
#salver1  172.17.0.2  6379 ----->   6379
127.0.0.1:6379> SLAVEOF 172.17.0.4 6379
OK
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:172.17.0.4
master_port:6379
master_link_status:up
#slaver2  172.17.0.3  6379 ----->   6380
127.0.0.1:6379> SLAVEOF 172.17.0.4 6379
OK
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:172.17.0.4
master_port:6379
master_link_status:up
#主机INFO
127.0.0.1:6379> INFO replication
# Replication
role:master        #角色
connected_slaves:2      #从机个数
slave0:ip=172.17.0.3,port=6379,state=online,offset=32756,lag=0
slave1:ip=172.17.0.2,port=6379,state=online,offset=32756,lag=1
```



==**主写从读**==

```bash
172.17.0.3:6379> set k1 v1     #master
OK
172.17.0.2:6379> keys *   #slaver
1) "k1
172.17.0.4:6379> keys *		#slaver
1) "k1
```



真实的主从配置应该是在配置文件中配置，这样的话是永久的，我们这里使用的是命令，暂时的！！！

![image-20210220222112051](C:\Users\Cristiano-Ronaldo\AppData\Roaming\Typora\typora-user-images\image-20210220222112051.png)

> 细节

1、主机可以写，从机不能。所有的主机信息都会自动备份到从机。

```bash
127.0.0.1:6379> set k3 v3
(error) READONLY You can't write against a read only replica.
```

2、主机下线之后，从机依旧连接到主机。但是没有写操作

​		主机回来之后，一切如旧。

3、【命令行状态下】从机宕机之后，**主机上的salve就减去一**，在这个期间，主机上写入的，从机再次上线之后不能读了。

那是因为，**命令行状态下**的从机宕机后再上线就恢复了主机身份



















