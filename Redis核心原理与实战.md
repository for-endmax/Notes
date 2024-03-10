# Redis如何执行的

用户输入一条命令

客户端将命令转换成RESP协议（把解析的工作交给客户端），然后再通过socket发送给服务器

服务器收到命令，分析请求，提取参数

执行前准备（权限校验，内存检测）

执行命令

执行完成后进行统计（检查慢查询、记录信息、持久化）

返回结果给客户端

## Redis持久化

类型：快照RDB，文件追加AOF，混合

**RDB：**

将某一个时刻的内存快照（Snapshot），以二进制的方式写入磁盘的过程

触发方式：

- 手动触发
  - save 阻塞主进程
  - bgsave 不阻塞主进程，fork子进程
- 被动触发
  - save m n 在m秒内有n个键发生改变，触发持久化
  - flushall 清除所有快照和数据
  - 主从同步 主节点使用bgsave

配置查询：`config get save`

禁用持久化：`config set save ""`

优点：二进制、重启恢复更快

缺点：只能保存某个时间间隔的数据，fork可能很耗时

**AOF：**

查询AOF是否开启：`config get appendonly`

命令行开启：`config set appendonly yes`

触发方式：

- 自动触发
  - 满足持久化策略：always, everysec,  no
  - 满足AOF重写触发条件
- 手动触发
  - bgrewriteaof

AOF重写：AOF 文件重写是生成一个全新的文件，并把当前数据的最少操作命令保存到新文件上，当把所有的数据都保存至新文件之后，Redis 会交换两个文件，并把最新的持久化操作命令追加到新文件上

重写触发条件：文件容量大小、比例

持久化文件加载规则：开启了AOF便不会使用RDB

**混合持久化：**

在开启混合持久化的情况下，AOF 重写时会把 Redis 的持久化数据，以 RDB 的格式写入到 AOF 文件的开头，之后的数据再以 AOF 的格式化追加的文件的末尾。

查询是否开启：`config get aof-use-rdb-preamble`

## 字符串的使用和原理

用途：页面缓存、数字统计、共享session

单键值操作：

```
set key value
get key
append key value
strlen key
```

多键值操作：

```
mset 
mget
```

数据统计：

```
incr key
decr key
decrby key decrement
incrby key increment
incrbyfloat 
decrbyfloat
```

进阶

```
set key value [expiration EX seconds|PX milliseconds] [NX|XX] 
getset key value
msetnx key value [key value …]
getrange key start end 
```

内部实现：

sds：len 、free、buf[]

`object encoding key`：查看对应键的类型

类型：int, embstr, raw(数据与对象头不在一块)

redis的每个对象都会包含redisObject头：

- type：对象的数据类型，例如：string、list、hash 等，占用 4 bits 也就是半个字符的大小；
- encoding：对象数据编码，占用 4 bits；
- lru：记录对象的 LRU(Least Recently Used 的缩写，即最近最少使用)信息，内存回收时会用到此属性，占用 24 bits(3 字节)；
- refcount：引用计数器，占用 32 bits(4 字节)；
- *ptr：对象指针用于指向具体的内容，占用 64 bits(8 字节)。

