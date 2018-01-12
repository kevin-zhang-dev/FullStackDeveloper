# Redis 和 Memcache

> 1. 内存存储器		
> 2. 端口号：6379
>    1. 查看/结束进程：ps -ef | grep redis          /              kill[信号] 进程号       --- 信号： -9  强制杀死
> 3. 启动方式：主程序 + 配置文件	
>
> /working/redis/bin/redis-server /working/redis/conf/redis.conf
>
> 5. deamon 守护进程	
>
> 设置配置文件redis.conf 为 deamonize=yes 使 redis 在后台运行
>
> 6. 启动客户端：/working/redis/bin/redis-cli

| redis             | memcache         |
| ----------------- | ---------------- |
| 可持久化存储            | 不可持久化存储          |
| 丰富的数据类型           | 只有字符串 key=>value |
| 单核运算的             | 多核运算的            |
| 最大 value 存储为 512M | 最大 value 存储为 1M  |
| 写与 memcache 一样    | 读比 redis 快       |

文件查找三剑客：grep sed awk(统计数量)

##### 数据类型：string

添加/获得：set / get

多值添加/获得：mset/mget

删除：del	存在返回 1，不存在返回 0

自动加 1：incr/decr	incrby/decrby(指定添加多少，可以为负数)

memcache 是不可以使用负号的，redis 可以

最大值：2^63 - 1		最小值：- 2^63

> redis 的整形达到最大值，继续加的时候会报错，memcache 是回到 0，继续增加
>
> redis 到最小值的时候会报错，memcache 会一直在 0

##### Hash：redis 存储数据的方式

hset/hget	字段是最后一个覆盖前面的

hmet/hmset	取值的时候把字段也取出来了

hkey：获取到字段

hdel

hlen

##### List 列表

lpush/lpop (可以重复)  从左边弹出/弹入

lrange list 0 -1	-1 表示最后

llen：长度

rpush/rpop 从右边弹出/弹入

##### Set 集合(无序)

sadd set 添加集合(不能重复)

smembers set 查看集合

spop set 2	弹出 2 个

scard 显示长度

sinter setone settwo 交集

sdiff setone settwo 差集

sunion setone settwo 并集

##### SortedSet 集合(有序)

zadd key int key int ...

zrange

zcard

zrevrange sset 0 -1 反序

zincreby sset int key：给指定的值加多少权重

> 有序集合和无序集合，都是唯一值
>
> list 和 hash 可以有重复的值
>
> hash 的字段是不能重复的
>
> ★ redis 推荐使用 ：作为分隔符

##### keys

keys 100*100		* 表示通配

> 实际项目中 key 是有前缀的，可以实现百万级删除
>
> quan:openid 	openid 唯一值，不会重复
>
> /working/redis/bin/redis-cli keys "user*" |xargs /working/redis/bin/redis-cli del

#####  xargs：把管道左边的值，找到一条就传一条给右边。如果没有这个值，是一次性给右边

find	可以查找系统里的文件。

find 目录 -name 【查什么】

exists 检查 key 存在否

expire key int 设置过期时间

ttl key 查看还有多久过期

type 检查类型

select 选择 redis 的哪一个库 	

> 不同的库数据不能相互访问，redis 默认是 16

ping：客户端对服务端进行通信

flushdb：清空当前库

flushall：清空所有库

auth：验证