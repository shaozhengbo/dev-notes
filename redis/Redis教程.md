# Redis教程

## 1. 简介

Redis 是完全开源免费的，遵守BSD协议，是一个高性能的key-value数据库。

Redis 与其他 key - value 缓存产品有以下三个特点：

- Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- Redis支持数据的备份，即master-slave模式的数据备份。



### Redis 优势

- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。



### Redis与其他key-value存储有什么不同？

Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。

Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，应为数据量不能大于硬件内存。在内存数据库方面的另一个优点是， 相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。 同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。



## 2. 安装

### docker安装

```sh
# 拉取镜像
docker pull redis:latest

# 启动并设置密码
docker run -d --name myredis -p 6379:6379 redis --requirepass "123456"
```



### 其他安装

参照官方手册

https://www.redis.net.cn/tutorial/3503.html



## 3. 配置

Redis 的配置文件位于 Redis 安装目录下，文件名为 redis.conf。

你可以通过 **CONFIG** 命令查看或设置配置项。

官方说明

https://www.redis.net.cn/tutorial/3504.html

```SHELL
> CONFIG GET *
rdbchecksum
yes

# Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
daemonize
no
io-threads-do-reads
no
lua-replicate-commands
yes
always-show-logo
no
protected-mode
no

# 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大
rdbcompression
yes
rdb-del-sync-files
no

# 指定是否激活重置哈希，默认为开启（后面在介绍Redis的哈希算法时具体介绍）
activerehashing
yes
stop-writes-on-bgsave-error
yes
dynamic-hz
yes
lazyfree-lazy-eviction
no
lazyfree-lazy-expire
no
lazyfree-lazy-server-del
no
lazyfree-lazy-user-del
no
repl-disable-tcp-nodelay
no
repl-diskless-sync
no
gopher-enabled
no
aof-rewrite-incremental-fsync
yes
no-appendfsync-on-rewrite
no
cluster-require-full-coverage
yes
rdb-save-incremental-fsync
yes
aof-load-truncated
yes
aof-use-rdb-preamble
yes
cluster-replica-no-failover
no
cluster-slave-no-failover
no
replica-lazy-flush
no
slave-lazy-flush
no
replica-serve-stale-data
yes
slave-serve-stale-data
yes
replica-read-only
yes
slave-read-only
yes
replica-ignore-maxmemory
yes
slave-ignore-maxmemory
yes
jemalloc-bg-thread
yes
activedefrag
no
syslog-enabled
no
cluster-enabled
no

# 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no
appendonly
no
cluster-allow-reads-when-down
no
oom-score-adj
no
aclfile

unixsocket

# 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
# pidfile /var/run/redis.pid
pidfile

replica-announce-ip

slave-announce-ip

masteruser

# 当master服务设置了密码保护时，slav服务连接master的密码
# masterauth <master-password>
masterauth

cluster-announce-ip

syslog-ident
redis

# 指定本地数据库文件名，默认值为dump.rdb
dbfilename
dump.rdb

# 指定更新日志文件名，默认为appendonly.aof
appendfilename
appendonly.aof
server_cpulist

bio_cpulist

aof_rewrite_cpulist

bgsave_cpulist

supervised
no
syslog-facility
local0
repl-diskless-load
disabled

# 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
loglevel
notice
maxmemory-policy
noeviction

# 指定更新日志条件，共有3个可选值：     no：表示等操作系统进行数据缓存同步到磁盘（快）     always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）     everysec：表示每秒同步一次（折衷，默认值）
appendfsync
everysec

# 设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
databases
16

# 指定Redis监听端口，默认端口为6379，作者在自己的一篇博文中解释了为什么选用6379作为默认端口，因为6379在手机按键上MERZ对应的号码，而MERZ取自意大利歌女Alessia Merz的名字
port
6379
io-threads
1
auto-aof-rewrite-percentage
100
cluster-replica-validity-factor
10
cluster-slave-validity-factor
10
list-max-ziplist-size
-2
tcp-keepalive
300
cluster-migration-barrier
1
active-defrag-cycle-min
1
active-defrag-cycle-max
25
active-defrag-threshold-lower
10
active-defrag-threshold-upper
100
lfu-log-factor
10
lfu-decay-time
1
replica-priority
100
slave-priority
100
repl-diskless-sync-delay
5
maxmemory-samples
5

# 当客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能
timeout
0
replica-announce-port
0
slave-announce-port
0
tcp-backlog
511
cluster-announce-bus-port
0
cluster-announce-port
0
repl-timeout
60
repl-ping-replica-period
10
repl-ping-slave-period
10
list-compress-depth
0
rdb-key-save-delay
0
key-load-delay
0
active-expire-effort
1
hz
10
min-replicas-to-write
0
min-slaves-to-write
0
min-replicas-max-lag
10
min-slaves-max-lag
10

# 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息
maxclients
10000
active-defrag-max-scan-fields
1000
slowlog-max-len
128
acllog-max-len
128
lua-time-limit
5000
cluster-node-timeout
15000
slowlog-log-slower-than
10000
latency-monitor-threshold
0
proto-max-bulk-len
536870912
stream-node-max-entries
100
repl-backlog-size
1048576

# 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区
# maxmemory <bytes>
maxmemory
0
hash-max-ziplist-entries
512
set-max-intset-entries
512
zset-max-ziplist-entries
128
active-defrag-ignore-bytes
104857600
hash-max-ziplist-value
64
stream-node-max-bytes
4096
zset-max-ziplist-value
64
hll-sparse-max-bytes
3000
tracking-table-max-keys
1000000
repl-backlog-ttl
3600
auto-aof-rewrite-min-size
67108864
tls-port
0
tls-session-cache-size
20480
tls-session-cache-timeout
300
tls-cluster
no
tls-replication
no
tls-auth-clients
yes
tls-prefer-server-ciphers
no
tls-session-caching
yes
tls-cert-file

tls-key-file

tls-dh-params-file

tls-ca-cert-file

tls-ca-cert-dir

tls-protocols

tls-ciphers

tls-ciphersuites

# 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null
logfile

client-query-buffer-limit
1073741824
watchdog-period
0

# 指定本地数据库存放目录
dir
/data

# 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
# save <seconds> <changes>
# Redis默认配置文件中提供了三个条件：
# save 900 1
# save 300 10
# save 60 10000
save

client-output-buffer-limit
normal 0 0 0 slave 268435456 67108864 60 pubsub 33554432 8388608 60
unixsocketperm
0

# 设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步
# slaveof <masterip> <masterport>
slaveof

notify-keyspace-events

# 绑定的主机地址
# bind 127.0.0.1
bind

# 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH <password>命令提供密码，默认关闭
requirepass
123456
oom-score-adj-values
0 200 800
```



## 4. Redis数据类型

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。



### String（字符串）

string是redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value。

string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象 。

string类型是Redis最基本的数据类型，一个键最大能存储512MB。

#### 实例

```shell
> SET name "shaozhbo"
OK
> GET name
shaozhbo
```

在以上实例中我们使用了 Redis 的 **SET** 和 **GET** 命令。键为 name，对应的值为shaozhbo。

**注意：**一个键最大能存储512MB。



### Hash（哈希）

Redis hash 是一个键值对集合。

Redis hash是一个string类型的field和value的映射表，hash特别适合用于**存储对象**。

#### 实例

```shell
> HMSET user:1 username shaozhbopassword redis.net.cn points 200
ERR wrong number of arguments for 'hmset' command
> HMSET user:1 username shaozhbo password P@ssw0rd points 200
OK
> HGETALL user:1
username
shaozhbo
password
P@ssw0rd
points
200
```

以上实例中 hash 数据类型存储了包含用户脚本信息的用户对象。 实例中我们使用了 Redis **HMSET, HGETALL** 命令，**user:1** 为键值。

每个 hash 可以存储2^(32-1)键值对（40多亿）。



### List（列表）

Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。

#### 实例

```
> LPUSH shaozhbo.list redis
1
> LPUSH shaozhbo.list mongodb
2
> LPUSH shaozhbo.list rabitmq
3
> LRANGE shaozhbo.list 0 10
rabitmq
mongodb
redis
```

列表最多可存储2^(32-1)元素 (4294967295, 每个列表可存储40多亿)。



### Set（集合）

Redis的Set是string类型的无序集合。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

#### sadd 命令

添加一个string元素到,key对应的set集合中，成功返回1,如果元素以及在集合中返回0,key对应的set不存在返回错误。

```
sadd key member
```

#### 实例

```
> sadd shaozhbo.set redis
1
> sadd shaozhbo.set mongodb
1
> sadd shaozhbo.set rabitmq
1
> sadd shaozhbo.set rabitmq
0
> smembers shaozhbo.set
rabitmq
mongodb
redis
```

**注意：**以上实例中 rabitmq 添加了两次，但根据集合内元素的唯一性，第二次插入的元素将被忽略。

集合中最大的成员数为2^(32-1)(4294967295, 每个集合可存储40多亿个成员)。



### zset(sorted set：有序集合)

Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

zset的成员是唯一的,但分数(score)却可以重复。

#### zadd 命令

添加元素到集合，元素在集合中存在则更新对应score

```
zadd key score member 
```

#### 实例

```
> zadd shaozhbo.zset 0 redis
1
> zadd shaozhbo.zset 0 mongodb
1
> zadd shaozhbo.zset 0 rabitmq
1
> zadd shaozhbo.zset 0 rabitmq
0
> zrangebyscore shaozhbo.zset 0 1000
mongodb
rabitmq
redis
```

