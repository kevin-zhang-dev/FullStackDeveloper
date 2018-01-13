# memcached

##### 源码安装

三步骤：

```
1. ./configure: 检查环境是不是可以安装这个软件，然后再配置这个软件的参数，生成makefile文件。
2. make: 读取 makefile 文件，编译生成二进制文件。
3. make install: 把二进制文件安装到相应的目录文件里面。
```

> 编译安装make 或  make install 出错的时候，使用 make clean 清除，然后解决问题，继续安装

进入图形界面：按 alt+ctrl+F2 登录系统。使用ifconfig查看IP地址。使用putty登录系统。使用 vim /etc/inittab 打开，修改里面的5变为3，下次进入之后，就不会进入图形界面。

rpm -qa | grep httpd		// 查询 Apache 是否安装

rpm -e httpd --nodeps	// 强制卸载 Apache

ps -ef | grep httpd		// 检查Apache是否启动

chkconfig --list | grep iptables		// 查看命令

tar -zxvf 压缩包

> Linux下面递归创建目录的参数是 -p

```
★ Apache安装
./configure --prefix=/working/httpd --enable-so
启动
/working/httpd/bin/apachectl start
★ MySQL安装
1. 移动解压目录到 /usr/local/mysql
2. 初始化程序：/usr/local/mysql/scripts/mysql_install_db
/usr/local/mysql/scripts/mysql_install_db --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data/ --user=mysql
3. 修改属主和属组
chown mysql:root COPYING		// 修改属主和属组
chown mysql:root /usr/local/mysql	// 递归修改 mysql 属主和属组
4. 启动 MySQL
/usr/local/mysql/support-files/mysql.server start
5. 登录mysql服务器
/usr/local/mysql/bin/mysql -u root -p
6. 设置密码
set password=password('root')
★ PHP安装
--with-apxs2=/working/httpd/bin/apxs	// Apache调用PHP必须的参数
--with-mysql=/usr/local/mysql
--enable-mbstring
依赖解决
rpm -ivh /root/libxml2.rpm
devel 是CentOS系列的开发包，必须依赖软件包，名称相同。
```

> /etc/ 下面是程序默认的配置文件的地方

vim /etc/passwd		// 查找用户

##### PHP项目执行流程

##### ![php](C:\Users\KpZhang\Desktop\php.png)

##### 大型项目优化的方向

(1) 代码优化

```php
<?php 

// 有60个ID要通过他们查询信息
$id_arr = array(1,2,3,4,5,6);

// 优化之前
foreach ($id_arr as $id) {
	$sql = "select * from test where id=$id";
}

// 优化之后
// $id_str ="1,2,3,4,5,6"
$id_str  join(',', $id_arr);
$sql = "select * from test where id in ($id_str)"
```

(2) 数据库优化

对数据库配置

更新数据库的时候，你现在的系统IO占用了多少，你需要的更新的数据有多少

(3) 静态化技术

(4) 缓存优化

就是把各种数据，静态化的html数据，或者数据库中的数据，保存到内存里面去。这个时候，就直接访问内存，内存的速度比硬盘快很多倍。

> PHP可以操作内存，使用扩展 shmop

memcache: 内存管理工具

```
页面分析
html:就是静态化的页面。
结果集：缓存我们的结果集数据。
```

memcache特性

```
memcache是内存缓存服务器
memcache是只支持字符串
memcache是key=>value
memcache是不太重要的，没有数据备份
memcache的key最大支持250字节
memcache的value最大支持1M
```

安装

```
安装依赖libevent-devel
./configure --prefix=/usr/local/libevent
./configure --prefix=/working/memcached --with-libevent=/usr/local/libevent
```

> 在memcached里面只有主程序，没有客户端。要使用其他工具当做它的客户端。

启动

```
/working/memcached/bin/memcached -u root -d	// -d表示以守护进程的方式运行，即后台运行
```

Telnet 连接memcache服务器

```
telnet ip地址 11211
```

> 一般使用xShell工具进行连接memcache

指令

```php
1. add：添加数据。有相同的key的时候添加失败，没有key的时候添加成功。
add key 0 0 4
	key: 下标
	0：压缩标识
	0：时间，0表示永不过期
	4：表示要输入的value有4个字节
数据保存在memcache的内存缓存工具里，没有保存到硬盘，电脑关机重启，数据就丢失了。

2. set: 添加数据。有相同key的时候是更新；没有相同key的时候是添加。
set name 0 0 4

3. get: 获取数据。
get key

4. incr: 指定key的值添加多少，所以值一定是数字。自动增加。
incr key num	// 在memcache里面，可以让数字有自动增长
☆ incr 的自动增长，只能是0~2^64-1，当到达最大值的时候，会像顺时针一样，回到起点0，重新开始。
☆ incr 的参数不能是负数
  
5. decr: 指定值减多少
decr age 1	// 指定 age 减少 1
☆ decr 在最小值 0 的时候就不会再减，一直是 0。
  
5. delete: 删除
delete key
  
6. flush_all: 清空memcache上面的所有key		// 在工作中不能使用这个值
  
7. stats: 查看状态
注意两个值：
  hits: 命中
  misses: 未命中
通过这些值的数据量，可以判断你缓存存储的数据是不是有效的。通过未命中与命中的数据量来进行缓存数据的调整。让缓存数据非常的有效。
```

PHP中memcache扩展的对象方法对比

```
1. Memcache::set
bool Memcache::set ( string $key , mixed $var [, int $flag [, int $expire ]] )
key: 是字符串
flag: 是指定压缩只有(MEMCACHE_COMPRESSED)值的时候才会压缩。
expire: 当前写入缓存的数据的失效时间。如果此值设置为0表明此数据永不过期。你可以设置一个UNIX时间戳或 以秒为单位的整数（从当前算起的时间差）来说明此数据的过期时间，但是在后一种设置方式中，不能超过 2592000秒（30天）。
☆ 特别秒的时候，不能超过30天的秒数。超过之后应该使用时间戳。
```

总结：压缩的时候，如果指定对数据进行压缩，压缩是要消耗时间的。取数据也要解压缩，也是会消耗时间的。目前内存已经不贵了，基本不会去压缩数据了。

```php
<?php 

$mem = new Memcache();

$mem->connection('192.168.173.45', 11211);

$ret = $mem->getStats();

var_dump($ret)
```

##### memcache能存储的数据类型

PHP中的八种数据类型

1.布尔型

```php
<?php 

$mem = new Memcache();

$mem->connection('192.168.173.45', 11211);

$true = true;

$mem->set('true', $true, 0, 0);

$ret = $mem->get('true');

var_dump($ret);		// "1"

$false = false;

$mem->set('false', $false, 0, 0);

$ret = $mem->get('false');

var_dump($ret)		// ""
  
//根据分析，确定布尔是隐式转换之后，存储进去的
```

2.字符串

```php
<?php 

$mem = new Memcache();

$mem->connection('192.168.173.45', 11211);

$true = 'true';

$mem->set('true', $true, 0, 0);

$ret = $mem->get('true');

var_dump($ret);		// "true"
```

3.整型

```php
<?php 

$mem = new Memcache();

$mem->connection('192.168.173.45', 11211);

$true = 110;

$mem->set('true', $true, 0, 0);

$ret = $mem->get('true');

var_dump($ret);		// "110"

// 说明：整型存储之后，取出来是字符串的。
```

4.浮点型

```
<?php 

$mem = new Memcache();

$mem->connection('192.168.173.45', 11211);

$true = 110.120;

$mem->set('true', $true, 0, 0);

$ret = $mem->get('true');

var_dump($ret);		// "110.12"

// 说明：标量类型都是隐式转换的。
'abc' => int => 0
'1abc' => int => 1
```

复合类型

1.对象

```php
<?php 

$mem = new Memcache();

$mem->connection('192.168.173.45', 11211);

$true = new obj;

$mem->set('true', $true, 0, 0);

$ret = $mem->get('true');

// 服务器存储：0:1:"A":1:{s:4:"name",s:4:"zkp";}
// 这个就是序列化之后的值
var_dump($ret->getName());		// "zkp"

class obj {
	public $name = 'kzp';

	public function getName()
	{
		return $this->name;
	}
}

总结：这个memcache里面存储的是序列化之后的值。也就是说，memcache扩展会自动帮助我们实现存储的时候序列化，取数据的时候反序列化。
☆ 这个是memcache.dll扩展实现的功能。
```

2.数组

总结：复合类型的数据存储的时候，memcache.dll扩展会帮助我们实现序列化操作。

特殊类型

1.资源型

```php
<?php 

$mem->memcache_connect('192.168.173.45', 11211);

$true = $mem;

$mem->set('true', $true, 0, 0);

$ret = $mem->get('true');

var_dump($ret);		// {["connection"=>int(0)]}

Note:
谨记：资源类型变量（比如文件或连接）不能被存储在缓存中，因为它们在序列化状态不能被完整描述。
```

2.null

```php
<?php 

$mem->memcache_connect('192.168.173.45', 11211);

$true = null;

$mem->set('true', $true, 0, 0);

$ret = $mem->get('true');

var_dump($ret);		// NULL
```

八种类型的说明：

标量类型有四种，都是隐式转换之后存储到memcache里面的。

复合与特殊四种，都是序列化之后存储到memcache里面的。序列化的动作是memcache.dll扩展实现的

##### 分布式memcache服务器

memcache的分布式是通过扩展memcache.dll中的addServer实现的。还可以使用PHP代码实现一个功能，进行分布式，这个需要算法功底。

```
简单的示例：
第一台：0；第二台：1；第三天：2

存储一个key，把这个key都通过hash计算一个整型的值
比如：name的hash的值，假如是112200
112200%3=[0,1,2]
如果是0，就选中第一台存储就可以了
```

##### session存入memcache

```php
<?php 

ini_set('session.save_handler', 'memcache');
ini_set('session.save_path', 'tcp://192.168.173.45:11211');

session_start();

$_SESSION['test'] = "Are you OK?";

echo session_id();		// 用于在数据库取出测试的值
```

session分布式存入memcache

```php
<?php 

ini_set('session.save_handler', 'memcache');
ini_set('session.save_path', 'tcp://192.168.173.45:11211;tcp://192.168.173.45:11212');

session_start();

$_SESSION['test'] = "Are you OK?";

echo session_id();		// 用于在数据库取出测试的值
```

session只能存储到一台服务器上，session存储到memcache没有办法平均分配。

分布式存储数据

```php
<?php 

$memcache = new Memcache;
$memcache->addServer('192.168.173.45', 11211);
$memcache->addServer('192.168.173.45', 11212);

$key = 'test1';
$value = 'test1';
$memcache->set($key, $value, 0, 0);

$key = 'test1';
$value = 'test1';
$memcache->set($key, $value, 0, 0);

$key = 'test1';
$value = 'test1';
$memcache->set($key, $value, 0, 0);
```

##### 分布式存在的问题

第一个问题

在配置了多个memcache服务器的时候，这些配置的顺序成功之后，就不要去更改，因为你去配置的时候，他就按顺序把号码标上了。如果你换了位置，再次去请求的时候，也是按顺序标上的。

如果第一天按顺序把服务器配置成功，这个时候，存储了很多数据。在你第N天之后，改变了顺序，就会出现，hash取余的时候，配置上的服务器对不上号，就会取不到数据。

第二个问题

假设有3台服务器，任意一台宕机了，这个时候就只有2台。addServer只检查出2台。所以这个时候hash取余的值，也对不上号。这个时候就出现了数据乱了。

解决方案：

使用一致性hash，可以解决这个问题。就是使用服务器的唯一标识来进行hash取余。服务器唯一标识有ip加上端口号。

##### memcache中的常见问题

(1) memcache 内存算法(lazy expiration)

懒惰算法：就是在数据存储进memcache的时候，设置过期时间，在获取数据的时候才检查。当获取数据的时候，检查过期，如果过期就返回false，如果没有过期就返回数据。

(2) memcache缓存策略(LRU: Least Recently Used)

最近最少使用原则：在数据存储满的时候，会删除复合规则的数据，把新的数据存储进去，这个功能是默认的。

这个默认的功能是可以关闭的，在启用memcache服务器的时候使用-M就可以了。

但是在工作中基本上没有场景使用这个功能。

(3) memcache分布式算法

就是使用addServer实现，可以使用一致性哈希，也可以自己实现算法。

一致性哈希也是要自己代码实现的。

##### memcache面试题

(1) memcache的cache机制是怎样的？

懒惰算法 +  最近最少使用算法

(2) memcached如何实现冗余机制？

冗余：就是把数据复制多份。

没有必要实现，因为我们的数据是临时的。如果找不到数据了，就应该去mysql去读取。

非要实现的话，可以做一个主备，也可以使用redis

(3) memcached 如何处理容错的？

容错：请求的时候返回错误。

这个时候，可以不用处理。如果非要处理，可以使用主备。

(4) 如何将memcached中item批量导入导出？

说明：memcached是高速的内存缓存，入股实现这个功能，就可能在使用中途产生一定时间断开，这是不允许的，所以，我们应该在存储数据到memcached的时候，可以存储一份到mysql里面。如果要操作，就可以操作mysql里的表数据。

(5) memcached 是如何做身份验证的？

memcached是不能做身份验证的。因为他不能使用账户和密码登录。如果非要做，就可以使用防火墙来实现。

(6) memcached能接受的key的最大长度是多少？

key是250

(7) memcached对item的过期时间有什么限制？

时间戳没有限制，是秒就有30天秒数的限制。

(8) memcached最大能存储多大的单个item?

value能实现1M的大小。

(9) 为什么单个item的大小被限制在1M byte 之内？

因为内存算法的实现。

##### 补充

工作实例：

分布式的代码写法：

```php
<?php

$mem_conf = array(
	array('host' => 'IP', 'port' => 11211),
	array('host' => 'IP', 'port' => 11211),
	array('host' => 'IP', 'port' => 11211),
)

$mem = new memcache();

foreach ($mem_conf as $memcache) {
	$mem->addServer($memcache['host'], $memcache['port']);
}
```

单memcache的时候

```php
$mem = memcache_connect('IP', 11211);
$mem->get('key');
```

业务描述：

现在有一个人，要访问我们的PHP，我们的数据存储在memcache里面，需要这个人的信息更新到公共的信息key里面。也就是当有100个人访问的时候，这个公共信息库应该有这100个人的信息。这个信息都在一个key上面。注意并发访问。

例如：信息key: name, age, ......s

```php
<?php

// 代码：lock 应该去设置过期时间。因为有可能后面的代码没有办法执行，有进程死掉的可能。
while (!$mem->add('lock', 'lock', 0, 160)) {
	// sleep(1); // 秒为单位
	usleep(150); // 毫秒：毫秒的时间，根据下面的代码的执行时间来设置
}

$name = $mem->get('name'); // value = 110,110,120
$age = $mme->get('age'); // value = 12,12,23

// 110,110,120,200
$name .= $username;
$age .= $userage;

$mem->set('name', $name);
$mem->set('age', $age);

$mem->delete('lock'); 
```

