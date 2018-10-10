Apache HTTP Server (httpd) ��������
================================

http://blog.csdn.net/lsyz0021/article/details/51998848

# ��װ Apache

## ����

�ٷ�����ҳ
http://httpd.apache.org/download.cgi

### �������°�
ѡ������һ�����а�
http://httpd.apache.org/docs/current/platform/windows.html#down

����
https://www.apachelounge.com/download/

ѹ�����ļ���ַ
http://home.apache.org/~steffenal/VC15/binaries/httpd-2.4.34-win64-VC15.zip



### ���ؾɰ� 2.2

����汾֧�� Windows XP/2003
http://archive.apache.org/dist/httpd/binaries/win32/
https://www.apachelounge.com/download/win32/

ѡ���Ƿ���� SSL
http://archive.apache.org/dist/httpd/binaries/win32/httpd-2.2.25-win32-x86-no_ssl.msi
http://archive.apache.org/dist/httpd/binaries/win32/httpd-2.2.25-win32-x86-openssl-0.9.8y.msi

�����΢��һ��
https://www.apachelounge.com/download/win32/binaries/httpd-2.2.34-win32.zip

��Ҫ VC++ 2010
https://www.microsoft.com/zh-CN/download/details.aspx?id=8328



## ��װ

�ٷ��ĵ�
http://httpd.apache.org/docs/2.4/platform/windows.html

��װΪ����ǰ��������
```sh
httpd.exe -k install
```

��������

```sh
bin> httpd.exe -w -n "Apache2" -k start
```

�����а���
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



### ģ��

**mod_fcgid**
https://www.apachelounge.com/download/
https://www.apachelounge.com/download/VC15/modules/mod_fcgid-2.3.9-win64-VC15.zip


## ����
httpd.conf

### Ŀ¼����
����Ŀ¼������ڣ������޷�������Ҳû�д�����־
```sh
# �������Ŀ¼
ServerRoot "c:/Apache24"

# ��վ��Ŀ¼
DocumentRoot "c:/Apache24/htdocs/html"

# Ŀ¼ѡ������
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

# Ĭ����ҳ
<IfModule dir_module>
?    DirectoryIndex index.html index.php
</IfModule>
```



### �����˿�

```sh
#Listen 12.34.56.78:80
Listen 80
```
����ʱҪ��֤��ͬ�˿ڲ�����������ռ��
http://jingyan.baidu.com/article/c85b7a642df6f7003bac95d9.html



### ��־

```sh
ErrorLog "logs/error.log"
LogLevel warn

<IfModule log_config_module>
  CustomLog "logs/access.log" common
</IfModule>
```



### ��������

```sh
Include conf/extra/httpd-vhosts.conf
```

���÷���������
```sh
ServerName www.example.com:80
```

IPv6֧��
```sh
Listen [fe80::ecbe:a1a4:f64c:2756]:80
<VirtualHost [fe80::ecbe:a1a4:f64c:2756]:80>
    DocumentRoot "K:\Benny"
#    ServerName [fe80::ecbe:a1a4:f64c:2756]
	SetEnv RUNTIME_ENVIROMENT DEV
</VirtualHost>
```
��������� http://[fe80::ecbe:a1a4:f64c:2756]/



### HTTP ѹ��

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



### ֧�� PHP

�����ĵ� http://php.net/manual/fa/install.windows.apache2.php

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
PHP �̰߳�ȫ ts �� php7apache2_4.dll
Apache 2.2

```sh
LoadModule php5_module "c:/php/php5apache2.dll"
AddHandler application/x-httpd-php .php
```

configure the path to php.ini
```
PHPIniDir "C:/php"
```

**URL ��д**

```sh
LoadModule rewrite_module modules/mod_rewrite.so
```

Ŀ¼����
```sh
AllowOverride All
```

��̬����
```sh
# cmd �´����㿪ͷ���ļ�
echo > .htaccess
```



### ��������

���� MPM
Include conf/extra/httpd-mpm.conf

�û�Ŀ¼����
Include conf/extra/httpd-userdir.conf

�����ֲ�
Include conf/extra/httpd-manual.conf

���� cgi-bin
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



# �ο�

- [����HTTPЭ�����������Դ�򵥶��з���HTTPSQS[ԭ��]](http://zyan.cc/httpsqs/)

- [��APMServ 5.2.6����һ�����ٴApache��PHP��MySQL��Nginx��Memcached��ASPƽ̨����ɫ���[ԭ��]](http://zyan.cc/post/373/)
