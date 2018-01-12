# memcache

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

