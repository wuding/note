Cygwin 使用指南
==============
https://github.com/wuding/note/blob/master/Cygwin.md



# 序

以前也似乎看到过 Cygwin 这个词，没在意。
这次是因为安装 [vbot](https://github.com/HanSon/vbot) 需要安装 Swoole 但是只能 Linux
然后就搜到了 Windows 使用 Cygwin 安装。



### 关于 Cygwin

https://zh.wikipedia.org/wiki/Cygwin
Cygnus Solutions 开发

https://fanyi.baidu.com/#en/zh/Cygnus
天鹅座



# 安装 Cygwin

https://www.cygwin.com/setup-x86_64.exe

Windows XP 和 Server 2003 可以使用 Cygwin [2.5.2](https://cygwin.com/ml/cygwin/2016-06/msg00328.html)

1. Install from Internet

2. Root Directory `D:\ProgramFiles\Cygwin64`

3. Local Package Directory `D:\Program_Data\Cygwin`

4. Use System Proxy Settings

5. User URL: Add http://mirrors.163.com/cygwin/

    via [在 Windows 下搭建 swoole 环境与测试](https://juejin.im/post/5b7136586fb9a009b230813c)

6. Select Packages, View Full, Search:

   gcc, gcc-g++, php, php-devel, autoconf, pcre-devel, wget, unzip

还要选择 php 扩展 fileinfo, gd, simplexml 因为 vbot 需要，Composer 要 json，常见的也一并选上，phar 等等……

**参考**：

- [Cygwin下安装Linux PHP环境和Swoole扩展并在PHPStorm中调试](https://my.oschina.net/yanpengquan/blog/658205)
- https://cygwin.com/mirrors.html
- https://cygwin.com/packages/



### 安装 X Window 桌面环境

- [Cygwin/X User's Guide](https://x.cygwin.com/docs/ug/cygwin-x-ug.html)

- [Cygwin，以及远程登陆Linux桌面](http://www.cppblog.com/len/archive/2008/07/03/55234.html)

- [使用cygwin X server实现Linux远程桌面 (for windows)](https://blog.easwy.com/archives/linux-remote-desktop-via-cygwin-x-server/)



# 安装 Swoole

点击打开 `Cygwin64 Terminal`

```shell
cd /home
# 下载
wget https://github.com/swoole/swoole-src/archive/v4.1.2.zip
# 解压
unzip swoole-src-4.1.2.zip

cd /home/swoole-src-4.1.2
# 输入命令
phpize
# 等待一会后执行：
./configure && make && make install
# 配置、编译、编译安装 swoole
```

会在 2 处生成 dll

- /home/swoole-src-4.1.2/modules/swoole.dll
- /lib/php/20160303/swoole.dll



### 配置 php.ini

使用命令: `php -i | grep php.ini`，找到 php cli 使用的配置文件 php.ini 的路径

编辑

> /etc/php.ini

```ini
extension=swoole.dll
```

查看是否安装成功
```shell
php -m
```



# 安装 PEAR

[安装pecl和pear命令](https://www.jianshu.com/p/4db600c12160)

```sh
wget http://pear.php.net/go-pear.phar
php go-pear.phar
```



**安装好后要编辑 Cygwin 全局环境变量**

> /etc/profile

```shell
PATH="/usr/local/bin:/usr/bin:/home/Benny/pear/bin"
```



**检查配置是否正确**

```shell
pear config-show
```

部分错误可以直接修改

```shell
pear config-set php_bin /usr/bin/php
```
配置文件在 `/home/Benny/.pearrc`
可以参照 `/home/Benny/pear/share/pear/PEAR/Config.php` 对应的配置变量名称修改



**配置 PEAR 环境变量**

> Cygwin.bat
```shell
set PHP_PEAR_PHP_BIN=/usr/bin/php
set PHP_PEAR_INSTALL_DIR=/home/Benny/pear/share/pear
set PHP_PEAR_BIN_DIR=/usr/bin/php
set PHP_PEAR_SYSCONF_DIR=/etc
```
但是只有点击 bat 有效



修改 Cygwin 默认终端 mintty 环境变量

> /home/Benny/.bash_profile
```sh
PHP_PEAR_PHP_BIN=/usr/bin/php
PHP_PEAR_INSTALL_DIR=/home/Benny/pear/share/pear
PHP_PEAR_BIN_DIR=/usr/bin/php
PHP_PEAR_SYSCONF_DIR=/etc
```


最后你可能还要修改 /home/Benny/pear/bin/ 目录下文件中的变量 `$PHP` `$INCDIR` `$INCARG`



**参考**：

- [cygwin默认shell配置文件](https://www.cnblogs.com/xlmeng1988/archive/2013/01/08/2851542.html)



# 安装 PHP 扩展 Redis

`pecl install redis` 好几次都不行，即使安装了 [igbinary](https://github.com/igbinary/igbinary)
虽然生成了 redis.dll 但是 php 报错
最后还是手动下载源代码 https://github.com/phpredis/phpredis/archive/develop.zip [安装](https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown)成功

**参考**：

- [Windows（ Cygwin） 下搭建 Swoole + Redis +Posix 环境](http://www.weicot.com/windows%EF%BC%88-cygwin%EF%BC%89-%E4%B8%8B%E6%90%AD%E5%BB%BA-swoole-redis-posix-%E7%8E%AF%E5%A2%83/)



# 安装 apt-cyg

安装 Cygwin 时选择包：lynx, openSSH
还有 apt-cyg 的依赖软件：wget, tar, gawk, bzip2 这些可能之前都已安装好了

使用 https://github.com/transcode-open/apt-cyg 官方安装方法：
```sh
lynx -source https://raw.githubusercontent.com/transcode-open/apt-cyg/master/apt-cyg > apt-cyg
install apt-cyg /bin
```
只是需要修改源文件链接，其实下载 apt-cyg 这个脚本文件放到 /bin 即可

**参考**：
- [win10 安装Cygwin apt-cyg](https://www.jianshu.com/p/f6624db59114)
- [Windows下安装Cygwin及apt-cyg](https://www.jianshu.com/p/fac45920628d)



# 安装 Apache

```sh
apt-cyg install cygrunsrv
# 这个包好像没了，有 cygrunsrv 应该就可以了
apt-cyg install cygserver

apt-cyg install httpd
apt-cyg install httpd-mod_php7
```



**测试 Apache**

```sh
/usr/sbin/apachectl restart
```



**配置 Apache**

> /etc/httpd/conf/httpd.conf
```ini
# 改 ip 和端口，避免冲突
Listen 127.0.0.1:8888

# 不设置的话可能报错
ServerName localhost:8888

# 启用 mpm 配置
Include conf/extra/httpd-mpm.conf
```



**设置启动的进程数**
httpd 的 mod_php 必须使用 prefork 模式的 MPM（Multi-Processing Modules），默认应该已经配置好了，可以通过 `/usr/sbin/apachectl -t -D DUMP_MODULES | grep mpm` 来确认下。

默认情况下会开启很多 httpd 进程，可以参照下面配置减少

> /etc/httpd/conf/extra/httpd-mpm.conf
```ini
# 修改数目
<IfModule mpm_prefork_module>
    StartServers             1
    MinSpareServers          1
    MaxSpareServers          2
    MaxRequestWorkers        3
    MaxConnectionsPerChild   0
</IfModule>
```



**安装服务**

```sh
/etc/rc.d/init.d/httpd install
```

可能报错，如下
> Installing httpd daemon: cygrunsrv: Error installing a service: OpenSCManager: Win32 error 5: access denied.

“以管理员身份运行“ Cygwin 即可

安装成功 Windows 服务 services.msc 会出现 CYGWIN Apache HTTP Server



**启动服务**

```sh
cygrunsrv -S httpd
```
又可能报错，如下
> cygrunsrv: Error starting a service: QueryServiceStatus:  Win32 error 1062:

删除 /var/run/httpd/httpd.pid 重试



**测试 PHP**

> /srv/www/htdocs/phpinfo.php
```php
<?php
phpinfo();
```



**可能需要修改配置**

> /etc/httpd/conf.d/php7.conf
```ini
# 其实就是模块命名不同
#<IfModule mod_php7.c>
<IfModule php7_module>
```

> /etc/httpd/conf.modules.d/mod_php7.conf
```ini
#<IfModule prefork.c>
<IfModule mpm_prefork_module>
```



**参考**：

- [在Cygwin上安装MySQL，Apache和PHP](https://zh4ui.net/post/2017-04-23-cygwin-mysql-apache-php/)



# 安装 sshd 服务

**参考**：

- [cygwin openssh for windows](http://blog.51cto.com/irow10/1869471) Cygwin + OpenSSH FOR Windows的安装配置
- [安装 cygwin sshd 服务遇到的问题](https://www.cnblogs.com/tiga/archive/2012/11/16/2772643.html)
- [通过cygwin安装openSSH](http://xpenxpen.iteye.com/blog/2061856)
- [SecureCRT登录本地cygwin。](http://blog.51cto.com/hellonetwork/1243304)



# 安装 MySQL

```sh
# 软件包
apt-cyg install mysql
apt-cyg install mysql-server
```



**安装数据库**

```sh
mysql_install_db
# 时间比较长
```



**设置密码**

```sh
/usr/bin/mysql_secure_installation
```


可能报错
> Can't connect to local MySQL server through socket '/var/run/mysql.sock' (2 "No such file or directory")

解决方式
```sh
# 查找映像
ps aux | grep mysqld

# 设置 socket
mysql --socket=/var/run/mysql.sock
```



**配置 MySQL**

> /etc/my.cnf.d/server.cnf

```ini
[mysqld]
port=3308
socket=/var/run/mysql.sock
datadir=/var/lib/mysql
#log-error=error.log
```

> /etc/my.cnf.d/client.cnf

```ini
[client]
port=3308
#socket=/var/run/mysql.sock
```



**启动 MySQL 服务**

```sh
/bin/mysqld_safe &
```



**关闭 MySQL 服务**

```sh
mysqladmin -u root -p shutdown
# 输入前面设置的密码
```



**安装为 Windows 服务**

```sh
cygrunsrv --install mysqld --disp 'CYGWIN MySQL Server' --path /usr/bin/mysqld_safe --args '--datadir=/var/lib/mysql --pid-file=/var/lib/mysql/mysql.pid'
```



**使用 mklink 做目录链接**

```sh
D:\ProgramFiles\cygwin64\var\lib>mklink /D mysql K:\tmp\mysql
```



**参考**：

- [mysqld_safe — MySQL Server Startup Script](https://dev.mysql.com/doc/refman/5.6/en/mysqld-safe.html)
- [cygwin下](http://www.360doc.com/content/17/1014/19/48413195_694937443.shtml) [mysql安装](https://hyhsystem.cn/wordpress/?page_id=178)
- [Can’t connect MySQL server through socket的解决方法](https://my.oschina.net/lcdmusic/blog/638900)
- [Ubuntu下出现Mysql error(2002)的解决方法](https://blog.csdn.net/leisure512/article/details/5139104)



# 使用 NetBeans

**参考**：

- [bash on win10 代替cygwin开发应用](https://zhuanlan.zhihu.com/p/22620823)

