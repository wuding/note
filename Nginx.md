Nginx 使用指南
=============
读音 Engine X
- https://www.zhihu.com/question/19739907
- https://segmentfault.com/q/1010000002432695

# 安装 Nginx
安装 nginx/Windows，需要下载 1.3.11 或以上版本

- http://nginx.org/en/download.html
- https://nginx.org/download/

##### 参考：
- http://www.cnblogs.com/nick-huang/p/4638398.html
- [Nginx 安装配置](http://www.runoob.com/linux/nginx-install-setup.html)
- [nginx服务器安装及配置文件详解](http://seanlook.com/2015/05/17/nginx-install-and-config/)
- [Nginx安装及配置文件 nginx.conf 详解](https://div.io/topic/1395)


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

查看进程
```sh
tasklist /fi "imagename eq nginx.exe"
```

nginx/Windows作为标准控制台应用运行，而不是系统服务。可以用下面的命令控制：
```sh
nginx -s stop	# 快速退出
nginx -s quit	# 优雅退出
nginx -s reload	# 更换配置，启动新的工作进程，优雅的关闭以往的工作进程
nginx -s reopen	# 重新打开日志文件
```

# 配置
配置文件中的目录请使用“/”，而不是“\”做目录分隔

##### 工具：
- https://nginxconfig.io/
- https://www.tools4nerds.com/online-tools/nginx-conf-generator

##### 参考：
- [Full Example Configuration](https://www.nginx.com/resources/wiki/start/topics/examples/full/)
- [nginx快速入门之配置篇](https://zhuanlan.zhihu.com/p/31202053)
- [Nginx 配置文件nginx.conf中文详解](https://www.w3cschool.cn/nginx/nginx-d1aw28wa.html) - [简书](https://www.jianshu.com/p/3e2b9964c279) - [博客园](https://www.cnblogs.com/liang-wei/p/5849771.html)
- [nginx的配置、虚拟主机、负载均衡和反向代理（1）](https://www.zybuluo.com/phper/note/89391) - [（2）](https://www.zybuluo.com/phper/note/90310) - [（3）](https://www.zybuluo.com/phper/note/133244)

## 虚拟主机
### 虚拟主机名
可以使用确切的名字，通配符，或者是正则表达式来定义：
```sh
# 不含“Host”字段的请求，需要指定一个空名字
# 可以将IP地址作为虚拟主机名
# 特殊的虚拟主机名“$hostname”
server {
    listen       80;
    server_name  example.org  www.example.org "" 192.168.1.1;
    ...
}

server {
    listen       80;
    server_name  *.example.org;
    ...
}

server {
    listen       80;
    server_name  mail.*;
    ...
}

server {
    listen       80;
    server_name  ~^(?<user>.+)\.example\.net$;
    ...
}
```
- 通配符名字只可以在名字的起始处或结尾处包含一个星号，并且星号与其他字符之间用点分隔。
- 有一种形如“.example.org”的特殊通配符
- 为了使用正则表达式，虚拟主机名必须以波浪线“~”起始。正则表达式是一个一个串行的测试，所以是最慢的
- 命名的正则表达式捕获组在后面可以作为变量使用：
```sh
server {
    server_name   ~^(www\.)?(?<domain>.+)$;

    location / {
        root   /sites/$domain;
    }
}

server {
    server_name   ~^(www\.)?(.+)$;

    location / {
        root   /sites/$2;
    }
}
```
- 在匹配所有的服务器的例子中，可以见到一个奇怪的名字“_”：
```sh
server {
    listen       80  default_server;
    server_name  _;
    return       444;
}
```
如果定义了大量名字，或者定义了非常长的名字，那可能需要在http配置块中使用server_names_hash_max_size和server_names_hash_bucket_size指令进行调整。

### 默认虚拟主机
- 第一个被列出的虚拟主机即nginx的默认虚拟主机
- default_server 显式地设置某个主机为默认虚拟主机，捕获组也可以以数字方式引用：
```sh
server {
    listen      192.168.1.1:80 default_server;
    server_name example.net www.example.net;
    ...
}

server {
    listen      192.168.1.2:80 default_server;
    server_name example.com www.example.com;
    ...
}
```
请注意"default_server"是监听端口的属性，而不是主机名的属性。



## 虚拟目录

```
location /path/ {
    root html
}
# 实际访问 html/path/

location /paths/ {
    alias html/paths/
}
# 路径结尾必须加斜杠
```



## 进程

```
# 工作进程数量需要不少于可用CPU的个数
worker_processes  2;
```
- 虽然可以启动若干工作进程运行，实际上只有一个进程在处理请求所有请求。
- 一个工作进程只能处理不超过1024个并发连接。

## 日志
```
error_log  logs/error.log debug;

log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';

access_log  logs/access.log  main;
```
如果日志文件不存在，那失败原因会记录在Windows事件日志中。

##### 参考：
- [nginx的error.log日志常见的几个错误解决方法](https://blog.51cto.com/chenx1242/1769724)

## 错误页面
```
error_page   500 502 503 504  /50x.html;
location = /50x.html {
  root   html;
}
```

## SSL证书安装

##### 参考：
- https://cloud.tencent.com/document/product/400/4143
- [HTTPS 简介及使用官方工具 Certbot 配置 Let’s Encrypt SSL 安全证书详细教程](https://linuxstory.org/deploy-lets-encrypt-ssl-certificate-with-certbot/)

```
server {
  listen       80; # 合并HTTP/HTTPS主机
  listen       443 ssl;
  server_name  urlnk.host;

  ssl_certificate      K:\env\win\ProgramData/nginx\conf\cert\urlnk.host.crt;
  ssl_certificate_key  K:\env\win\ProgramData/nginx\conf\cert\urlnk.host.key;

  # HTTPS服务器优化
  keepalive_timeout   70;
  ssl_session_cache    shared:SSL:10m; # 共享会话缓存
  ssl_session_timeout  10m; # 缓存超时（分钟）

  ssl_ciphers  HIGH:!aNULL:!MD5; # 1.0.5及以后版本，默认SSL密码算法
  ssl_prefer_server_ciphers  on;

  location / {
    root	D:\www\work\cdn;
    index  index.html index.htm;
    autoindex on;
  }
}
```

### HTTP/2
```
server {
  listen 443 ssl http2;
  server_name www.urlnk.com;
}
```


## 自定义变量

全局有效，不可以和自带变量重名
```sh
set $doc_root /srv/www/htdocs;
fastcgi_param  DOCUMENT_ROOT      $doc_root;
```



## 运行 php

https://www.nginx.com/resources/wiki/start/topics/examples/phpfastcgionwindows/

try_files 和 QUERY_STRING
```
location / {
  root   K:/env/win/ProgramData/nginx/html;
  index  index.html index.php;
  autoindex on;
  try_files   $uri $uri/ /index.php$uri$is_args$args;
}
```

FastCGI
```
location ~ \.php($|/) {
  root           K:/env/win/ProgramData/nginx/html;
  fastcgi_pass   fastcgi_backend;
  fastcgi_read_timeout 150;
  fastcgi_index  index.php;
  fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;

  fastcgi_split_path_info  ^(.+\.php)(/.*)$;
  fastcgi_param  PATH_INFO $fastcgi_path_info;
  fastcgi_param  RUNTIME_ENVIROMENT 'PRO';

  include        fastcgi_params;
}
```

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



## 80 转 443

```sh
server {
    listen       80;
    server_name  example.org;
    return       301 https://www.example.org$request_uri;
}

# 或者
server {
    listen 80;
    server_name localhost;
    rewrite ^(.*)$ https://${server_name}$1 permanent;
}
```
${server_name} 可以换成 $host



## HTTP 压缩
```
gzip on;
gzip_min_length 1k;
gzip_buffers 4 16k;
gzip_comp_level 5;
gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php;
```
##### 参考：
- [Module ngx_http_gzip_module](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)
- [Nginx配置 - Gzip压缩](https://www.jianshu.com/p/e0ff1e275e7f)
- [为你的网站开启 gzip 压缩功能（nodejs、nginx）](https://blog.wenzhixin.net.cn/2013/11/10/server_gzip_on/)
- [入门系列之在Nginx配置Gzip](https://juejin.im/post/5b518d1a6fb9a04fe548e8fc)


## 反向代理
```
# 配置 Apache 虚拟主机 
<VirtualHost *:8080>
  ServerAdmin webmaster@dummy-host2.example.com
  DocumentRoot "/usr/local/nginx/html"
  ServerName "192.168.80.22"
  ErrorLog "logs/dummy-host2.example.com-error_log"
  CustomLog "logs/dummy-host2.example.com-access_log" common
</VirtualHost>
# 设置权限
<Directory />
  Options FollowSymLinks
  AllowOverride None
  Order deny,allow
  Allow from all
</Directory>

# 设置 Nginx 配置文件 .php 文件让 Apache 来解析
location ~ \.php$ {
  proxy_set_header X-Forwarded-For $remote_addr;
  proxy_pass   http://192.168.80.22:8080;
}
```

## 负载均衡
```
upstream fastcgi_backend {
  server 127.0.0.1:9001;
  server 127.0.0.1:9002;
}

location / {
  proxy_pass http://fastcgi_backend;
}
```

## expires 缓存
```
# 开启gzip
gzip on;
# 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
gzip_min_length 1k;
# gzip 压缩级别，1-10，数字越大压缩的越好，也越占用CPU时间，后面会有详细说明
gzip_comp_level 2;
# 进行压缩的文件类型。javascript有多种形式。其中的值可以在 mime.types 文件中找到。
gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png font/ttf font/otf image/svg+xml;
# 是否在http header中添加Vary: Accept-Encoding，建议开启
gzip_vary on;
# 禁用IE 6 gzip
gzip_disable "MSIE [1-6]\.";

# 开启缓存
location ~* ^.+\.(ico|gif|jpg|jpeg|png)$ { 
    access_log   off; 
    expires      30d;
}

location ~* ^.+\.(css|js|txt|xml|swf|wav)$ {
    access_log   off;
    expires      24h;
}

location ~* ^.+\.(html|htm)$ {
    expires      1h;
}

location ~* ^.+\.(eot|ttf|otf|woff|svg)$ {
    access_log   off;
    expires max;
}

# 格式
# expires 30s;
# expires 30m;
# expires 2h;
# expires 30d;
```

##### 参考：
- [nginx开启gzip和缓存配置](https://segmentfault.com/a/1190000007015010)
- [加速nginx: 开启gzip和缓存](https://www.darrenfang.com/2015/01/setting-up-http-cache-and-gzip-with-nginx/)


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



## could not build server_names_hash, you should increase server_names_hash_bucket_size: 32

```sh
http {
	server_names_hash_bucket_size 512;
```

值为 32 的倍数



## WSASocketW() failed (10106: The requested service provider could not be loaded or initialized)

以管理员身份打开命令提示符
```
netsh winsock reset
```


## SSL: error:0200107B:system library:fopen:Unknown error:fopen
SSL 证书必须放 conf 目录下，用相对路径，如：cert/example.com.crt



## 开机自启动

创建快捷方式放到

%programdata%\Microsoft\Windows\Start Menu\Programs\Startup

*C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp*

```sh
shell:startup
shell:Common Startup
```
C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup



# 参考文档

- [Nginx 入门指南](http://wiki.jikexueyuan.com/project/nginx/)
- [nginx安装配置|nginx负载均衡|nginx反向代理|gzip压缩|expires缓存](https://segmentfault.com/a/1190000011489789)
- https://nginx.org/en/docs/ => http://tengine.taobao.org/nginx_docs/cn/
- https://wizardforcel.gitbooks.io/nginx-doc <=> https://www.kancloud.cn/wizardforcel/nginx-doc
- https://www.yiibai.com/nginx/
- [Nginx 烹调书](https://huliuqing.gitbooks.io/complete-nginx-cookbook-zh/)
- [Nginx完全配置指南](https://legacy.gitbook.com/book/lingerhk/complete-nginx-cookbook/details)
