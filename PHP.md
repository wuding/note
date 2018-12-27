PHP ����ָ��
===========

# I. ��װ PHP

## ����
http://windows.php.net/download
http://windows.php.net/downloads/
https://windows.php.net/downloads/releases/archives/php-5.4.45-Win32-VC9-x86.zip

Thread Safe ���� php7apache2_4.dll
��Ӧ�汾�� VC ���п�Ҫ��װ

## ����
1. ���� Windows PATH ����·�������� C:\php

2. �����в鿴�汾��Ϣ

```
php -v
```



## ������

### PHP-CGI
start D:/ProgramFiles/nginx-1.13.10/RunHiddenConsole.exe D:/ProgramFiles/nginx-1.13.10/xxfpm.exe "D:/ProgramFiles/php-7.2.3-nts-x64/php-cgi.exe" -n 2 -i 127.0.0.1 -p 9000



# II. ����

php.ini

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
extension=curl
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



### �ο���

[iniscan](https://github.com/psecio/iniscan)

[php-ini-cleanup](https://github.com/perusio/php-ini-cleanup)

[php.ini���İ�](https://github.com/SeonWaterLee/php-ini)

[PHP�����ļ������ķ���](https://github.com/HeDefine/PHP.ini-for-Chinese)

[very-secure-php-ini](https://github.com/danehrlich1/very-secure-php-ini)



# III. ���Բο�

## �����

��������� instanceof

### ����
42 ** 2 = 42 * 42



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
