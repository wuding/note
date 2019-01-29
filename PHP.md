PHP 开发指南
===========



## I. 安装 PHP

### 下载
http://windows.php.net/download
http://windows.php.net/downloads/
https://windows.php.net/downloads/releases/archives/php-5.4.45-Win32-VC9-x86.zip

Thread Safe 才有 php7apache2_4.dll
对应版本的 VC 运行库要安装



[Experimental: PHP 5.6 and Apache 2.4.10 VC11 for 2K3/XP](https://www.apachelounge.com/viewtopic.php?t=6334)



### 环境需求

#### VC15

https://visualstudio.microsoft.com/downloads/

Other Tools and Frameworks > Microsoft Visual C++ Redistributable for Visual Studio 2017



### 测试
1. 配置 Windows PATH 环境路径，例如 C:\php

2. 命令行查看版本信息

```
php -v
```



### 命令行

```sh
C:\Users\Administrator>php -h
Usage: php [options] [-f] <file> [--] [args...]
   php [options] -r <code> [--] [args...]
   php [options] [-B <begin_code>] -R <code> [-E <end_code>] [--] [args...]
   php [options] [-B <begin_code>] -F <file> [-E <end_code>] [--] [args...]
   php [options] -S <addr>:<port> [-t docroot] [router]
   php [options] -- [args...]
   php [options] -a

  -a               Run as interactive shell
  -c <path>|<file> Look for php.ini file in this directory
  -n               No configuration (ini) files will be used
  -d foo[=bar]     Define INI entry foo with value 'bar'
  -e               Generate extended information for debugger/profiler
  -f <file>        Parse and execute <file>.
  -h               This help
  -i               PHP information
  -l               Syntax check only (lint)
  -m               Show compiled in modules
  -r <code>        Run PHP <code> without using script tags <?..?>
  -B <begin_code>  Run PHP <begin_code> before processing input lines
  -R <code>        Run PHP <code> for every input line
  -F <file>        Parse and execute <file> for every input line
  -E <end_code>    Run PHP <end_code> after processing all input lines
  -H               Hide any passed arguments from external tools.
  -S <addr>:<port> Run with built-in web server.
  -t <docroot>     Specify document root <docroot> for built-in web server.
  -s               Output HTML syntax highlighted source.
  -v               Version number
  -w               Output source with stripped comments and whitespace.
  -z <file>        Load Zend extension <file>.

  args...          Arguments passed to script. Use -- args when first argument
                   starts with - or script is read from stdin

  --ini            Show configuration file names

  --rf <name>      Show information about function <name>.
  --rc <name>      Show information about class <name>.
  --re <name>      Show information about extension <name>.
  --rz <name>      Show information about Zend extension <name>.
  --ri <name>      Show configuration for extension <name>.
```





### 服务器

#### PHP-CGI
start D:/ProgramFiles/nginx-1.13.10/RunHiddenConsole.exe D:/ProgramFiles/nginx-1.13.10/xxfpm.exe "D:/ProgramFiles/php-7.2.3-nts-x64/php-cgi.exe" -n 2 -i 127.0.0.1 -p 9000



## II. 配置

php.ini



##### 参考：

[iniscan](https://github.com/psecio/iniscan)

[php-ini-cleanup](https://github.com/perusio/php-ini-cleanup)

[php.ini中文版](https://github.com/SeonWaterLee/php-ini)

[PHP配置文件的中文翻译](https://github.com/HeDefine/PHP.ini-for-Chinese)

[very-secure-php-ini](https://github.com/danehrlich1/very-secure-php-ini)



### 运行时配置

#### 配置文件

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




#### .user.ini 文件

  ```ini
  user_ini.filename
  user_ini.cache_ttl 
  ```



#### 配置可被设定范围

  PHP_INI_ALL 可在任何地方设定 
  PHP_INI_USER ini_set() 注册表 .user.ini  
  PHP_INI_PERDIR 可在 php.ini httpd.conf  .htaccess (.user.ini ?)
  PHP_INI_SYSTEM 可在 php.ini 或 httpd.conf



#### 修改
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



### 基本配置项

#### 时区
```
[Date]
date.timezone = PRC
```
对应的 php 函数
```php
date_default_timezone_set()
```


#### PECL 扩展

扩展目录

```ini
extension_dir = "ext"
```



常用扩展

```ini
; 协议
extension=curl
extension=php_openssl.dll

; 文本和翻译
extension=php_gettext.dll
extension=php_intl.dll
extension=php_mbstring.dll

; 数据库
extension=php_mysqli.dll
extension=php_pdo_mysql.dll
```




### 高级配置项

#### 开启 OPcache
```
[opcache]
zend_extension="K:\env\win\ProgramFiles(x86)\php-7.3.0RC1-nts\ext\php_opcache.dll"

; Determines if Zend OPCache is enabled
opcache.enable=1

; Determines if Zend OPCache is enabled for the CLI version of PHP
opcache.enable_cli=1
```



### 常用配置

#### Paths and Directories

```ini
include_path = ".;c:\php\includes"
sys_temp_dir = "D:/env/tmp/sys"
enable_dl = Off
```



#### File Uploads

```ini
file_uploads = On
upload_tmp_dir = "D:/env/tmp/upload"
upload_max_filesize = 20M
max_file_uploads = 20
```



#### 日期

```ini
[Date]
date.timezone = PRC
```



#### 会话

```ini
[Session]
session.save_handler = files
session.save_path = "N;MODE;/path"
session.name = PHPSESSID
session.use_trans_sid = 0
session.use_cookies = 1
session.use_only_cookies = 1
; 值设置为 0 可以在客户端禁用 cookie 时用 sid
```

##### 参考：

[禁用cookie后session是如何设置的](http://blog.sina.com.cn/s/blog_4d6c4525010173rf.html)







## III. 语言参考

### 运算符

类型运算符 instanceof

[细说PHP7中的NULL合并运算符](https://www.yduba.com/biancheng-7332345516.html)

#### 其它
```
42 ** 2 = 42 * 42
```



### 类与对象

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



### 预定义接口


Closure



## IV. 函数参考

WinCache



### 函数功能补充

#### 数组

- [x] 清除数组的数字键名项
- [x] 比较数组的键值
- [x] 合并 JSON 格式数组，以文本存储

#### 编解码
- \u 开头的

[PHP解码unicode编码](https://blog.csdn.net/gongqinglin/article/details/80062695)

- DOMDocument::loadHTML 默认编码是 ISO-8859-1
```PHP
// 转为实体
$dom->loadHTML(mb_convert_encoding($profile, 'HTML-ENTITIES', 'UTF-8')); 
```
```html
<!-- 需要声明 -->
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
```
[PHP DOMDocument loadHTML出现乱码的解决方法](https://towait.com/blog/php-domdocument-loadhtml-not-encoding-utf-8-correctly/)

[DOMDocument->loadHTML()处理中文的一点问题](http://www.fwolf.com/blog/post/314)

## V. 附录

### php.ini 配置

选项列表 

段列表 
[HOST=<host>]
[PATH=<path>] 

(核心配置)选项说明 

最大执行时间、上传文件大小



### 版本差异

PHP5.6 不可以用list作为对象方法，PHP7可以
PHP5.4 不可以 $class->object()::staticfunc(); 要 $obj = $class->object(); $obj::staticfunc(); object()是获取一个对象



### 单元测试



[设计模式](https://github.com/OMGZui/DesignPattern)



### PEAR

- https://gerardnico.com/lang/php/pear
- https://pear.horde.org/



### [PHPDoc](http://docs.phpdoc.org/references/phpdoc/index.html)

https://github.com/php-fig/fig-standards/blob/master/proposed/phpdoc-tags.md



## VI. 关键技术

  Queries per second (QPS)
  OOP
  缓存技术、静态化设计
  Web Service



### RESTful

- [php如何发起POST DELETE GET POST 请求](http://www.cnblogs.com/agang-php/p/5210571.html)



### 多线程

- [PHP 真正多线程的使用](http://zyan.cc/pthreads/)



## VII. 参考文档

- [【PHP开发】国外程序员收集整理的 PHP 资源大全](http://www.cnblogs.com/taletao/p/4212916.html)
- [现代 PHP 新特性系列](https://laravelacademy.org/post/4221.html)



## VIII. 编辑器

https://en.wikipedia.org/wiki/List_of_PHP_editors



## IX. 框架

[自定义框架之路由](https://mengkang.net/1316.html)
