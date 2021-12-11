## Redis教程

**REmote DIctionary Server(Redis)** 是一个由Salvatore Sanfilippo写的**key-value存储系统。**

Redis是一个开源的使用ANSI C语言编写、遵守**BSD协议**、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并**提供多种语言的API**。

它通常被称为**数据结构服务器**，因为值（value）可以是 字符串(String), 哈希(Map), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。



## Redis 简介

- Redis支持**数据的持久化**，可以将内存中的数据保持在磁盘中，**重启的时候可以再次加载**进行使用。
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- Redis支持**数据的备份，即master-slave模式**的数据备份。

Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。并且提供对他们的**原子性操作**

Redis运行在内存中但是可以**持久化到磁盘**，所以在对不同数据集进行**高速读写时需要权衡内存**，应为数据量不能大于硬件内存。

## redis 命令

src/redis-cli

1. $ redis-cli -h host -p port -a password:  
2. $redis-cli -h 127.0.0.1 -p 6379 -a "773344"
3. 设置密码：CONFIG get requirepass， CONFIG set requirepass "runoob"



## Redis 键(key)

1. redis 127.0.0.1:6379> COMMAND KEY_NAME

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [DEL key](https://www.redis.net.cn/order/3528.html) 该命令用于在 key 存在是删除 key。 |
| 2    | [DUMP key](https://www.redis.net.cn/order/3529.html) 序列化给定 key ，并返回被序列化的值。 |
| 3    | [EXISTS key](https://www.redis.net.cn/order/3530.html) 检查给定 key 是否存在。 |
| 4    | [EXPIRE key](https://www.redis.net.cn/order/3531.html) seconds 为给定 key 设置过期时间。 |
| 5    | [EXPIREAT key timestamp](https://www.redis.net.cn/order/3532.html) EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。 |
| 6    | [PEXPIRE key milliseconds](https://www.redis.net.cn/order/3533.html) 设置 key 的过期时间亿以毫秒计。 |
| 7    | [PEXPIREAT key milliseconds-timestamp](https://www.redis.net.cn/order/3534.html) 设置 key 过期时间的时间戳(unix timestamp) 以毫秒计 |
| 8    | [KEYS pattern](https://www.redis.net.cn/order/3535.html) 查找所有符合给定模式( pattern)的 key 。 |
| 9    | [MOVE key db](https://www.redis.net.cn/order/3536.html) 将当前数据库的 key 移动到给定的数据库 db 当中。 |
| 10   | [PERSIST key](https://www.redis.net.cn/order/3537.html) 移除 key 的过期时间，key 将持久保持。 |
| 11   | [PTTL key](https://www.redis.net.cn/order/3538.html) 以毫秒为单位返回 key 的剩余的过期时间。 |
| 12   | [TTL key](https://www.redis.net.cn/order/3539.html) 以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。 |
| 13   | [RANDOMKEY](https://www.redis.net.cn/order/3540.html) 从当前数据库中随机返回一个 key 。 |
| 14   | [RENAME key newkey](https://www.redis.net.cn/order/3541.html) 修改 key 的名称 |
| 15   | [RENAMENX key newkey](https://www.redis.net.cn/order/3542.html) 仅当 newkey 不存在时，将 key 改名为 newkey 。 |
| 16   | [TYPE key](https://www.redis.net.cn/order/3543.html) 返回 key 所储存的值的类型。 |



## Redis 字符串(String)

1. redis 127.0.0.1:6379> SET w3ckey redis 

2. OK 

3. redis 127.0.0.1:6379> GET w3ckey 

4. "redis"

   | 序号 | 命令及描述                                                   |
   | :--- | :----------------------------------------------------------- |
   | 1    | [SET key value](https://www.redis.net.cn/order/3544.html) 设置指定 key 的值 |
   | 2    | [GET key](https://www.redis.net.cn/order/3545.html) 获取指定 key 的值。 |
   | 3    | [GETRANGE key start end](https://www.redis.net.cn/order/3546.html) 返回 key 中字符串值的子字符 |
   | 4    | [GETSET key value](https://www.redis.net.cn/order/3547.html) 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。 |
   | 5    | [GETBIT key offset](https://www.redis.net.cn/order/3548.html) 对 key 所储存的字符串值，获取指定偏移量上的位(bit)。 |
   | 6    | [MGET key1 [key2..\]](https://www.redis.net.cn/order/3549.html) 获取所有(一个或多个)给定 key 的值。 |
   | 7    | [SETBIT key offset value](https://www.redis.net.cn/order/3550.html) 对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。 |
   | 8    | [SETEX key seconds value](https://www.redis.net.cn/order/3551.html) 将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。 |
   | 9    | [SETNX key value](https://www.redis.net.cn/order/3552.html) 只有在 key 不存在时设置 key 的值。 |
   | 10   | [SETRANGE key offset value](https://www.redis.net.cn/order/3553.html) 用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。 |
   | 11   | [STRLEN key](https://www.redis.net.cn/order/3554.html) 返回 key 所储存的字符串值的长度。 |
   | 12   | [MSET key value [key value ...\]](https://www.redis.net.cn/order/3555.html) 同时设置一个或多个 key-value 对。 |
   | 13   | [MSETNX key value [key value ...\]](https://www.redis.net.cn/order/3556.html) 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。 |
   | 14   | [PSETEX key milliseconds value](https://www.redis.net.cn/order/3557.html) 这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。 |
   | 15   | [INCR key](https://www.redis.net.cn/order/3558.html) 将 key 中储存的数字值增一。 |
   | 16   | [INCRBY key increment](https://www.redis.net.cn/order/3559.html) 将 key 所储存的值加上给定的增量值（increment） 。 |
   | 17   | [INCRBYFLOAT key increment](https://www.redis.net.cn/order/3560.html) 将 key 所储存的值加上给定的浮点增量值（increment） 。 |
   | 18   | [DECR key](https://www.redis.net.cn/order/3561.html) 将 key 中储存的数字值减一。 |
   | 19   | [DECRBY key decrement](https://www.redis.net.cn/order/3562.html) key 所储存的值减去给定的减量值（decrement） 。 |
   | 20   | [APPEND key value](https://www.redis.net.cn/order/3563.html) 如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾。 |



# Redis 哈希(Hash)

Redis hash 是一个string类型的field和value的映射表，hash特别适合用于存储对象。

Redis 中每个 hash 可以存储 232 - 1 键值对（40多亿）。

1. redis 127.0.0.1:6379> HMSET w3ckey name "redis tutorial" description "redis basic commands for caching" likes 20 visitors 23000
2. OK
3. redis 127.0.0.1:6379> HGETALL w3ckey
4.  
5. \1) "name"
6. \2) "redis tutorial"
7. \3) "description"
8. \4) "redis basic commands for caching"
9. \5) "likes"
10. \6) "20"
11. \7) "visitors"
12. \8) "23000"

**在以上实例中，我们设置了 redis 的一些描述信息(name, description, likes, visitors) 到哈希表的 w3ckey 中。**

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [HDEL key field2 [field2\]](https://www.redis.net.cn/order/3564.html) 删除一个或多个哈希表字段 |
| 2    | [HEXISTS key field](https://www.redis.net.cn/order/3565.html) 查看哈希表 key 中，指定的字段是否存在。 |
| 3    | [HGET key field](https://www.redis.net.cn/order/3566.html) 获取存储在哈希表中指定字段的值/td> |
| 4    | [HGETALL key](https://www.redis.net.cn/order/3567.html) 获取在哈希表中指定 key 的所有字段和值 |
| 5    | [HINCRBY key field increment](https://www.redis.net.cn/order/3568.html) 为哈希表 key 中的指定字段的整数值加上增量 increment 。 |
| 6    | [HINCRBYFLOAT key field increment](https://www.redis.net.cn/order/3569.html) 为哈希表 key 中的指定字段的浮点数值加上增量 increment 。 |
| 7    | [HKEYS key](https://www.redis.net.cn/order/3570.html) 获取所有哈希表中的字段 |
| 8    | [HLEN key](https://www.redis.net.cn/order/3571.html) 获取哈希表中字段的数量 |
| 9    | [HMGET key field1 [field2\]](https://www.redis.net.cn/order/3572.html) 获取所有给定字段的值 |
| 10   | [HMSET key field1 value1 [field2 value2 \]](https://www.redis.net.cn/order/3573.html) 同时将多个 field-value (域-值)对设置到哈希表 key 中。 |
| 11   | [HSET key field value](https://www.redis.net.cn/order/3574.html) 将哈希表 key 中的字段 field 的值设为 value 。 |
| 12   | [HSETNX key field value](https://www.redis.net.cn/order/3575.html) 只有在字段 field 不存在时，设置哈希表字段的值。 |
| 13   | [HVALS key](https://www.redis.net.cn/order/3576.html) 获取哈希表中所有值 |
| 14   | HSCAN key cursor [MATCH pattern] [COUNT count] 迭代哈希表中的键值对。 |

# Redis 列表(List)

Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）

一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

1. redis 127.0.0.1:6379> LPUSH w3ckey redis
2. (integer) 1
3. redis 127.0.0.1:6379> LPUSH w3ckey mongodb
4. (integer) 2
5. redis 127.0.0.1:6379> LPUSH w3ckey mysql
6. (integer) 3
7. redis 127.0.0.1:6379> LRANGE w3ckey 0 10
8.  
9. \1) "mysql"
10. \2) "mongodb"
11. \3) "redis"

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [BLPOP key1 [key2 \] timeout](https://www.redis.net.cn/order/3577.html) 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 2    | [BRPOP key1 [key2 \] timeout](https://www.redis.net.cn/order/3578.html) 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 3    | [BRPOPLPUSH source destination timeout](https://www.redis.net.cn/order/3579.html) 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 4    | [LINDEX key index](https://www.redis.net.cn/order/3580.html) 通过索引获取列表中的元素 |
| 5    | [LINSERT key BEFORE\|AFTER pivot value](https://www.redis.net.cn/order/3581.html) 在列表的元素前或者后插入元素 |
| 6    | [LLEN key](https://www.redis.net.cn/order/3582.html) 获取列表长度 |
| 7    | [LPOP key](https://www.redis.net.cn/order/3583.html) 移出并获取列表的第一个元素 |
| 8    | [LPUSH key value1 [value2\]](https://www.redis.net.cn/order/3584.html) 将一个或多个值插入到列表头部 |
| 9    | [LPUSHX key value](https://www.redis.net.cn/order/3585.html) 将一个或多个值插入到已存在的列表头部 |
| 10   | [LRANGE key start stop](https://www.redis.net.cn/order/3586.html) 获取列表指定范围内的元素 |
| 11   | [LREM key count value](https://www.redis.net.cn/order/3587.html) 移除列表元素 |
| 12   | [LSET key index value](https://www.redis.net.cn/order/3588.html) 通过索引设置列表元素的值 |
| 13   | [LTRIM key start stop](https://www.redis.net.cn/order/3589.html) 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。 |
| 14   | [RPOP key](https://www.redis.net.cn/order/3590.html) 移除并获取列表最后一个元素 |
| 15   | [RPOPLPUSH source destination](https://www.redis.net.cn/order/3591.html) 移除列表的最后一个元素，并将该元素添加到另一个列表并返回 |
| 16   | [RPUSH key value1 [value2\]](https://www.redis.net.cn/order/3592.html) 在列表中添加一个或多个值 |
| 17   | [RPUSHX key value](https://www.redis.net.cn/order/3593.html) 为已存在的列表添加值 |

1. redis-benchmark -h 127.0.0.1 -p 6379 -t set,lpush -n 100000 -q 无法执行

![image-20211126204757448](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211126204757448.png)

![image-20211126212545776](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211126212545776.png)

![image-20211126212559464](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211126212559464.png)





