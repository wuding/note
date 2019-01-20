MySQL
======



#### 参考：

[MySQL 5.1 中文参考手册(在线文档)](http://www.mysqlab.net/docs/view/refman-5.1-zh/chapter/index.html) <=> http://tool.oschina.net/uploads/apidocs/mysql-5.1-zh/

[MySQL 5.6 手册翻译](https://github.com/mysql2cn/manual56) 无人问津

[MySQL 5.7 参考手册](https://strongyoung.gitbooks.io/mysql-reference-manual/) <=> https://github.com/fork-copy/Notes/tree/master/DataBase

https://strongyoung.gitbooks.io/common-mysql-queries/

https://blog.csdn.net/butterfly_resting/article/category/7412077

https://blog.csdn.net/aaaaaa1569/article/category/7723659

[MySQL参考手册译文](http://www.shuaihua.cc/article/tag/storage/)



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

```sh
mysqld -install
mysqld -remove MySQL
mysqld --help
```

### 彻底删除
```sh
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

MySQL Workbench

Navicat

phpMyAdmin

[my.cnf生成工具](http://imysql.com/my-cnf-wizard.html)

[SQLAdvisor](https://github.com/Meituan-Dianping/SQLAdvisor)



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



### 字段自定义排序

```
FIELD(author, '李雷','韩梅梅','安华');
```

#### 参考：

[MySQL in 查询，并通过 FIELD 函数按照查询条件顺序返回结果](https://segmentfault.com/a/1190000003742537)



### 检测空值

ISNULL ( check_expression , replacement_value )

#### 参考：

[SQL中的ISNULL函数介绍](http://database.51cto.com/art/201009/224323.htm)





# 引擎

### 参考：

[数据库存储引擎](https://github.com/jaywcjlove/mysql-tutorial/blob/master/chapter3/3.5.md)

[MySQL优化笔记（五）–数据库存储引擎](https://blog.csdn.net/jack__frost/article/details/72904318) - [更多](https://www.jianshu.com/u/8dad2b82bce7)

[MySQL数据库引擎](https://blog.csdn.net/wangyang1354/article/details/50740041)

[mysql几种引擎和使用场景](https://blog.csdn.net/cool_wayen/article/details/79585277)

[【转载】如何选择MySQL存储引擎](https://www.cnblogs.com/xujishou/p/6343431.html)

[mysql的储存引擎](https://blog.csdn.net/qq_41887789/article/details/79775015)



默认存储引擎

```ini
[mysqld]
default-storage-engine=InnoDB
```

建表时
```mysql
create table mytbl(   
    id int primary key,   
    name varchar(50)   
)type=MyISAM;
```

修改
```mysql
alter table table_name type = InnoDB;
```

检验
```mysql
show table status from table_name;
或者 
show create table table_name
```



## 操作

```mysql
SHOW ENGINES \G;
```



## InnoDB

支持事务 ACID 和外键约束
经常更新的表
通过 bin-log 恢复



## MyISAM

查询速度快
支持全文索引和压缩索引



## MRG_MYISAM 水平分表

相同 MyISAM 表的集合
auction.MRG

```mysql
CREATE TABLE IF NOT EXISTS `alluser` (  
  `id` int(11) NOT NULL ,  
  `name` varchar(50) DEFAULT NULL,  
   PRIMARY KEY (`id`)
) ENGINE=MRG_MYISAM  
DEFAULT CHARSET=utf8 
UNION=(user1,user2);  
```

插入方式
```mysql
ALTER TABLE `test_engine`.`alluser` INSERT_METHOD = LAST;
```
还可以建一个 ID 表，根据个位数（或取模）决定插入哪个表

可以通过修改总表的引擎合并所有子表数据
总表和子表结构必须一致，主键都不可以用自动增长



## Federated

跨服务器代理

https://en.wikipedia.org/wiki/MySQL_Federated



## Archive
仅支持插入和查询，zlib 压缩
作为仓库，存储作为历史记录的数据



## Memory

速度快，适用场景：
内容固定，统计中间表
```ini
max_heap_table_size=
```



## Blackhole

适用场景：
验证 dump file
检测 binlog 
充当日志服务器
复制数据到备份数据库



## CSV

不支持索引，所有字段必须为非空



## Performance_Schema

MySQL 5.5 新增，收集数据库服务器的性能参数



# 数据文件

5.7 5.5 不兼容



# 字符集

SET NAMES utf8mb4



# 主从复制

最好版本一致，要么保证 Master 版本低于 Slave

1. ### 配置 Master 主服务器

```mysql
# 创建新用户 repl
create user repl;

# 必须具有 REPLICATION SLAVE 权限
# 密码为 mysql
# 这里 % 是通配符
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'192.168.100.%' IDENTIFIED BY 'mysql';
```

my.ini 添加

```ini
[mysqld]
# 给数据库服务的唯一标识，一般为大家设置服务器 IP 的末尾号
server-id=4
log-bin=master-bin
log-bin-index=master-bin.index
```

重启 MySQL 服务

查看日志

```mysql
SHOW MASTER STATUS;
```



2. ### 配置 Slave 从服务器

my.ini 添加

```ini
[mysqld]
server-id=12
relay-log-index=slave-relay-bin.index
relay-log=slave-relay-bin 
```

重启 MySQL 服务

连接 Master

```mysql
change master to 
master_host='192.168.100.4', # Master 服务器 IP
master_port=3306,
master_user='repl',
master_password='mysql', 
master_log_file='master-bin.000001', # Master 服务器产生的日志，参考上面的查看 Master 日志
master_log_pos=0;
```
启动 Slave
```mysql
start slave;
SHOW SLAVE STATUS \G；
```



## 常见问题

### Could not initialize master info structrue

```mysql
stop slave;
reset slave;
# 然后重复上面的查看 Master 日志，连接 Master
start slave;
```



### 参考：

[MySQL Reference Manual / Replication](https://dev.mysql.com/doc/refman/5.7/en/replication.html)

[Mysql主从配置，实现读写分离](https://www.cnblogs.com/alvin_xp/p/4162249.html)

[MySQL主从复制与主主复制](https://www.cnblogs.com/phpstudy2015-6/p/6485819.html)

[【实操笔记】MySQL主从同步功能实现](https://www.cnblogs.com/hellxz/p/8378601.html)

[Mysql主从同步(1)-主从/主主环境部署梳理](https://www.cnblogs.com/kevingrace/p/6256603.html)

[Mysql主从同步（复制）](https://www.cnblogs.com/kylinlin/p/5258719.html)

[mysql主从复制实现数据库同步](https://www.cnblogs.com/rwxwsblog/p/4542417.html)

  

# 读写分离

MySQL Proxy

[MyCat](https://github.com/MyCATApache/Mycat-Server) <- Cobar <-  [Amoeba](https://sourceforge.net/projects/amoeba/)

[Atlas](https://github.com/Qihoo360/Atlas)



### 参考：

[MySQL读写分离](https://www.cnblogs.com/phpstudy2015-6/p/6687480.html)



# 索引优化、查询优化和存储优化

### 参考：

[MySQL查询优化](https://www.cnblogs.com/phpstudy2015-6/p/6509331.html)

[MySQL 优化](https://github.com/judasn/Linux-Tutorial/blob/master/markdown-file/Mysql-Optimize.md)



## 组合索引

情景

```mysql
select * from test where sex='M' and city='Guangzhou' order by create_at asc limit 100;
```

解决

```mysql
alter table test add index sex_city_create_at (sex,city,create_at);
```

检验

```mysql
explain select * from test where sex='M' and city='Guangzhou' order by create_at asc limit 100;
```

强制

```mysql
explain select * from test FORCE index(sex_city_create_at) where sex='M' and city='Guangzhou' order by create_at asc limit 100;
```

### 参考：

[MySQL使用组合索引](http://www.luckybird.me/mysql%e4%bd%bf%e7%94%a8%e7%bb%84%e5%90%88%e7%b4%a2%e5%bc%95.html)



# 派生版本

MariaDB

Percona Server for MySQL