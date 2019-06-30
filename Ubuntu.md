# Ubuntu

### 目录

[安装 Ubuntu](#安装-ubuntu)

[安装 Apache](#安装 Apache)

[安装 PHP](#安装 PHP)

[安装 MySQL](#安装 MySQL)

[安装 Webmin](#安装 Webmin)



## 安装 Ubuntu

### WSL

微软应用商店搜索并安装 ubuntu

文件系统根目录

C:\Users\86176\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs



## 安装常用工具

```sh
sudo apt install unzip
sudo apt install unrar
```

### 参考

- [Ubuntu 下 unzip用法](https://www.cnblogs.com/qunshu/p/3150090.html)
- [ubuntu下安装nginx时依赖库zlib，pcre，openssl安装方法](https://blog.csdn.net/z920954494/article/details/52132125)



## 安装 Apache

更新下软件源

```sh
sudo apt-get update
```

安装

```sh
sudo apt-get install apache2
```

网站根目录 /var/www/html

访问 127.0.0.1

重启

```sh
sudo /etc/init.d/apache2 restart
```



### 常见问题

#### Failed to enable APR_TCP_DEFER_ACCEPT

`sudo vi` 打开 `/etc/apache2/apache2.conf`，在文件的最底部加上一行以下内容：

```
AcceptFilter http none
```



### 参考

- [图解Ubuntu下Apache WEB服务器的安装](https://jingyan.baidu.com/article/8275fc865b85c046a13cf673.html)



## 安装 PHP

```sh
sudo apt-get install php7.0
```

建议中看到有 7.2，取消安装  7.0

```sh
sudo apt-get install php7.2
php -v
```

### 安装 Apache 模块

先搜索

```sh
apt-cache search libapache2-mod-php 
```

选择安装一个版本

```sh
sudo apt-get install libapache2-mod-php7.2
```

### 编写一个测试文件

```sh
cd /var/www/html
sudo vim test.php
# sudo gedit test.php
```

test.php

```php
<?php
phpinfo();
```

### 安装扩展

```sh
sudo apt-get install php7.0-mysql
sudo apt-get install php7.0-mbstring
```

### 安装 Composer

```sh
sudo apt-get install composer
composer
```

### 参考

- [ubuntu搭建php开发环境记录](https://www.cnblogs.com/impy/p/8040684.html)
- [如何在Ubuntu系统中安装PHP](https://jingyan.baidu.com/article/4b52d702d6d877fc5d774b44.html)



## 安装 MySQL

```sh
sudo apt-get install mysql-server mysql-client
mysql -v
```

### 参考

- [如何在Ubuntu中安装MySQL数据库软件](https://jingyan.baidu.com/article/29697b9155c683ab21de3c55.html)



## 安装 Webmin

[在Ubuntu中安装功能最强大的管理工具Webmin](https://jingyan.baidu.com/article/a24b33cd35fe1a19ff002b74.html)



## 常见问题

### 文件和目录权限

```sh
ls -l
# 或 ll

sudo chmod 777 folder
# 修改
```

#### 参考

- [Ubuntu修改文件权限](https://blog.csdn.net/slwhy/article/details/78876237)



### dpkg was interrupted

```sh
sudo dpkg --configure -a
sudo apt-get update
sudo apt-get upgrade
# sudo apt-get -f install
```

- [ubuntu遇到了 dpkg was interrupted, you must manually run 'dpkg..的问题](https://blog.csdn.net/gudujianjsk/article/details/7893156)
