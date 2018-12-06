MySQL
======

# 安装
## 下载

https://dev.mysql.com/downloads/mysql/
-> https://dev.mysql.com/downloads/windows/installer/5.7.html
--> https://dev.mysql.com/downloads/file/?id=467606
---> https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.17.0.msi

https://dev.mysql.com/downloads/mysql/8.0.html
-> https://dev.mysql.com/downloads/windows/installer/8.0.html

*https://dev.mysql.com/downloads/installer/

## Windows 服务
```
mysqld -install
mysqld -remove MySQL
mysqld --help
```

### 彻底删除
```
net stop MySQL
sc delete MySQL
```

*注册表清理*
```
\HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\eventlog\Application\MySQL
\HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\services\eventlog\Application\MySQL
\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\eventlog\Application\MySQL
```
搜索 MySQL

## 安装错误
### 2503 权限不够
以管理员身份运行命令提示符 cmd，在命令行下打开安装程序
```sh
C:\windows\system32>j:
J:\>cd J:\inst\MySQL
J:\inst\MySQL>start mysql-5.5.61-winx64.msi
```



# 命令行

**MySQL 5.5 Command Line Client**
The MySQL Command Line Shell
其实就是改了图标

```
%SystemRoot%\Installer\{CB61ED82-C788-4C88-B173-7CF9E84623BB}\Icon.MysqlCmdShell
```
的快捷方式
```sh
"C:\Program Files\MySQL\MySQL Server 5.5\bin\mysql.exe" "--defaults-file=C:\Program Files\MySQL\MySQL Server 5.5\my.ini" "-uroot" "-p"
```
可以在 cmd 里启动
```sh
start mysql
mysql -u root -p
mysqladmin -u用户名 -p旧密码 password 新密码
```

### SET 命令改密码
```sh
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.5.19 MySQL Community Server (GPL)

Copyright (c) 2000, 2011, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>set password for root@localhost = password('root');
```

### 数据表改密码
```
mysql>use mysql; 
mysql>update user set password=password('123') where user='root' and host='localhost'; 
mysql>flush privileges; 
```

### 忘记密码
跳过权限表验证
```
mysqld --skip-grant-tables
```



## 帮助

### help

```sh
mysql> help

For information about MySQL products and services, visit:
   http://www.mysql.com/
For developer information, including the MySQL Reference Manual, visit:
   http://dev.mysql.com/
To buy MySQL Enterprise support, training, or other products, visit:
   https://shop.mysql.com/

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
notee     (\t) Don't write into outfile.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog
with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.

For server side help, type 'help contents'

mysql>
```

### help contents

```sh
mysql> help contents
You asked for help about help category: "Contents"
For more information, type 'help <item>', where <item> is one of the following
categories:
   Account Management
   Administration
   Compound Statements
   Data Definition
   Data Manipulation
   Data Types
   Functions
   Functions and Modifiers for Use with GROUP BY
   Geographic Features
   Help Metadata
   Language Structure
   Plugins
   Table Maintenance
   Transactions
   User-Defined Functions
   Utility
```
### help Data Definition

```sh
mysql> help Data Definition
You asked for help about help category: "Data Definition"
For more information, type 'help <item>', where <item> is one of the following
topics:
   ALTER DATABASE
   ALTER EVENT
   ALTER FUNCTION
   ALTER LOGFILE GROUP
   ALTER PROCEDURE
   ALTER SERVER
   ALTER TABLE
   ALTER TABLESPACE
   ALTER VIEW
   CONSTRAINT
   CREATE DATABASE
   CREATE EVENT
   CREATE FUNCTION
   CREATE INDEX
   CREATE PROCEDURE
   CREATE SERVER
   CREATE TABLE
   CREATE TABLESPACE
   CREATE TRIGGER
   CREATE VIEW
   DROP DATABASE
   DROP EVENT
   DROP FUNCTION
   DROP INDEX
   DROP PROCEDURE
   DROP SERVER
   DROP TABLE
   DROP TABLESPACE
   DROP TRIGGER
   DROP VIEW
   MERGE
   RENAME TABLE
   TRUNCATE TABLE
```
### help CREATE DATABASE

```sh
mysql> help CREATE DATABASE
Name: 'CREATE DATABASE'
Description:
Syntax:
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name
    [create_specification] ...

create_specification:
    [DEFAULT] CHARACTER SET [=] charset_name
  | [DEFAULT] COLLATE [=] collation_name

CREATE DATABASE creates a database with the given name. To use this
statement, you need the CREATE privilege for the database. CREATE
SCHEMA is a synonym for CREATE DATABASE.

URL: http://dev.mysql.com/doc/refman/5.5/en/create-database.html


mysql>
```
示例
```sql
CREATE SCHEMA `soft` DEFAULT CHARACTER SET utf8mb4 ;
```

### help ALTER DATABASE

```sh
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 20
Server version: 5.5.19 MySQL Community Server (GPL)

Copyright (c) 2000, 2011, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> help ALTER DATABASE
Name: 'ALTER DATABASE'
Description:
Syntax:
ALTER {DATABASE | SCHEMA} [db_name]
    alter_specification ...
ALTER {DATABASE | SCHEMA} db_name
    UPGRADE DATA DIRECTORY NAME

alter_specification:
    [DEFAULT] CHARACTER SET [=] charset_name
  | [DEFAULT] COLLATE [=] collation_name

ALTER DATABASE enables you to change the overall characteristics of a
database. These characteristics are stored in the db.opt file in the
database directory. To use ALTER DATABASE, you need the ALTER privilege
on the database. ALTER SCHEMA is a synonym for ALTER DATABASE.

The database name can be omitted from the first syntax, in which case
the statement applies to the default database.

National Language Characteristics

The CHARACTER SET clause changes the default database character set.
The COLLATE clause changes the default database collation.
http://dev.mysql.com/doc/refman/5.5/en/charset.html, discusses
character set and collation names.

You can see what character sets and collations are available using,
respectively, the SHOW CHARACTER SET and SHOW COLLATION statements. See
[HELP SHOW CHARACTER SET], and [HELP SHOW COLLATION], for more
information.

If you change the default character set or collation for a database,
stored routines that use the database defaults must be dropped and
recreated so that they use the new defaults. (In a stored routine,
variables with character data types use the database defaults if the
character set or collation are not specified explicitly. See [HELP
CREATE PROCEDURE].)

Upgrading from Versions Older than MySQL 5.1

The syntax that includes the UPGRADE DATA DIRECTORY NAME clause updates
the name of the directory associated with the database to use the
encoding implemented in MySQL 5.1 for mapping database names to
database directory names (see
http://dev.mysql.com/doc/refman/5.5/en/identifier-mapping.html). This
clause is for use under these conditions:

o It is intended when upgrading MySQL to 5.1 or later from older
  versions.

o It is intended to update a database directory name to the current
  encoding format if the name contains special characters that need
  encoding.

o The statement is used by mysqlcheck (as invoked by mysql_upgrade).

For example, if a database in MySQL 5.0 has the name a-b-c, the name
contains instances of the - (dash) character. In MySQL 5.0, the
database directory is also named a-b-c, which is not necessarily safe
for all file systems. In MySQL 5.1 and later, the same database name is
encoded as a@002db@002dc to produce a file system-neutral directory
name.

When a MySQL installation is upgraded to MySQL 5.1 or later from an
older version,the server displays a name such as a-b-c (which is in the
old format) as #mysql50#a-b-c, and you must refer to the name using the
#mysql50# prefix. Use UPGRADE DATA DIRECTORY NAME in this case to
explicitly tell the server to re-encode the database directory name to
the current encoding format:

ALTER DATABASE `#mysql50#a-b-c` UPGRADE DATA DIRECTORY NAME

After executing this statement, you can refer to the database as a-b-c
without the special #mysql50# prefix.

URL: http://dev.mysql.com/doc/refman/5.5/en/alter-database.html


mysql>
```
示例
```sql
ALTER DATABASE `soft` COLLATE utf8mb4_unicode_ci
```



# 图形界面客户端工具

## MySQL Workbench 6.3 CE



# Document Store
https://yq.aliyun.com/articles/38288


# 数据表操作

## 增
INSERT INTO tbl SELECT fld FROM table
SELECT fld INTO tbl FROM table

REPLACE INTO test VALUES (1, 'New', '2014-08-20 18:47:42');

## 查
SELECT DISTINCT fld FROM tbl

SELECT fld AS '列 名',CASE WHEN age>22 THEN '大龄' ELSE '正常' END AS "年 龄",(CASE sex WHEN 1 THEN '男' WHEN 2 THEN '女' ELSE '未知' END) 性别

### 连接
SELECT fld FROM tbl [INNER] JOIN table ON 
SELECT fld FROM tbl LEFT JOIN table ON
SELECT fld FROM tbl RIGHT JOIN table ON
SELECT fld FROM tbl FULL JOIN table ON
SELECT fld FROM tbl CROSS JOIN table ON

**别名**
SELECT fld FROM tbl A JOIN tbl B ON 



## 改

UPDATE tbl SET fld = REPLACE(fld, from, to)



# 定义语言

CREATE SERVER srv

EXPLAIN SELECT

## 表

### 创建
CREATE TABLE tbl LIKE table
CREATE TABLE tbl AS SELECT fld FROM table

SHOW CREATE TABLE tbl

CREATE TEMPORARY TABLE tbl

### 修改
ALTER TABLE `tbl` DROP `fld`[, DROP `col`];

## 视图
CREATE VIEW tbl AS SELECT fld FROM table


## 索引
ALTER TABLE tbl ADD PRIMARY KEY (`col`)
ALTER TABLE tbl ADD UNIQUE (`col`)
ALTER TABLE tbl ADD FULLTEXT (`col`)
ALTER TABLE tbl ADD INDEX idx (`col`,`fld`)



# 函数

## 更新替换
replace(object,search,replace)

UPDATE `url_site_detail` SET pic = REPLACE(pic, 'http://www.myzyzy.com/Uploads/', 'http://pic.myzyzy.com/') WHERE `pic` REGEXP '^http://www.myzyzy.com/Uploads/vod/'

## 修剪
trim([{BOTH | LEADING | TRAILING} [remstr] FROM] str)


# 引擎

## 操作
SHOW ENGINES

## MRG_MYISAM
auction.MRG



# 数据文件

5.7 5.5 不兼容



# 字符集

SET NAMES utf8mb4

========================
索引优化、查询优化和存储优化
主从开发