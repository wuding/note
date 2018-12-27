PHP 开发指南
===========

# I. 安装 PHP

## 下载
http://windows.php.net/download
http://windows.php.net/downloads/
https://windows.php.net/downloads/releases/archives/php-5.4.45-Win32-VC9-x86.zip

Thread Safe 才有 php7apache2_4.dll
对应版本的 VC 运行库要安装

## 测试
1. 配置 Windows PATH 环境路径，例如 C:\php

2. 命令行查看版本信息

```
php -v
```



## 服务器

### PHP-CGI
start D:/ProgramFiles/nginx-1.13.10/RunHiddenConsole.exe D:/ProgramFiles/nginx-1.13.10/xxfpm.exe "D:/ProgramFiles/php-7.2.3-nts-x64/php-cgi.exe" -n 2 -i 127.0.0.1 -p 9000



# II. 配置

php.ini

## 运行时配置
### 配置文件

1. **搜索路径**
    SAPI 指定位置 PHPIniDir, -c
    PHPRC 环境变量
    注册表 [HKEY_LOCAL_MACHINE\SOFTWARE\PHP\(x(.y(.z)))] IniFilePath
    当前工作目录（对于 CLI）
    ?web 服务器(根)目录（对于 SAPI 模块）
    Windows 目录

2. **名称**
    php-SAPI.ini 会替代 php.ini

3. **可以使用**
    环境变量
    引用已存在的 .ini 变量 open_basedir = ${open_basedir} ":/new/dir"




### .user.ini 文件

  ```ini
  user_ini.filename
  user_ini.cache_ttl 
  ```



### 配置可被设定范围

  PHP_INI_ALL 可在任何地方设定 
  PHP_INI_USER ini_set() 注册表 .user.ini  
  PHP_INI_PERDIR 可在 php.ini httpd.conf  .htaccess (.user.ini ?)
  PHP_INI_SYSTEM 可在 php.ini 或 httpd.conf



### 修改
1. **.htaccess**

  ```sh
  php_value name value
  php_flag name on|off
  ```

2. **httpd.conf**
    不会被 .htaccess 覆盖

  ```
  php_admin_value name value|none
  php_admin_flag name on|off
  ```

3. **注册表**
    HKLM\SOFTWARE\PHP\Per Directory Values\c\inetpub\wwwroot 



## 基本配置项

### 时区
```
[Date]
date.timezone = PRC
```
对应的 php 函数
```php
date_default_timezone_set()
```


### PECL 扩展

扩展目录

```ini
extension_dir = "ext"
```



常用扩展

```ini
extension=curl
```




## 高级配置项

### 开启 OPcache
```
[opcache]
zend_extension="K:\env\win\ProgramFiles(x86)\php-7.3.0RC1-nts\ext\php_opcache.dll"

; Determines if Zend OPCache is enabled
opcache.enable=1

; Determines if Zend OPCache is enabled for the CLI version of PHP
opcache.enable_cli=1
```



### 参考：

[iniscan](https://github.com/psecio/iniscan)

[php-ini-cleanup](https://github.com/perusio/php-ini-cleanup)

[php.ini中文版](https://github.com/SeonWaterLee/php-ini)

[PHP配置文件的中文翻译](https://github.com/HeDefine/PHP.ini-for-Chinese)

[very-secure-php-ini](https://github.com/danehrlich1/very-secure-php-ini)



# III. 语言参考

## 运算符

类型运算符 instanceof

### 其它
42 ** 2 = 42 * 42



## 类与对象

Trait

重载
__set
__get
__isset
__unset

__call
__callStatic

魔术方法
__sleep
__wakeup

__toString
__invoke

__set_state
__debugInfo



## 预定义接口


Closure



# IV. 函数参考

WinCache



# V. 附录

## php.ini 配置

选项列表 

段列表 
[HOST=<host>]
[PATH=<path>] 

(核心配置)选项说明 

最大执行时间、上传文件大小



## 版本差异

PHP5.6 不可以用list作为对象方法，PHP7可以
PHP5.4 不可以 $class->object()::staticfunc(); 要 $obj = $class->object(); $obj::staticfunc(); object()是获取一个对象



# VI. 关键技术

  Queries per second (QPS)
  OOP
  缓存技术、静态化设计
  Web Service



## RESTful

- [php如何发起POST DELETE GET POST 请求](http://www.cnblogs.com/agang-php/p/5210571.html)



## 多线程

- [PHP 真正多线程的使用](http://zyan.cc/pthreads/)



# VII. 参考文档

- [【PHP开发】国外程序员收集整理的 PHP 资源大全](http://www.cnblogs.com/taletao/p/4212916.html)
