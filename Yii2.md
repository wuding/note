# Yii2

Yes It Is! 的缩写。读作“易”，含义：极致简单与不断演变。



## 安装 Yii

需要 PHP 5.4.0 或以上版本支持。

```sh
composer create-project --prefer-dist yiisoft/yii2-app-basic basic

# 最新开发版
composer create-project --prefer-dist --stability=dev yiisoft/yii2-app-basic basic
```

修改 config/web.php 文件，配置密钥：
```php
// !!! 在下面插入一段密钥（若为空） - 以供 cookie validation 的需要
'cookieValidationKey' => '在此处输入你的密钥',
```



### 安装 Assets



### 验证安装的结果

```sh
# port 可选，默认 8080
php yii serve --port=8888
```
http://localhost:8080/

检查当前 PHP 环境是否满足 Yii 最基本需求：
```sh
php requirements.php
```

### 配置 Web 服务器

#### 推荐使用的 Apache 配置
```ini
# 设置文档根目录为 "basic/web"
DocumentRoot "path/to/basic/web"

<Directory "path/to/basic/web">
    # 开启 mod_rewrite 用于美化 URL 功能的支持（译注：对应 pretty URL 选项）
    RewriteEngine on
    # 如果请求的是真实存在的文件或目录，直接访问
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    # 如果请求的不是真实文件或目录，分发请求至 index.php
    RewriteRule . index.php

    # if $showScriptName is false in UrlManager, do not allow accessing URLs with script name
    RewriteRule ^index.php/ - [L,R=404]
    
    # ...其它设置...
</Directory>
```

#### 推荐使用的 Nginx 配置
```ini
server {
    charset utf-8;
    client_max_body_size 128M;

    listen 80; ## listen for ipv4
    #listen [::]:80 default_server ipv6only=on; ## listen for ipv6

    server_name mysite.test;
    root        /path/to/basic/web;
    index       index.php;

    access_log  /path/to/basic/log/access.log;
    error_log   /path/to/basic/log/error.log;

    location / {
        # Redirect everything that isn't a real file to index.php
        try_files $uri $uri/ /index.php$is_args$args;
    }

    # uncomment to avoid processing of calls to non-existing static files by Yii
    #location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
    #    try_files $uri =404;
    #}
    #error_page 404 /404.html;

    # deny accessing php files for the /assets directory
    location ~ ^/assets/.*\.php$ {
        deny all;
    }
    
    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass 127.0.0.1:9000;
        #fastcgi_pass unix:/var/run/php5-fpm.sock;
        try_files $uri =404;
    }

    location ~* /\. {
        deny all;
    }
}
```
php.ini 文件中设置 `cgi.fix_pathinfo=0` ， 能避免掉很多不必要的 stat() 系统调用。

当运行一个 HTTPS 服务器时，需要添加 `fastcgi_param HTTPS on`; 一行， 这样 Yii 才能正确地判断连接是否安全。
