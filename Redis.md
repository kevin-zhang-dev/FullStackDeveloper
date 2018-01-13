# Redis

redis和memcache是一样的，也是一个内存存储器。redis是可以持久化存储的。redis与memcache的并发访问量，都是特别大的。测试上限都是过W的，以秒来计算的。但是具体都接受多少并发，是根据项目的复杂度来进行的。

说明：redis和memcache基本上都不会成为我们网站的瓶颈，这些瓶颈出现的最多的地方是IO，这个IO就是一次性读取硬盘数据的量。

还有一个就是网络带宽。100W的带宽最大传输的数据量是100/8。

测试的时候，感觉测试的效果不理想，就要考虑带宽。

##### Redis与Memcache的区别

```
redis是可以持久化的，memcache是不能持久化的。
redis是丰富的数据类型，memcache是只有字符串k=>v
redis是单核运算的，memcache是多核运算的。
redis最大的value存储是512M，memcache最大的存储是1M。
redis在写与memcache的时候是一样的，memcache的读比redis快。		
```

##### 安装

```
make
make PREFIX=/working/redis install
复制解压后中的配置文件到redis文件目录中 /working/redis/conf
修改配置文件中数据存放的位置 dir /working/redis/data
```

端口号：6379

> 1. 启动方式：主程序 + 配置文件
>
> /working/redis/bin/redis-server /working/redis/conf/redis.conf
>
> 2. deamon 守护进程	
>
> 设置配置文件redis.conf 为 deamonize=yes 使 redis 在后台运行
>
> 3. 启动客户端：/working/redis/bin/redis-cli

kill [信号] 进程号

默认可以不写信号：不写就是会通知进程，让进程把自己关闭

信号: -9 就是强制杀死。是系统直接操作。不等master进程通知worker进程，默认信号时-15

##### 数据类型：string

1.set：添加一个值。存在就修改

```
set key value添加/获得：set / get
```

2.get: 获取这个值

```
get key
```

3.mset：多值添加

```
mset key value key value ...
```

4.mget: 多值获得

```
mget key key key ...
```

5.del: 删除

```
del key ... 		// 	存在返回 1，不存在返回 0
```

6.incr：指定的key自动添加1

```
incr key		// 这个key的值会自动的加1
```

7.incrby：这个也是添加的意思，但是要指定添加多少。这个就是和memcache的incr和increment一样

```
incrby age -10 
重点：memcache是不可以使用负号的，redis是可以的
```

memcache可以使用的整型是多大？ 2^64 无符号的值的范围

redis 可以使用的整型是多大？ 2^64 有符号的值的范围

> 说明：redis的整型到达最大值，继续加的时候，会报错。memcache是回到0，继续增加。

8.decr：自动减1

```
decr key
```

9.decrby: 指定减去多少

```
decrby key -10
```

> redis 到最小值的时候会报错，memcache 会一直在 0，不会报错

##### Hash：redis 存储数据的方式

1.hset：设置值

```
hset key field value		// hset user:100 name zkp
```

2.hget: 获取这个值

```
hget key field		// hget user:100 name
```

3.hmset: 多个值的设置

```
hmset user:100 name zkp age 18 sex 1	// 字段是最后一个覆盖前面的
```

4.hmget: 获取多个值

```
hmget user:100 name age sex
```

5.hgetall: 获取key里面的所有值

```
hgetall user:100		// 取值的同时把字段也取出来了
```

6.hkeys：获取到所有字段

```
hkeys user:100
```

7.hlen: 获取到字段的长度(个数)

```
hlen user:100
```

8.hlen：删除指定字段，可以多个

```
hdel user:100 sex age
```

##### List 列表

1.lpush: 从左边添加进数据

```
lpush key value value		// 值可以重复
```

2.lrange：查看结果

```
lrange list 0 -1	
0: 表示索引从0开始
-1：表示最后
```

 3.lpop 从左边弹出

```
lpop list
```

> 只需要 lpush 和 lpop 配合，就实现了堆栈。

4.llen：长度的验证

```
llen list
```

5.rpush: 右边插入数据

```
rpush list zhao qian sun
```

6.rpop 从右边弹出

```
lpush list
```

```
使用 lpush 与 rpop 实现了队列，或者是 rpush 与 lpop 实现队列
```

##### Set 集合(无序)

1.sadd：添加集合

```
sadd key value value value ...		// 集合是不能有重复的值
```

2.smembers：查看集合

```
smembers key
```

3.spop: 弹出(随机)

```
spop set	默认弹出1个	 	
spop set 2	弹出2个
```

4.scard: 显示长度

```
scard key
```

5.sinter：交集

```
sinter setone settwo
```

6.sdiff：差集

```
sdiff setone settwo		// redis的差集是由第一个集合决定的，只显示第一个的
```

7.sunion: 并集

```
sunion setone settwo
```

##### SortedSet 集合(有序)

1.zadd: 有序集合

```
zadd key int value int value...		// 数字就是用来排序的
```

2.zcard：显示长度

```
zcard sset
```

3.zrange: 显示范围(正序)

```
zrange sset 0 -1
```

4.zrevrange: 显示范围(反序)

```
zrevrange sset 0 -1 withscores		// 显示权重
```

5.zincreby：给指定的值加多少权重

```
zincrby key int value
```

> 有序集合和无序集合，都是唯一值。不能有重复的值。
>
> list 和 hash 可以有重复的值。
>
> hash 的字段是不能重复的
>
> ★ redis 推荐使用 ：作为分隔符

##### Key(键)

keys: 获取所有的key

```
keys *		// * 表示通配
keys *100	// 通配左边
keys 100*	// 通配右边
```

使用客户端进行操作

```
// 使用客户端执行一次keys命令，但是这种方式必须使用引号把*引起来
/working/redis/bin/redis-cli keys "*"
// 确定这个命令的执行方式是在Linux上面执行的。
```

> 实际项目中 key 是有前缀的，可以实现百万级删除
>
> quan:openid 	openid 唯一值，不会重复
>
> /working/redis/bin/redis-cli keys "user*" |xargs /working/redis/bin/redis-cli del

#####  xargs：把管道左边的值，找到一条就传一条给右边。如果没有这个值，是一次性给右边

find	可以查找系统里的文件。

find 目录【查询条件】【查什么】

```
find ./ -name "*.gz"		// 通配符必须使用引号括起来
find ./ -name "*log" |xargs ls -l	// 查找并查看详细信息
```

文件的内容查找三剑客：grep sed awk(统计IP数量)

exists：检查 key 存在否

```
exists list
```

expire: 设置过期时间

```
expire name 5	// 5秒之后过期
```

ttl：查看还有多久过期

```
ttl key
```

type: 检查类型

```
type key
```

select: 选择 redis 的哪一个库 	

> 不同的库数据不能相互访问，redis 默认是 16个库。默认是 0 库，说明库的索引是从 0 开始。

ping：客户端对服务端进行通信，看看服务端还在线吗？

```
ping [ok]		// ping 的时候发送消息，就会返回这些消息，服务器健康检查时使用
```

flushdb：清空当前库

```
flushdb
```

flushall：清空所有库

```
flushall
```

> memcache 和 redis 是一样的，都有清空。但是这些命令都不要去操作，如果要删除，只删除自己的

auth：验证

我们登录redis的时候，需要密码。就必须修改配置文件，把密码配置成功。

redis.conf 中修改 requirepass password

```
auth password	// 登录redis
```

### 持久化功能

##### 1. snap shotting 快照持久化

```
save 900 1: 900就是时间，1就是key。是900秒之后有一个key改变就保存一次。
save 300 10: 是300秒之后有10个key改变就保存一次。
save 60 10000: 是60秒之后有1W个key改变就保存一次。
```

##### 2. append only file(AOF 持久化，精细持久化)

```
always：总是保存，每一个key改变都要保存，保存的时候就特别的慢。
everysec: 就是每一秒保存一次，这个相对上面的一个就快很多。
no: 就是随系统的OI来保存的，随机的。不确定。不推荐。
```

两种方案进行对比：

首先快照持久化是把数据压缩之后保存的。

AOF 是把我们的操作的命令保存下来的。

默认设置的是快照持久化是每1秒有10000个key改变的时候，才保存一次。

这个时候如果宕机，数据就会有一点遗失，怎么选择方法，根据你数据的重要性来选择。

> 与memcache的区别。是redis因为持久化数据到硬盘，所以重启redis的时候，数据依然是存在的。

### redis的主从模式

##### 如何配置Replication

(1) 主服务其不用更改

(2) 从服务器进行配置

(3) 找到从服务器，在同一台电脑上启动两个进程

​	★ 数据文件保存在不同的目录里面

​	★ 端口号是不能一样的

​	★ PID 文件的名称是不能一样

##### PHP操作redis实现读写分离

```php
<?php  

// redis的主从，一定是从PHP层面来进行操作
// 通过代码来进行判断，使用哪个实例化的对象

// 实例化写的对象
$redis = new redis();
$redis->connect('192.168.173.97', 6379);
$redis->auth('password');

// 实例化读的对象
$sredis = new redis();
$sredis->connect('192.168.173.97', 6380);
$sredis->auth('password');

$str = 'hello world';

if ($redis->set('hi', $str)) {
	$ret = $sredis->get('hi');
	var_dump($ret);
} else {
	echo "哭一下！";
}
```

setnx: 这个和memcache里面的add效果是一样的。

memcache里面的add做了加锁的功能，所以这个值也是可以做加锁的功能。

### 补充

边界问题！

1.高并发的环境：抢购，或者秒杀！

```
秒杀：现在是17点到17:30开始秒杀。用户可以秒杀多少次，确定是每个用户一次。

分析：确定时间，在这个时间才能开始。
时间到了就开始吗？自己去定义。
我这里定义时间到了会有一个key存在，时间没到这个key是不存在。
这个key可以由后台页面，产品去点一个开关去实现。
用户半个小时只能抢到一个，怎么去定义这个东西？
把用户的openid(唯一ID)存储到redis里面，抢购成功的。判断redis里面有这个用户，就返回已经抢到了。

集合(无序)：哪一个集合可以去判断。这个集合里面存在什么值！确定使用无序集合来判断。
```

2.邮件系统

使用第三方写好的代码发送。因为这个发送邮件要调用底层代码，相当的难，我们写不出来。

