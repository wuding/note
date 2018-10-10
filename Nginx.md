Nginx 使用指南
=============

# 安装 Nginx

http://nginx.org/en/download.html
http://nginx.org/download/nginx-1.11.12.zip
https://nginx.org/download/nginx-1.14.0.zip
http://www.cnblogs.com/nick-huang/p/4638398.html

```sh
C:\Users\Administrator>g:
G:\>cd G:\Program_Files\nginx-1.11.12
G:\Program_Files\nginx-1.11.12>start nginx
```



# 命令行

```sh
D:\ProgramFiles\nginx-1.13.10>nginx -h
nginx version: nginx/1.13.10
Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: NONE)
  -c filename   : set configuration file (default: conf/nginx.conf)
  -g directives : set global directives out of configuration file
```



# 配置

## 进程

```
worker_processes  2;
```



## SSL证书安装

https://cloud.tencent.com/document/product/400/4143



## 自定义变量

全局有效，不可以和自带变量重名
```sh
set $doc_root /srv/www/htdocs;
fastcgi_param  DOCUMENT_ROOT      $doc_root;
```



## 运行 php

https://www.nginx.com/resources/wiki/start/topics/examples/phpfastcgionwindows/

try_files 和 QUERY_STRING

**使用 PATH_INFO**

```sh
fastcgi_split_path_info  ^(.+\.php)(/.*)$;
fastcgi_param  PATH_INFO $fastcgi_path_info;
```



## php 使用远程服务器

1. **相对路径**
nginx 和 php-fpm 启动程序的命令行初始目录，php-fpm.conf 可以改 chdir 选项
```sh
fastcgi_param  SCRIPT_FILENAME  html$fastcgi_script_name;
```

2. **绝对路径**
更改 root 文档根路径不会影响远程 php-fpm 运行
```sh
root html;
fastcgi_param  SCRIPT_FILENAME  $doc_root$fastcgi_script_name;
```

**参考：**

- [Nginx调用远程php-fpm](https://www.cnblogs.com/feiyafeiblog/p/6938515.html)



## 跨域

```sh
add_header Access-Control-Allow-Origin *;
add_header Access-Control-Allow-Origin "*";
add_header Access-Control-Allow-Headers "Origin, X-Requested-With, Content-Type, Accept";
add_header Access-Control-Allow-Methods "GET, POST, OPTIONS";
```



# 常见问题

## bind() to 0.0.0.0:443 failed

```sh
netstat -aon|findstr "443"
tasklist|findstr "1408" 
taskkill /F /pid 1408
```

可能和已经运行的 nginx （cmd 下通过运行 .bat 启动，进程可能是 RuntimeBroker.exe） 或 httpd 冲突；

也可能是 vmware-hostd.exe （需要停止 VMware 所有服务，并将启动类型设置为手动）

**参考：**

- [Nginx 错误 bind() to 0.0.0.0:443 failed 解决方法](https://blog.yoodb.com/yoodb/article/detail/1264)

- [Nginx 错误处理方法: bind() to 0.0.0.0:80 failed](https://www.cnblogs.com/YangJieCheng/p/5843660.html)



## 开机自启动

创建快捷方式放到

%programdata%\Microsoft\Windows\Start Menu\Programs\Startup

*C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp*

```sh
shell:startup
```
C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup



# 参考文档

- [Nginx 入门指南](http://wiki.jikexueyuan.com/project/nginx/)