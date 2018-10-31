Apache HTTP Server (httpd) 快速入门
================================

http://blog.csdn.net/lsyz0021/article/details/51998848

# 安装 Apache

## 下载

官方下载页
http://httpd.apache.org/download.cgi

### 下载最新版
选择其中一个发行版
http://httpd.apache.org/docs/current/platform/windows.html#down

例如
https://www.apachelounge.com/download/

压缩包文件地址
http://home.apache.org/~steffenal/VC15/binaries/httpd-2.4.34-win64-VC15.zip



### 下载旧版 2.2

这个版本支持 Windows XP/2003
http://archive.apache.org/dist/httpd/binaries/win32/
https://www.apachelounge.com/download/win32/

选择是否包含 SSL
http://archive.apache.org/dist/httpd/binaries/win32/httpd-2.2.25-win32-x86-no_ssl.msi
http://archive.apache.org/dist/httpd/binaries/win32/httpd-2.2.25-win32-x86-openssl-0.9.8y.msi

这个稍微新一点
https://www.apachelounge.com/download/win32/binaries/httpd-2.2.34-win32.zip

需要 VC++ 2010
https://www.microsoft.com/zh-CN/download/details.aspx?id=8328



## 安装

官方文档
http://httpd.apache.org/docs/2.4/platform/windows.html

安装为服务前请先配置
```sh
httpd.exe -k install
```

测试运行

```sh
bin> httpd.exe -w -n "Apache2" -k start
```

命令行帮助
```sh
L:\Users\Benny>d:
D:\>cd D:\ProgramFiles\Apache24\bin
D:\ProgramFiles\Apache24\bin>httpd ?
Usage: httpd [-D name] [-d directory] [-f file]
             [-C "directive"] [-c "directive"]
             [-w] [-k start|restart|stop|shutdown] [-n service_name]
             [-k install|config|uninstall] [-n service_name]
             [-v] [-V] [-h] [-l] [-L] [-t] [-T] [-S] [-X]
Options:
  -D name            : define a name for use in <IfDefine name> directives
  -d directory       : specify an alternate initial ServerRoot
  -f file            : specify an alternate ServerConfigFile
  -C "directive"     : process directive before reading config files
  -c "directive"     : process directive after reading config files
  -n name            : set service name and use its ServerConfigFile and ServerR
oot
  -k start           : tell Apache to start
  -k restart         : tell running Apache to do a graceful restart
  -k stop|shutdown   : tell running Apache to shutdown
  -k install         : install an Apache service
  -k config          : change startup Options of an Apache service
  -k uninstall       : uninstall an Apache service
  -w                 : hold open the console window on error
  -e level           : show startup errors of level (see LogLevel)
  -E file            : log startup errors to file
  -v                 : show version number
  -V                 : show compile settings
  -h                 : list available command line options (this page)
  -l                 : list compiled in modules
  -L                 : list available configuration directives
  -t -D DUMP_VHOSTS  : show parsed vhost settings
  -t -D DUMP_RUN_CFG : show parsed run settings
  -S                 : a synonym for -t -D DUMP_VHOSTS -D DUMP_RUN_CFG
  -t -D DUMP_MODULES : show all loaded modules
  -M                 : a synonym for -t -D DUMP_MODULES
  -t -D DUMP_INCLUDES: show all included configuration files
  -t                 : run syntax check for config files
  -T                 : start without DocumentRoot(s) check
  -X                 : debug mode (only one worker, do not detach)

D:\ProgramFiles\Apache24\bin>
```



### 模块

**mod_fcgid**
https://www.apachelounge.com/download/
https://www.apachelounge.com/download/VC15/modules/mod_fcgid-2.3.9-win64-VC15.zip


## 配置
httpd.conf

### 目录设置
所有目录必须存在，否则无法启动，也没有错误日志
```sh
# 软件所在目录
ServerRoot "c:/Apache24"

# 网站根目录
DocumentRoot "c:/Apache24/htdocs/html"

# 目录选项配置
<Directory "c:/Apache24/htdocs">
?    #
?    # Possible values for the Options directive are "None", "All",
?    # or any combination of:
?    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
?    #
?    # Note that "MultiViews" must be named *explicitly* --- "Options All"
?    # doesn't give it to you.
?    #
?    # The Options directive is both complicated and important.  Please see
?    # http://httpd.apache.org/docs/2.4/mod/core.html#options
?    # for more information.
?    #
?    Options Indexes FollowSymLinks ExecCGI

    #
    # AllowOverride controls what directives may be placed in .htaccess files.
    # It can be "All", "None", or any combination of the keywords:
    #   AllowOverride FileInfo AuthConfig Limit
    #
    AllowOverride All
    
    #
    # Controls who can get stuff from this server.
    #
    Require all granted
</Directory>

# 默认首页
<IfModule dir_module>
?    DirectoryIndex index.html index.php
</IfModule>
```



### 监听端口

```sh
#Listen 12.34.56.78:80
Listen 80
```
启动时要保证相同端口不被其他程序占用
http://jingyan.baidu.com/article/c85b7a642df6f7003bac95d9.html



### 日志

```sh
ErrorLog "logs/error.log"
LogLevel warn

<IfModule log_config_module>
  CustomLog "logs/access.log" common
</IfModule>
```



### 虚拟主机

```sh
Include conf/extra/httpd-vhosts.conf
```

设置服务器域名
```sh
ServerName www.example.com:80
```

IPv6支持
```sh
Listen [fe80::ecbe:a1a4:f64c:2756]:80
<VirtualHost [fe80::ecbe:a1a4:f64c:2756]:80>
    DocumentRoot "K:\Benny"
#    ServerName [fe80::ecbe:a1a4:f64c:2756]
	SetEnv RUNTIME_ENVIROMENT DEV
</VirtualHost>
```
浏览器访问 http://[fe80::ecbe:a1a4:f64c:2756]/



### HTTP 压缩

```sh
LoadModule headers_module modules/mod_headers.so
LoadModule deflate_module modules/mod_deflate.so
LoadModule filter_module modules/mod_filter.so
<IfModule deflate_module>
SetOutputFilter DEFLATE
# DeflateCompressionLevel 9
# SetEnvIfNoCase Request_URI "\.(?:gif|jpe?g|png)$" no-gzip
AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript
AddOutputFilter DEFLATE js css
</IfModule>
```



### 支持 PHP

参阅文档 http://php.net/manual/fa/install.windows.apache2.php

httpd.conf
```sh
Include conf/extra/httpd-php.conf
```

**FastCGI**
httpd-php.conf

```
LoadModule fcgid_module modules/mod_fcgid.so
<IfModule fcgid_module>
Include conf/extra/httpd-fcgid.conf
FcgidInitialEnv PHPRC "G:/ProgramFiles/php-7.1.3-x64/"
AddHandler fcgid-script .php
FcgidWrapper "G:/ProgramFiles/php-7.1.3-x64/php-cgi.exe" .php
</IfModule>
```

**ISAPI**
PHP 线程安全 ts 有 php7apache2_4.dll
Apache 2.2

```sh
LoadModule php5_module "c:/php/php5apache2.dll"
AddHandler application/x-httpd-php .php
```

configure the path to php.ini
```
PHPIniDir "C:/php"
```

**URL 重写**

```sh
LoadModule rewrite_module modules/mod_rewrite.so
```

目录配置
```sh
AllowOverride All
```

动态配置
```sh
# cmd 下创建点开头的文件
echo > .htaccess
```



### 其它配置

开启 MPM
Include conf/extra/httpd-mpm.conf

用户目录设置
Include conf/extra/httpd-userdir.conf

开启手册
Include conf/extra/httpd-manual.conf

配置 cgi-bin
```sh
<Directory "c:/Apache24/cgi-bin">
    AllowOverride None
    Options None
    Require all granted
</Directory>
<IfModule alias_module>
    #
    # Redirect: Allows you to tell clients about documents that used to 
    # exist in your server's namespace, but do not anymore. The client 
    # will make a new request for the document at its new location.
    # Example:
    # Redirect permanent /foo http://www.example.com/bar

    #
    # Alias: Maps web paths into filesystem paths and is used to
    # access content that does not live under the DocumentRoot.
    # Example:
    # Alias /webpath /full/filesystem/path
    #
    # If you include a trailing / on /webpath then the server will
    # require it to be present in the URL.  You will also likely
    # need to provide a <Directory> section to allow access to
    # the filesystem path.
    
    #
    # ScriptAlias: This controls which directories contain server scripts. 
    # ScriptAliases are essentially the same as Aliases, except that
    # documents in the target directory are treated as applications and
    # run by the server when requested rather than as documents sent to the
    # client.  The same rules about trailing "/" apply to ScriptAlias
    # directives as to Alias.
    #
    ScriptAlias /cgi-bin/ "c:/Apache24/cgi-bin/"

</IfModule>
```



# 参考

- [基于HTTP协议的轻量级开源简单队列服务：HTTPSQS[原创]](http://zyan.cc/httpsqs/)

- [《APMServ 5.2.6》：一键快速搭建Apache＋PHP＋MySQL＋Nginx＋Memcached＋ASP平台的绿色软件[原创]](http://zyan.cc/post/373/)
