## Redis

#### **Redis的五大数据类型**

**1.String(字符串)**

​	**(1)：expire key ：**设置key的过期时间

​	**(2)：ttl key：**查看key的过期时间**[-1表示永不过期，-2表示已经过期]**

​	**(3)：del key：**删除key

​	**(4)：append key value：**向key中追加值

```shell
127.0.0.1:6381> get k1
"v1"
127.0.0.1:6381> APPEND k1 abc
(integer) 5
127.0.0.1:6381> get k1
"v1abc"
127.0.0.1:6381> 
```

​	**(5)：incr key：**将key自增1**[有原子性]**

​	**(6)：incrby key step：**将key自增step

​	**(7)：decrby key step：**将key自减step

​	**(8)：getrange key start end：**获取 start 到 end的范围的key的值

​	**(9)：setrange key start end value：**在start 到 end范围内设置值

​	**(10)：setex key 10 value：**设置值并且设置过期时间

​	**(11)：setnx key value：**设置值**[如果不存在]**

​	**(12)：mset k1 v1 k2 v2 k3 v3.......：**设置多个key

​	**(13)：mget k1 k2 k3.......：**获取多个值

​	**(14)：msetnx k1  v1 k2  v2 k3 v3......：** **[部分key存在不成功]**



#### **2.Hash(哈希)：是一个string类型的field和value映射表**

​	**kv模式不变，但是value是一个键值对**

​	**(1)：hset   user  id   11：** 设置user  id 的值

​	**(2)：hget  user  id：** 获取 id的值

​	**(3)：hmset  customer  id  11 name  jack  age  20：**设置多个键值对

​	**(4)：hgetall  customer：**获取所有的value 

​	**(5)：hdel  customer  name：**删除name

​	**(6)：hexists customer  name：**判断是否存在key

​	**(7)：hkeys customer：**获取所有的key

​	**(8)：hvals customer：**获取所有的value

​	**(9)：hincrby customer age 2：**将指定的key增加step

​	**(10)：hsetnx customer age 26：**如果不存在key就设置

​	

**3.List(集合)：底层LinkedList**

​	**(1)：lpush list01 1 2 3 4 5........：** **列表采用头插法**

​	**(2)：lrange list01 0 -1........：** **取值[5,4,3,2,1]**

​	**(3)：rpush	| rrange........：** **采用尾插法**

​	**(4)：lpop list01：** pop出左边的第一个元素

​	**(5)：rpop list01：** pop出右边第一个元素

​	**(6)：ltrim list01 starIndex endIndex：** **截取list01从start到 end的元素重新赋给list01**

​	**(7)：RpopLpush list01 list02：** **从list01的(右端)尾部pop出一个元素加入到list02的(左端)头部**

​	**(8)：lset list02 index value：**将list02的index位置设置值为value

​	**(9)：Linsert key before/after 值1 值2：**把值2插入到值1的前/后



**4.Set(集合)：无序无重复集合**

​	**(1)：sadd set01 1 1 2 2 3 3：**把多个值去重后加入set01

​	**(2)：smembers set01：**查看set01的值

​	**(3)：scard set01：**获取集合的元素个数

​	**(4)：srem set01 value：**删除集合的值

​	**(5)：srandmember set01 3：**在set01中随机抽3个值

​	**(6)：smove set01 set02	5：**将set01的5移动到set02

​	**(7)：sdiff set01 set02：**求set01 和 set02 的差集

​	**(8)：sinter set01 set02：**求交集合

​	**(9)：sunion set01 set02：**求并集合



**5.Zset(sorted set：有序集合)：**有序无重复(**按照内置的[scores]分数排序**)

​	**(1)：zadd  zset01  v1 score v2  score v3 score...........：**给zset01设置多个key并且设置分数**[score]**

​	**(2)：zrange zset01  0   -1 withscores：**列出全部的元素和分数

​	**(3)：zrem zset01  v1：**删除v1

​	**(4)：zcount  zset01  60  80：**统计60-80分的值

​	**(5)：zrank  zset01  v1：**拿到v1的排名(下标值)



#### Units单位

```shell
# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.

```

#### INCLUEDS

```shell
################################## INCLUDES ###################################

# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis servers but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# Note that option "include" won't be rewritten by command "CONFIG REWRITE"
# from admin or Redis Sentinel. Since Redis always uses the last processed
# line as value of a configuration directive, you'd better put includes
# at the beginning of this file to avoid overwriting config change at runtime.
#
# If instead you are interested in using includes to override configuration
# options, it is better to use include as the last line.
#
# include /path/to/local.conf
# include /path/to/other.conf

```

#### redis后台启动

```shell

################################# GENERAL #####################################

# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
# When Redis is supervised by upstart or systemd, this parameter has no impact.
daemonize yes

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#                        requires "expect stop" in your upstart job config
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#                        on startup, and updating Redis status on a regular
#                        basis.
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."

```

#### redis日志级别

```java

# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice

# Specify the log file name. Also the empty string can be used to force
# Redis to log on the standard output. Note that if you use standard
# output for logging but daemonize, logs will be sent to /dev/null
logfile ""
```

#### LIMITS限制

```java

# Set the max number of connected clients at the same time. By default
# this limit is set to 10000 clients, however if the Redis server is not
# able to configure the process file limit to allow for the specified limit
# the max number of allowed clients is set to the current file limit
# minus 32 (as Redis reserves a few file descriptors for internal uses).
#
# Once the limit is reached Redis will close all the new connections sending
# an error 'max number of clients reached'.
#
# IMPORTANT: When Redis Cluster is used, the max number of connections is also
# shared with the cluster bus: every node in the cluster will use two
# connections, one incoming and another outgoing. It is important to size the
# limit accordingly in case of very large clusters.
#
# maxclients 10000 //最大连接10000

```

#### 缓存淘汰策略

```java
# maxmemory <bytes>

# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select one from the following behaviors:
#
# volatile-lru -> Evict using approximated LRU, only keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU. //LRU缓存淘汰算法
# volatile-lfu -> Evict using approximated LFU, only keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU. //LFU缓存淘汰算法
# volatile-random -> Remove a random key having an expire set.
# allkeys-random -> Remove a random key, any key. //随机
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)//有限时间
# noeviction -> Don't evict anything, just return an error on write operations.(默认)
#
# LRU means Least Recently Used //最近最少使用
# LFU means Least Frequently Used 
#
# Both LRU, LFU and volatile-ttl are implemented using approximated
# randomized algorithms.
#
# Note: with any of the above policies, when there are no suitable keys for
                                                              990,8         48%
# maxmemory-policy noeviction //默认

```



#### **Redis持久化操作------(RDB和AOF)**

（1）**RDB**：行话将的Snapshot快照，**在指定时间间隔内将内存中的数据集快照写入磁盘**

（2）Redis会**单独创建一个子进程（fork）**来进行持久化，会将数据写入到一个临时文件

​			，待持久化过程结束后，再用这个临时文件替换上次持久化好的文件，整个过程中，

​			主进程不进行任何IO操作，确保提高性能，**如果需要大规模恢复数据并且对数据**，

​			**并且对数据的完整性(CAP中的一致性)不是非常敏感**，**建议使用RDB**，会比AOF方式高效

（3）fork：复制一个与当前进程一样的进程，新的进程的所有数据信息和原进程一致

（4）rdb保存的是dump.rdb文件

（5）配置文件的位置

（6）**劣势**：在最后一次快照可能会数据丢失，需要两倍的内存空间，并且在数据集非常大时很耗时（fork）

```java
################################ SNAPSHOTTING  ################################

# Save the DB to disk.
#
# save <seconds> <changes>
# //如果在给定的时间范围内，执行了write的操作次数
# Redis will save the DB if both the given number of seconds and the given
# number of write operations against the DB occurred.
#
# Snapshotting can be completely disabled with a single empty string argument
# as in following example:
#
# save ""
#
# Unless specified otherwise, by default Redis will save the DB:
#   * After 3600 seconds (an hour) if at least 1 key changed
#   * After 300 seconds (5 minutes) if at least 100 keys changed
#   * After 60 seconds if at least 10000 keys changed
# //你可以选择以下3种方式的任意一条
# You can set these explicitly by uncommenting the three following lines.
#
# save 3600 1	// 3600s 被改过1次
# save 300 100  //300s内 key被改过100次
save 20 3	//20s内 key被改过3次
    
    
# By default Redis will stop accepting writes if RDB snapshots are enabled
# (at least one save point) and the latest background save failed.
# This will make the user aware (in a hard way) that data is not persisting
# on disk properly, otherwise chances are that no one will notice and some
# disaster will happen.
#
# If the background saving process will start working again Redis will
# automatically allow writes again.
#
# However if you have setup your proper monitoring of the Redis server
# and persistence, you may want to disable this feature so that Redis will
# continue to work as usual even if there are problems with disk,
# permissions, and so forth.
stop-writes-on-bgsave-error yes	//在save的时候出错了前台就停止写
    
    
# Compress string objects using LZF when dump .rdb databases? //是否启动压缩算法
# By default compression is enabled as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.
rdbcompression yes

# Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
# This makes the format more resistant to corruption but there is a performance
# hit to pay (around 10%) when saving and loading RDB files, so you can disable it
# for maximum performances.
#
# RDB files created with checksum disabled have a checksum of zero that will
# tell the loading code to skip the check.
rdbchecksum yes

# Enables or disables full sanitation checks for ziplist and listpack etc when
# loading an RDB or RESTORE payload. This reduces the chances of a assertion or
# crash later on while processing commands.
# Options:
#   no         - Never perform full sanitation
#   yes        - Always perform full sanitation
#   clients    - Perform full sanitation only for user connections.
#                Excludes: RDB files, RESTORE commands received from the master
#                connection, and client connections which have the
#                skip-sanitize-payload ACL flag.
# The default should be 'clients' but since it currently affects cluster
# resharding via MIGRATE, it is temporarily set to 'no' by default.
#
# sanitize-dump-payload no

# The filename where to dump the DB //rdb的文件名，redis每次启动会读取该文件
dbfilename dump.rdb

```

#### 测试 save 20 3

```java
127.0.0.1:6379> set k1 v2
OK
127.0.0.1:6379> set k1 v3
OK
127.0.0.1:6379> set k1 v4 //改动3次
OK
127.0.0.1:6379> exiy
(error) ERR unknown command `exiy`, with args beginning with: 
127.0.0.1:6379> exit
[root@centos7 myredis]# ll
总用量 124
-rw-r--r--. 1 root root   104 3月  18 22:07 dump.rdb //产生了dump.rdb
-rw-r--r--. 1 root root   183 3月  15 10:40 redis6379.conf
-rw-r--r--. 1 root root   183 3月  15 10:42 redis6380.conf
-rw-r--r--. 1 root root   183 3月  15 10:43 redis6381.conf
-rw-r--r--. 1 root root   183 3月  15 10:44 redis6389.conf
-rw-r--r--. 1 root root   183 3月  15 10:44 redis6390.conf
-rw-r--r--. 1 root root   183 3月  15 10:44 redis6391.conf
-rw-r--r--. 1 root root 92212 3月  15 09:21 redis.conf
-rw-r--r--. 1 root root   392 3月  15 10:10 sentinel.conf

```

#### AOF持久化操作-------(Append Only File)

​	（1）以**日志**的形式来记录**每一个写操作**，将Redis所执行的所有指令记录下来（读操作不记录），

​			**只许追加文件**但不可以改写文件，redis启动会读取该文件**重构数据集**

​	（2）优点：灵活的配置，数据完整性高

​	（3）缺点：相同的数据集的数据而言aof远大于aof，恢复速度慢于rdb

​	（4）aof跟rdb同时存在时，优先加载aof

​	（5）修复aof文件	

```shell
redis-check-aof --fix appendnoly.aof
```



```java
############################## APPEND ONLY MODE ###############################

# By default Redis asynchronously dumps the dataset on disk. This mode is
# good enough in many applications, but an issue with the Redis process or
# a power outage may result into a few minutes of writes lost (depending on
# the configured save points).
#
# The Append Only File is an alternative persistence mode that provides
# much better durability. For instance using the default data fsync policy
# (see later in the config file) Redis can lose just one second of writes in a
# dramatic event like a server power outage, or a single write if something
# wrong with the Redis process itself happens, but the operating system is
# still running correctly.
#
# AOF and RDB persistence can be enabled at the same time without problems.
# If the AOF is enabled on startup Redis will load the AOF, that is the file
# with the better durability guarantees.
#
# Please check http://redis.io/topics/persistence for more information.

appendonly no //默认关闭

# The name of the append only file (default: "appendonly.aof")

appendfilename "appendonly.aof"
    

```

​	（6）AOF的三种策略 **(no | always | everysec)**

```java
# Redis supports three different modes:
#
# no: don't fsync, just let the OS flush the data when it wants. Faster. //让系统决定
# always: fsync after every write to the append only log. Slow, Safest. //同步---每次追加(存在性能问题)
# everysec: fsync only one time every second. Compromise.	//异步每秒追加
#
# The default is "everysec", as that's usually the right compromise between
# speed and data safety. It's up to you to understand if you can relax this to
# "no" that will let the operating system flush the output buffer when
# it wants, for better performances (but if you can live with the idea of
# some data loss consider the default persistence mode that's snapshotting),
# or on the contrary, use "always" that's very slow but a bit safer than
# everysec.
#
# More details please check the following article:
# http://antirez.com/post/redis-persistence-demystified.html
#
# If unsure, use "everysec".

# appendfsync always
appendfsync everysec

```

​	（7）**AOF重写** （Rewrite）

​			**触发机制：**记录上一次aof文件的大小，默认配置当aof文件大小是**上一次rewirte后小大的一倍**并且**文件大于64M时触发**	

```java
# Specify a percentage of zero in order to disable the automatic AOF
# rewrite feature.

auto-aof-rewrite-percentage 100 //设置重写的基准值和百分比
auto-aof-rewrite-min-size 64mb	

```

#### Redis事务

```java
//可以一次执行多个命令，本质是一组命令，一个事务中的所有命令都会被序列化，按顺序地串行化执行。

一个队列中，一次性，顺序性，排他性的执行一系列命令。
    redis部分支持事务，不保证原子性
```

​	（1）**discard**：取消事务

```java
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 22
QUEUED
127.0.0.1:6379(TX)> set k3 33
QUEUED
127.0.0.1:6379(TX)> DISCARD //放弃组队（事务）
OK
127.0.0.1:6379> 

```

​	（2）**exec**：执行所有事务块内的命令

```java
127.0.0.1:6379(TX)> get k2
QUEUED
127.0.0.1:6379(TX)> get k3
QUEUED
127.0.0.1:6379(TX)> EXEC //执行
1) OK
2) OK
3) "v2"
4) "v3"
127.0.0.1:6379> 

```

​	（3）**multi**：一个事务的开始

```java
127.0.0.1:6379> MULTI //开启
OK
127.0.0.1:6379(TX)> 

```

```java
127.0.0.1:6379(TX)> set k2 v2
QUEUED //入队
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> 


```

​	（4）**watch key1 key2.....**：监视key

​	（5）**unwatch**：取消监视所有key

**乐观锁，悲观锁，CAS**



#### Redis消息发布和订阅机制

​	（1）订阅3个频道

```java
127.0.0.1:6379> SUBSCRIBE c1 c2 c3
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "c1"
3) (integer) 1
1) "subscribe"
2) "c2"
3) (integer) 2
1) "subscribe"
2) "c3"
3) (integer) 3


```

```java
127.0.0.1:6379> PUBLISH c2 hello-redis //发布消息
(integer) 1
127.0.0.1:6379> 

```



#### 高可用之Redis主从模式

（1）**读(slave)写(master)分离 + 容灾恢复**

```java
	从库配置：slaveof  + 主库ip  +  port
	修改端口号，pid_file,dump.rdb命名，logfile命名
```

**启动redis主从服务器**

```java
[root@centos7 myredis]# redis-server redis6379.conf 
[root@centos7 myredis]# redis-server redis6380.conf 
[root@centos7 myredis]# redis-server redis6381.conf 
[root@centos7 myredis]# ps -ef | grep redus
root      71625  12672  0 12:36 pts/3    00:00:00 grep --color=auto redus
[root@centos7 myredis]# ps -ef | grep redis
root      71554      1  0 12:35 ?        00:00:00 redis-server *:6379
root      71566      1  0 12:35 ?        00:00:00 redis-server *:6380
root      71578      1  0 12:36 ?        00:00:00 redis-server *:6381
root      71635  12672  0 12:36 pts/3    00:00:00 grep --color=auto redis
[root@centos7 myredis]# 

```

（2）查看服务器角色

```shell
127.0.0.1:6379> INFO replication
# Replication
role:master
connected_slaves:0
master_failover_state:no-failover
master_replid:37905c0fde4d0e503e75e598ee64ef7913eb3ad6
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379> 

```

（3）成为指定服务器的从机 **{master在slave第一次连接的主动将数据同步到slave}**

```java
127.0.0.1:6380> SLAVEOF 127.0.0.1 6379 //
OK
127.0.0.1:6380> //成为从服务器后，master每一次写操作---由从服务器主动请求master数据同步

```

```java
# Replication
role:slave //角色是从服务器
master_host:127.0.0.1
master_port:6379
master_link_status:up
master_last_io_seconds_ago:4
master_sync_in_progress:0
slave_repl_offset:220
slave_priority:100
slave_read_only:1
connected_slaves:0
master_failover_state:no-failover
master_replid:aaf68ba117e6ee451b8e6c4c3107ce207199de67
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:220
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:220
127.0.0.1:6380> 

    
# Replication
role:master //主机
connected_slaves:2
slave0:ip=127.0.0.1,port=6380,state=online,offset=276,lag=1
slave1:ip=127.0.0.1,port=6381,state=online,offset=276,lag=1
master_failover_state:no-failover
master_replid:aaf68ba117e6ee451b8e6c4c3107ce207199de67
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:276
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:276
127.0.0.1:6379> 


```

#### 哨兵模式(sentinel)

（1）创建sentinel.conf文件

```java
sentinel monitor mymaster 127.0.0.1 6379 1 //表示检控的主机，挂掉后从机票数>1被选举

```

（2）启动哨兵

```shell
[root@centos7 myredis]# redis-sentinel sentinel.conf 
74381:X 19 Mar 2022 13:20:19.298 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
74381:X 19 Mar 2022 13:20:19.298 # Redis version=6.2.1, bits=64, commit=00000000, modified=0, pid=74381, just started
74381:X 19 Mar 2022 13:20:19.298 # Configuration loaded
74381:X 19 Mar 2022 13:20:19.300 * Increased maximum number of open files to 10032 (it was originally set to 1024).
74381:X 19 Mar 2022 13:20:19.300 * monotonic clock: POSIX clock_gettime
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 6.2.1 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in sentinel mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 26379
 |    `-._   `._    /     _.-'    |     PID: 74381
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

74381:X 19 Mar 2022 13:20:19.301 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
74381:X 19 Mar 2022 13:20:19.304 # Sentinel ID is 5daf0e65d1549c3314595a9cf5646b5372bc59d8
74381:X 19 Mar 2022 13:20:19.304 # +monitor master mymaster 127.0.0.1 6379 quorum 1
74381:X 19 Mar 2022 13:20:19.306 * +slave slave 127.0.0.1:6380 127.0.0.1 6380 @ mymaster 127.0.0.1 6379
74381:X 19 Mar 2022 13:20:19.310 * +slave slave 127.0.0.1:6381 127.0.0.1 6381 @ mymaster 127.0.0.1 6379

//挂掉master
74381:X 19 Mar 2022 13:22:09.727 * +slave-reconf-done slave 127.0.0.1:6380 127.0.0.1 6380 @ mymaster 127.0.0.1 6379
74381:X 19 Mar 2022 13:22:09.786 # +failover-end master mymaster 127.0.0.1 6379
74381:X 19 Mar 2022 13:22:09.786 # +switch-master mymaster 127.0.0.1 6379 127.0.0.1 6381 //自动切换mymaster 6379------->6381
74381:X 19 Mar 2022 13:22:09.786 * +slave slave 127.0.0.1:6380 127.0.0.1 6380 @ mymaster 127.0.0.1 6381
74381:X 19 Mar 2022 13:22:09.786 * +slave slave 127.0.0.1:6379 127.0.0.1 6379 @ mymaster 127.0.0.1 6381

```

（3）重新启动6379

```shell
127.0.0.1:6379> INFO replication
# Replication
role:slave	//变成slave
master_host:127.0.0.1
master_port:6381
master_link_status:up
master_last_io_seconds_ago:1
master_sync_in_progress:0
slave_repl_offset:17651
slave_priority:100
slave_read_only:1
connected_slaves:0
master_failover_state:no-failover
master_replid:ee2c2137c54c9048e3fc093b85e9e49725abda7e
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:17651
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:16964
repl_backlog_histlen:688
127.0.0.1:6379> 

```

