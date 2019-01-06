PHP ����ָ��
===========

# I. ��װ PHP

## ����
http://windows.php.net/download
http://windows.php.net/downloads/
https://windows.php.net/downloads/releases/archives/php-5.4.45-Win32-VC9-x86.zip

Thread Safe ���� php7apache2_4.dll
��Ӧ�汾�� VC ���п�Ҫ��װ



## ��������

### VC15

https://visualstudio.microsoft.com/downloads/

Other Tools and Frameworks > Microsoft Visual C++ Redistributable for Visual Studio 2017



## ����
1. ���� Windows PATH ����·�������� C:\php

2. �����в鿴�汾��Ϣ

```
php -v
```



## ������

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





## ������

### PHP-CGI
start D:/ProgramFiles/nginx-1.13.10/RunHiddenConsole.exe D:/ProgramFiles/nginx-1.13.10/xxfpm.exe "D:/ProgramFiles/php-7.2.3-nts-x64/php-cgi.exe" -n 2 -i 127.0.0.1 -p 9000



# II. ����

php.ini



#### �ο���

[iniscan](https://github.com/psecio/iniscan)

[php-ini-cleanup](https://github.com/perusio/php-ini-cleanup)

[php.ini���İ�](https://github.com/SeonWaterLee/php-ini)

[PHP�����ļ������ķ���](https://github.com/HeDefine/PHP.ini-for-Chinese)

[very-secure-php-ini](https://github.com/danehrlich1/very-secure-php-ini)



## ����ʱ����

### �����ļ�

1. **����·��**
    SAPI ָ��λ�� PHPIniDir, -c
    PHPRC ��������
    ע��� [HKEY_LOCAL_MACHINE\SOFTWARE\PHP\(x(.y(.z)))] IniFilePath
    ��ǰ����Ŀ¼������ CLI��
    ?web ������(��)Ŀ¼������ SAPI ģ�飩
    Windows Ŀ¼

2. **����**
    php-SAPI.ini ����� php.ini

3. **����ʹ��**
    ��������
    �����Ѵ��ڵ� .ini ���� open_basedir = ${open_basedir} ":/new/dir"




### .user.ini �ļ�

  ```ini
  user_ini.filename
  user_ini.cache_ttl 
  ```



### ���ÿɱ��趨��Χ

  PHP_INI_ALL �����κεط��趨 
  PHP_INI_USER ini_set() ע��� .user.ini  
  PHP_INI_PERDIR ���� php.ini httpd.conf  .htaccess (.user.ini ?)
  PHP_INI_SYSTEM ���� php.ini �� httpd.conf



### �޸�
1. **.htaccess**

  ```sh
  php_value name value
  php_flag name on|off
  ```

2. **httpd.conf**
    ���ᱻ .htaccess ����

  ```
  php_admin_value name value|none
  php_admin_flag name on|off
  ```

3. **ע���**
    HKLM\SOFTWARE\PHP\Per Directory Values\c\inetpub\wwwroot 



## ����������

### ʱ��
```
[Date]
date.timezone = PRC
```
��Ӧ�� php ����
```php
date_default_timezone_set()
```


### PECL ��չ

��չĿ¼

```ini
extension_dir = "ext"
```



������չ

```ini
; Э��
extension=curl
extension=php_openssl.dll

; �ı��ͷ���
extension=php_gettext.dll
extension=php_intl.dll
extension=php_mbstring.dll

; ���ݿ�
extension=php_mysqli.dll
extension=php_pdo_mysql.dll
```




## �߼�������

### ���� OPcache
```
[opcache]
zend_extension="K:\env\win\ProgramFiles(x86)\php-7.3.0RC1-nts\ext\php_opcache.dll"

; Determines if Zend OPCache is enabled
opcache.enable=1

; Determines if Zend OPCache is enabled for the CLI version of PHP
opcache.enable_cli=1
```



## ��������

### Paths and Directories

```ini
include_path = ".;c:\php\includes"
sys_temp_dir = "D:/env/tmp/sys"
enable_dl = Off
```



### File Uploads

```ini
file_uploads = On
upload_tmp_dir = "D:/env/tmp/upload"
upload_max_filesize = 20M
max_file_uploads = 20
```



### ����

```ini
[Date]
date.timezone = PRC
```



### �Ự

```ini
[Session]
session.save_handler = files
session.save_path = "N;MODE;/path"
session.name = PHPSESSID
session.use_trans_sid = 0
session.use_cookies = 1
session.use_only_cookies = 1
; ֵ����Ϊ 0 �����ڿͻ��˽��� cookie ʱ�� sid
```

#### �ο���

[����cookie��session��������õ�](http://blog.sina.com.cn/s/blog_4d6c4525010173rf.html)







# III. ���Բο�

## �����

��������� instanceof

### ����
```
42 ** 2 = 42 * 42
```



## �������

Trait

����
__set
__get
__isset
__unset

__call
__callStatic

ħ������
__sleep
__wakeup

__toString
__invoke

__set_state
__debugInfo



## Ԥ����ӿ�


Closure



# IV. �����ο�

WinCache



## �������ܲ���

### ����

- [x] �����������ּ�����
- [x] �Ƚ�����ļ�ֵ
- [x] �ϲ� JSON ��ʽ���飬���ı��洢



# V. ��¼

## php.ini ����

ѡ���б� 

���б� 
[HOST=<host>]
[PATH=<path>] 

(��������)ѡ��˵�� 

���ִ��ʱ�䡢�ϴ��ļ���С



## �汾����

PHP5.6 ��������list��Ϊ���󷽷���PHP7����
PHP5.4 ������ $class->object()::staticfunc(); Ҫ $obj = $class->object(); $obj::staticfunc(); object()�ǻ�ȡһ������



# VI. �ؼ�����

  Queries per second (QPS)
  OOP
  ���漼������̬�����
  Web Service



## RESTful

- [php��η���POST DELETE GET POST ����](http://www.cnblogs.com/agang-php/p/5210571.html)



## ���߳�

- [PHP �������̵߳�ʹ��](http://zyan.cc/pthreads/)



# VII. �ο��ĵ�

- [��PHP�������������Ա�ռ������ PHP ��Դ��ȫ](http://www.cnblogs.com/taletao/p/4212916.html)
- [�ִ� PHP ������ϵ��](https://laravelacademy.org/post/4221.html)
- [Laravel ������Դ��Ŀ��ȫ](https://laravelacademy.org/laravel-project)
