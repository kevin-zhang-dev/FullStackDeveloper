# Linux

| 命令                          | 解释     |
| --------------------------- | ------ |
| touch bootstrap/helpers.php | 创建一个文件 |
|                             |        |
|                             |        |

> Linux 的 `touch` touch命令有两个功能：一是用于把已存在文件的时间标签更新为系统当前的时间（默认方式），它们的数据将原封不动地保留下来；二是用来创建新的空文件。

## redis

##### 1、什么是Redis

Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。

redis与memcache一样的，也是一个内存存储器。redis是可以持久化存储的。redis与memcache的并发访问量，都是特别大的。

测试上限都是过W的。以秒来计算的。但是这个具体都接收多少并发，是根据项目的复杂度来进行的。

说明：redis与memcache基本上都不会成为我们网站的瓶颈，这些瓶颈出现的最多的地方是IO，这个IO就是昨天说的，一次性读取硬盘数据的量。

还有一个就是网络带宽的。100W的带宽最大传输的数据量是100 / 8 。

测试的时候，很多时候，你感觉测试的效果不理想，你就要思考带宽。

##### 2、Redis与Memcache的区别

|                | redis | memcache |
| :------------- | :---: | :------: |
| **可持久化**       |   是   |    否     |
| **数据类型**       |   多   | 只有string |
|                | 单核运算  |   多核运算   |
| **最大value存储值** | 512M  |    1M    |
| **读/写**        | 快/一样  |   慢/一样   |



## 3、安装Redis

```
1）make 生成二进制文件
2）make PREFIX=/dir  install    安装程序，并指定默认目录
3）配置文件（redis.conf）：从解压目录复制到安装目录   cp redis.conf  /working/redis/conf/redis.conf
4）查看配置文件，看看数据是保存到哪里的？  dir /working/redis/data
4）修改配置文件：daemonize no → daemonize yes
```

redis-server：是主服务器程序

redis-cli：是redis的专用客户端

##### 启动方式：主程序 + 配置文件

/working/redis3.2.6/bin/redis-server/working/redis3.2.6/conf/redis.conf

客户端启动：/working/bin/redis-cli

## 4、Redis快速入门

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image054.jpg)

退出登录：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image056.jpg)

 

关闭我们的redis？

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image058.jpg)

 

kill [信号] 进程号

默认可以不写信号：不写就是说的是会通知进程，让进程把自己关闭。

信号：-9就是强制杀死。是系统直接操作。不等主进程通知worker进程。

默认信号是-15

 

-9杀死主进程之后：留下来的是什么进程？

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image060.jpg)

僵死进程！！

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image062.jpg)

启动一下：redis：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image064.jpg)

 

# 二、Redis支持的数据类型

## 1、String（字符串）

set ：添加一个值！

set key value

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image066.jpg)

添加值，存在就修改

get ：获取这个值

get key

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image068.jpg)

 

mset ：多值添加

mset key value key value ....

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image070.jpg)

 

mget ：多值获得

mget key key key ....

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image072.jpg)

 

del ：删除

del key ....

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image074.jpg)

删除存在的返回1，删除不存在的？

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image076.jpg)

 

incr ：指定的key自动添加1

incr key  ：这个key的值会自动的加1

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image078.jpg)

想一想：可以负号！

 

incrby ：这个也是指定添加的意思，但是要指定添加多少。

这个就是和memcache的incr 和increment一样的。

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image080.jpg)

使用负号！

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image082.jpg)

重点：memcache是不可以使用负号的，redis是可以的。

 

memcache可以使用的整型是多大？ 2^64无符号的值范围

redis可以使用的整型是多大？2^64有符号的值范围

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image083.png)

这个值前加面一个负号就是最小值。最大值就是这个值减1.

 

最大值测试：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image085.jpg)

说明：到redis的整型到达最大值，继续加的时候，会报错！！

memcache是回到0，继续增加！！

 

decr ：自动减1

decr key

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image087.jpg)

 

decrby ：指定减去多少。

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image089.jpg)

可以使用负数吗？

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image091.jpg)

负负得正！！

 

继续：测试最小值！

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image093.jpg)

结果：

redis到最小值的时候，会报错！！

memcache是一直在0，不会报错！！

 

## 2、Hash（哈希表）：redis存储数据的方式

hset ：设置值

hset key filed value

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image095.jpg)

 

hget ：获得这个值

hget key filed

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image097.jpg)

 

hmset ：多个值的设置

hmset key field value field value field value ....

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image099.jpg)

字段是最后一个覆盖前面的！！

 

hmget ：获得多个值

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image101.jpg)

 

hgetall ：获得key里面的所有值

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image103.jpg)

获得取值的同时把字段也获得出来了

 

key是随便你写的：user , user_100 , user:100,

 

hkeys 

hkeys key ：获取到字段

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image105.jpg)

 

hlen ：

hlen key  :获得key的长度

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image107.jpg)

 

hdel ：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image109.jpg)

 

## 3、List（列表）

双向链表：随便出！又吐又拉

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image111.jpg)

 

 

队列：先进先出！吃多了拉

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image113.jpg)

 

 

[堆]栈：先进后出！吃多了吐

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image115.jpg)

 

lpush ：从左边添加进数据

lpush key value value

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image117.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image119.jpg)

可以重复！！

 

lrange ：查看验证，结果！

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image121.jpg)

0 ：就是索引从0开始的

-1:表示最后

 

验证：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image123.jpg)

这里2就是索引的位置！！

 

lpop ：弹出：从左边弹出

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image125.jpg)

 

只需要lpush与lpop配合，就实现了堆栈

 

llen ：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image127.jpg)

长度的验证！

 

rpush ：右边的插入数据

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image129.jpg)

右边是从下面插入的，左边是从上面插入的。

 

rpop ：弹出：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image131.jpg)

从底部弹出：

 

使用lpush 与 rpop 实现了队列 或者是rpush 与 lpop 就实现了队列

 

## 4、Set（集合）无序的

sadd ：添加集合

sadd key value value value ...

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image133.jpg)

存在的value，可以重新吗？

 

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image135.jpg)

集合是不能有重复的值！

 

smembers ：查看

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image137.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image139.jpg)

这是个无序的。就是不要相信他的顺序！！

 

spop ：随机弹

spop set ：弹一个出来    ； spop set 2 ：弹出二个

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image141.jpg)

 

scard ：

scard key :显示长度

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image143.jpg)

 

sinter ：交集

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image145.jpg)

 

sdiff ：差集

redis的差集是由第一个集合决定的。只显示第一个的。

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image147.jpg)

 

sunion ：并集

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image149.jpg)

 

## 5、SortedSet（有序集合）

zadd ：有序集合

zadd key int value int value

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image151.jpg)

数字就是有序集合拿来排序的

通过数字的大小来进行排序

 

zcard :显示长度

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image153.jpg)

 

zrange ：显示范围

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image155.jpg)

默认是正序的。

 

 

zrevrange ：这个就是反序的

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image157.jpg)

 

排序的数字？

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image159.jpg)

 

正序的数字？

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image161.jpg)

 

zincrby ：给指定的值加多少分

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image163.jpg)

 

总结：

有序集合与无序集合，都是唯一值。不能有重复的值。

list与hash可以有重复的值。

hash的字段是不能重复的。

 

redis非常重要的，就是这5个数据类型！！

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image165.jpg)

面试的时候，只要能回答上这五个问题，就可以了。！！

问你是不是只有这五个，回答不是的。其它有了解但没有使用。就记得不清楚。

 

key：user:100

告诉大家，redis推荐大家使用:作为分割符。如果你的代码使用:作为分割符，看上去就专业了。

 

## 6、Key（键）

keys ：获得所以的key

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image167.jpg)

通配：*

 

通配左边：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image169.jpg)

通配右边：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image171.jpg)

 

这个命令超级重要！！

因为我们在实际工作中，要做数据测试的时候，上百W万的数据，都会一次使用完。使用完之后，这些数据都不能使用了。必需删除这些数据，然后才能继续生成百W数据，再进行测试。测试并不会一次性就没完成，要反复测试很多次的。

 

这个命令可以把不用的key找出来，并且删除掉。实际工作中我们的项目的key应该是有前缀的，比如我们是quan:openid

这个时候要把这些quan:openid全部找出来，就要使用keys quan:* 。找出来了。

 

使用客户端进行操作：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image173.jpg)

使用客户端执行一次，keys命令。但是这种方式必需是使用引号把*引起来。

 

确定这个命令的执行方式是在linux上面的执行。。

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image175.jpg)

 

使用这二个来进行配合：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image177.jpg)

 

说明：这种方式非常的重要！！

xargs：这个是在linux下面使用的，是linux命令。在面试的时候，经常问的。

它的作用就是把管道左边的值，找到一条就传一条到右边。

如果没有这个值，管道左边的值，是查找完成之后，一次性给右边的。

 

如果你要查的数据特别的大。如果找到之后一次性给右边，内存就可以不够用。所以我们要使用xargs命令，把数据进行一条一条的传，这样的话，数据的使用量就占用的空间特别少了。

 

这个命令的说明：

先说find文件查找命令：

这个命令可以查找系统里面的文件，与windows的查找功能是一样。

 

find 目录  [什么样的方式去查]   [查什么]

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image179.jpg)

一定可以使用通配符，必需有引号！！

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image181.jpg)

find的查找：

 

以xargs示例！！

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image183.jpg)

 

在linux文本查找，文件的内容查找。

有三剑客之称的是：grep   sed  awk 

如果你说你经常使用Linux绝对会问awk

 

exists ：检查key存在否

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image185.jpg)

 

expire ：设置过期时间

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image187.jpg)

5秒之后在获得name

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image188.png)

5秒之后就没有了

 

ttl ：查看还有多久过期

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image190.jpg)

 

type ：检查类型

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image192.jpg)

 

select ：库，选择redis的那一个库

查看配置文件：我们默认是有16个库

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image194.jpg)

默认是0库，说明库的索引是从0开始。

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image196.jpg)

说明，不同的库，数据不能相互访问到的。

 

库都是单独存在的。和mysql的库是一样的。

 

ping ：对客户端对服务端进行通信，看看服务端还在线吗？

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image198.jpg)

ping的时候发送消息。就会返回这些消息

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image200.jpg)

以上二种情况，就是通的。其它情况，都是不行的。

 

flushdb ：清空当前库

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image202.jpg)

 

flushall ：清空所以的库。

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image204.jpg)

 

memcache和redis是一样的，都有清空。但是这些命令都不要去操作。想想在测试机上面，肯定有很多人的数据，所以你要删除的时候，只删除自己的。

 

服务器和个人电脑不一样。服务器基本上不重启。没有问题一直在线的。不会关机的。

linux很稳定，可以做到10年不重启。windows试试。。

 

auth ：验证

我们登录redis的时候，需要密码。

就必需要修改配置文件，把密码配置成功。

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image205.png)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image207.jpg)

 

通过管道找到redis：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image209.jpg)

关闭redis

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image211.jpg)

启动redis:

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image213.jpg)

启动成功，使用客户端进行登录：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image215.jpg)

登录没有权限，请使用密码：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image217.jpg)

 

# 三、持久化功能

## 1、snap shotting快照持久化

打开配置文件：查看持久化配置：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image219.jpg)

save 900   1    ：900就是时间，1就是key。是900秒之后有一个key改变就保存一次

save 300  10 :：是300秒之后有10个key改变就保存一次

save 60  10000 ：是60秒之后有1w个key改变就保存一次。

 

这些持久化的方案，可以根据项目的实际情况来调整，如果项目没有特殊要求，可以不用调整。这些配置都是大牛，优化的特别好。考虑了很多问题的。

 

## 2、append only file （AOF持久化，精细持久化）

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image221.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image223.jpg)

保存的策略：

always ：总是保存，每一个key改变都要保存，保存的时候就特别的慢

everysec：就是第一秒保存一次，这个相对上面的一个就快很多。

no ：就是随系统的OI来保存的，随机的。不确定。不推荐。

 

这二种方案进行对比：

首先快照持久化是把数据压缩之后保存的。

AOF是把我们的操作的命令保存下来的。

 

默认设置是快照持久化是每1秒有10000个改变的时候，才保存一次。

AOF默认是每一秒保存一次。

这个时候如果宕机数据会有一点遗失，怎么选择方案，根据你数据的重要性来决定 。

 

## 3、测试测

快照持久化：

修改配置文件：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image225.jpg)

确定文件的时间：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image227.jpg)

重启redis:

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image229.jpg)

操作客户端：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image231.jpg)

 

设置6个值：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image233.jpg)

等待2分钟，看看有没有持久化到硬盘：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image235.jpg)

这个就是我们的内容！！已经成功的持久化的硬盘。

与memcache的区别。是redis因为持久化数据到了硬盘，所以重启redis的时候，数据依然是存在的。

 

AOF精细持久化：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image237.jpg)

保存退出就可以了

数据就是保存在配置的数据目录里面的

 

重启redis：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image239.jpg)

客户端登录：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image241.jpg)

查看存储的文件：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image243.jpg)

设置一个值之后，就存储成功了：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image245.jpg)

 

通过二个的测试，都配置成功了。

注意，在实际使用的时候，选择一种方案就可以了。

 

# 四、redis的主从模式

## 1、Redis Replication的特点和优势

1）同一个Master可以同步多个Slaves。

2）Slave同样可以接受其它Slaves的连接和同步请求，这样可以有效的分载Master的同步压力。因此我们可以将Redis的Replication架构视为图结构。

3）Master Server是以非阻塞的方式为Slaves提供服务。所以在Master-Slave同步期间，客户端仍然可以提交查询或修改请求。

4）Slave Server同样是以非阻塞的方式完成数据同步。在同步期间，如果有客户端提交查询请求，Redis则返回同步之前的数据。

5）为了分载Master的读操作压力，Slave服务器可以为客户端提供只读操作的服务，写服务仍然必须由Master来完成。即便如此，系统的伸缩性还是得到了很大的提高。

6）Master可以将数据保存操作交给Slaves完成，从而避免了在Master中要有独立的进程来完成此操作。

 

## 2、Redis Replication的工作原理

在Slave启动并连接到Master之后，它将主动发送一个SYNC命令。此后Master将启动后台存盘进程，同时收集所有接收到的用于修改数据集的命令，在后台进程执行完毕后，Master将传送整个数据库文件到Slave，以完成一次完全同步。而Slave服务器在接收到数据库文件数据之后将其存盘并加载到内存中。此后，Master继续将所有已经收集到的修改命令，和新的修改命令依次传送给Slaves，Slave将在本次执行这些数据修改命令，从而达到最终的数据同步。

如果Master和Slave之间的链接出现断连现象，Slave可以自动重连Master，但是在连接成功之后，一次完全同步将被自动执行。

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image247.jpg)

 

## 3、如何配置Replication

1）主服务器不用更改

2）从服务器进行配置

3）找到从服务器，在同一台电脑上启动二个进程。

①：数据文件保存在不同的目录里面

②：端口号是不能一样的

③：PID文件的名称不能一样

 

开始准备：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image249.jpg)

从服务器的配置文件，所在目录

把配置文件复制到目录里面：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image251.jpg)

 

准备一个数据目录：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image253.jpg)

 

修改配置文件：打开的是从服务器的配置文件

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image255.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image257.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image259.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image261.jpg)

配置成功！

 

启动一个从服务器：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image263.jpg)

登录从服务器：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image265.jpg)

 

配置主从服务器：

打开从服务器的配置文件：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image267.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image269.jpg)

保存退出，重新重启从服务器：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image271.jpg)

从服务器被干掉了

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image273.jpg)

恭喜启动成功！！

 

客户端登录：登录从服务器！

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image275.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image277.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image279.jpg)

同步成功！！！

 

验证从服务器是否是可读的？

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image281.jpg)

总结：验证这个过程已经OK。

 

在主服务器写入数据：

主服务写入：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image283.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image285.jpg)

 

## 4、应用案例

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image287.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image289.jpg)

 

主从的实现就完成。。

PHP代码的实现主从，就从下面的开始了。

 

# 五、PHP操作Redis（基本操作）

## 1、安装PHP的Redis扩展

把PHP变成一个redis的客户端，必需安装redis的扩展，才可以让php连接到redis的服务器

 

1）下载好扩展：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image291.jpg)

2）解压出来需要的：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image293.jpg)

3）把这个复制到php对应版本的扩展目录里面去：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image295.jpg)

4）在PHP.INI配置文件里面，配置上：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image297.jpg)

5）使用Phpinfo来验证：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image299.jpg)

就可以正常使用redis了。

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image301.jpg)

phpinfo会爆露很多重要信息出来，所以被外网访问到的页面，都不应该有这些信息的输出。如果你要测试，显示之后，立即删除。

 

但是是一定要确定删除干净了。

 

## 2、使用PHP操作Redis

去官网看看：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image303.jpg)

掉眼泪，没有说明，怎么办？

 

百度找到方式：

​         我也百度了找到了一个善良的人：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image305.jpg)

开源精神！！！

 

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image307.jpg)

连接报错：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image309.jpg)

连接不成功：因为上午的时候，我们的redis绑定了IP地址，只能是127.0.0.1才能访问到。修改这里：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image311.jpg)

主服务器的

 

从服务器的也修改：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image313.jpg)

 

把主从服务器一起重启：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image315.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image317.jpg)

启动成功就OK了：

 

访问一下我们的网页：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image319.jpg)

代码的读写分离：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image321.jpg)

 

setnx ：这个和memcache里面的add效果是一样的。

memcache里面的add做了加锁的功能，所以这个值也是可以做加锁的功能。

 

 

# 六、补充

边界问题！

 

高并发的环境：抢购，或者秒杀！！

 

秒杀：现在是17点到17:30开始秒杀。用户可以秒杀多少次，确定是每个用户一次。

 

分析：确定时间，在这个时间才能开始。

时间到了就开始吗？自己去定义。

​         我这里定义时间到了会有一个key存在，时间没有到这个key是不存在。

​         这个key可以由后台页面，产品去点一个开关去实现。

​         用户半个小时只能抢到一个，怎么去定义这个东西？

​         把用户的openid（唯一ID）存储到redis里面，抢购成功的。判断redis里面有这个用户，就返回已经抢到了。

 

​         集合（无序）：哪一个集合可以去判断，这个集合里面存在什么值！

确定使用无序集合来判断！！

 

 

补充一个邮件系统！！！

使用第三方写好的代码发送。因为这个发送邮件要调用底层代码，相当的难。所以我们写不出来，使用大神的。。

 

PHPMAILER:

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image323.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image325.jpg)下载成功：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image327.jpg)

学习怎么使用它？

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image329.jpg)

通过这个示例就可以实现发送邮件！！

 

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image331.jpg)

这些参数在你的邮件系统里面找。如果公司自己有邮件服务器，就是有的。没有的话，使用第三方的。

以qq为例 :

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image333.jpg)

开启这二个就可以了：

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image335.jpg)

![img](file:///C:/Users/KpZhang/AppData/Local/Temp/msohtmlclip1/01/clip_image337.jpg)

 

邮件发送就成功了。。

 

业务需求：

现在有订阅用户很多人。要给这些订阅用户发送信息，使用邮件发送。

就是有新的文章产生的时候，就可以在后台，点击发送，就会给这些订阅用户发送邮件。

 

订阅是在前台，是用户的主动行动。当用户点击之后，就应该存储到mysql里面。需要存储到redis里面。

也可以不是立即存储到redis里面，是每天晚上跑一个脚本，把当天订阅所有用户存储到redis里面去。

 

后台:

后台管理人员，选择是不是发送。如果发送才会解发邮件系统。

 

所以我们这里采用手工跑脚本，把用户信息存储到redis里面。

用户的什么信息存储到redis里面？

用户的openid 与邮箱，就可以发送邮件了。

 

开始跑脚本！就有一个PHP文件就可以了。我这里就不写了，直接在cli上面操作。

发送给用户的是经过邮箱，需要名称（openid）

redis里面使用什么方式存储：

字符串，hash还是集合？

黄鹤选择最优的是集合！

杨超选择最优的是hash！

张明哲选择最优的是集合！！

张三峰选择最优的是字符串！

 

$key = “openid-邮箱: openid-邮箱: openid-邮箱: openid-邮箱”;

hash：

$key = array(‘openid =>邮箱, ‘openid =>邮箱, ‘openid =>邮箱)

集合：

$key = array(openid-邮箱, openid-邮箱, openid-邮箱);

有序集合：

$key = array(int =>邮箱, ‘int =>邮箱, ‘int =>邮箱);

分析出来，选择hash是最优的。