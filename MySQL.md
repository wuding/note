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

[MySQL 资源大全中文版](https://github.com/jobbole/awesome-mysql-cn)

https://github.com/necan/MySQL-Tutorial



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
mysqld --install MySQLXY --defaults-file="C:\Program Files\MySQL\MySQL Server X.Y\my.ini"
mysqld --remove MySQL
mysqld --help
```

使用 sc 服务控制：
```sh
sc delete MySQL56
sc create MySQL56 start= auto  DisplayName= MySQL56  binPath= "\"C:\Program Files\MySQL\MySQL Server 5.6\bin\mysqld.exe\" --defaults-file=\"C:\ProgramData\MySQL\MySQL Server 5.6\my.ini\" MySQL56"
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

##### 参考：
- https://stackoverflow.com/questions/49958404/how-to-create-a-windows-service-using-sc-mysql

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
Enter password: ****b
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



### 常见问题

#### 中文乱码

my.ini

```ini
[client]
port=3306
[mysql]
default-character-set=utf8
[mysqld]
character-set-server=utf8mb4
```

mysql

```sql
-- 查看MySql编码格式
show variables like '%char%';

set character_set_database=utf8;
set character_set_server=utf8;
set character_set_client=gb2312;
set character_set_connection=gb2312;

-- SELECT 用到
set character_set_results=gb2312; 
```

[MySQL命令行查询乱码解决方法：](https://www.cnblogs.com/aksir/p/7070493.html)

[MySql中文乱码的那些事](https://www.jianshu.com/p/f6a6b624fc11)

[关于MySQL中的8个 character_set 变量说明](https://www.jianshu.com/p/7e839daccaf6)

[mysql字符集小结](https://blog.csdn.net/wyzxg/article/details/8779682)



### 初始化数据库目录

```sh
C:\Program Files\MySQL\MySQL Server 5.7\bin>mysqld --defaults-file="C:/ProgramData/MySQL/MySQL Server 5.7/my.ini" --initialize-insecure --user=mysql
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





## 数据表操作



### 增

INSERT INTO tbl SELECT fld FROM table

INSERT INTO db_nm (db_nm.fld, db_nm.fld2) SELECT field1,field2 FROM db2_name 



SELECT fld INTO tbl FROM table

REPLACE INTO test VALUES (1, 'New', '2014-08-20 18:47:42');

##### 参考:

[mysql数据库中插入数据INSERT INTO SET的优势](https://www.cnblogs.com/html55/p/9708475.html)

[mysql中insert into select from的使用](https://blog.csdn.net/NCTU_to_prove_safety/article/details/80520872)

[mysql INSERT INTO SELECT语句](https://blog.csdn.net/qq_38375394/article/details/79792250)



### 查

SELECT DISTINCT fld FROM tbl

SELECT fld AS '列 名',CASE WHEN age>22 THEN '大龄' ELSE '正常' END AS "年 龄",(CASE sex WHEN 1 THEN '男' WHEN 2 THEN '女' ELSE '未知' END) 性别

#### 连接
SELECT fld FROM tbl [INNER] JOIN table ON 
SELECT fld FROM tbl LEFT JOIN table ON
SELECT fld FROM tbl RIGHT JOIN table ON
SELECT fld FROM tbl FULL JOIN table ON
SELECT fld FROM tbl CROSS JOIN table ON

**别名**
SELECT fld FROM tbl A JOIN tbl B ON 

##### 参考：
[数据库表连接的简单解释](http://www.ruanyifeng.com/blog/2019/01/table-join.html)


### 改

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

##### 参考：

[MySQL有哪些索引类型 ?](https://segmentfault.com/q/1010000003832312)





## 数据类型

[MySQL 日期类型及默认设置](https://blog.csdn.net/gxy_2016/article/details/53436865)



## 函数



### 更新替换

replace(object,search,replace)

UPDATE `url_site_detail` SET pic = REPLACE(pic, 'http://www.myzyzy.com/Uploads/', 'http://pic.myzyzy.com/') WHERE `pic` REGEXP '^http://www.myzyzy.com/Uploads/vod/'



### 修剪

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



### 日期时间

NOW()

CURRENT_TIMESTAMP()

```sql
DATE_ADD(date,INTERVAL expr type)
```

##### 参考：

[MySQL：日期函数、时间函数总结](https://www.cnblogs.com/ggjucheng/p/3352280.html)

[MySQL DATE_ADD() 函数](http://www.w3school.com.cn/sql/func_date_add.asp)



#### 合并

CONCAT

CONCAT_WS

GROUP_CONCAT

**参考**：

[concat、concat_ws、group_concat函数用法](https://www.jianshu.com/p/ec9cc759e077)



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



# 大小写

#### 建表

```mysql
CREATE TABLE tbl_nm (
	fld_nm VARCHAR(10) BINARY
)
```

#### 查询

```mysql
SELECT * FROM tbl_nm WHERE BINARY fld_nm = 'URLNK'
```

#### 字符集

utf8_bin

#### 表名区分大小写

```ini
lower_case_table_names = 0
# 0 区分 1 不区分
```

#### 参考：

- [MySQL 字段中区分字符串大小写的解决方法](https://blog.mimvp.com/article/25443.html)



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
FLUSH PRIVILEGES;
```

my.ini 添加

```ini
[mysqld]
# 给数据库服务的唯一标识，一般为大家设置服务器 IP 的末尾号，每天实例的 server_id 都要不一样
server-id=4
log-bin=master-bin
log-bin-index=master-bin.index
## 以下配置不是必须的
# 复制类型：STATEMENT, ROW（保证数据一致）, MIXED（推荐）
binlog_format=MIXED
# 需要同步的数据库，如果没有本行即表示同步所有
binlog-do-db=DB1
binlog-do-db=DB2
# 忽略的数据库，和 binlog-do-db 为互斥关系，设置其一即可
binlog-ignore-db=mysql
binlog-ignore-db=information_schema
## global transaction identifier 全局事务 ID 效率高，不使用二进制日志文件名和偏移量
gtid_mode=on
enforce_gtid_consistency=on
# binlog，允许下端接入 slave，增大了从服务器的 IO 负载
log-slave-updates=1
##
sync-master-info=1
# 开启以保证不丢失数据，强制提交事务时，同步二进制日志
sync_binlog=1
# relay log
skip_slave_start=1
# 这样表示允许所有网段连接
bind-address=0.0.0.0
```

重启 MySQL 服务

查看日志

```mysql
SHOW MASTER STATUS;
```

```mysql
# 插入后刷新
FLUSH LOGS;
# 查看变动
SHOW binlog events \G;
# 确认 GTID 功能打开
show global variables like '%gtid%';
```



2. ### 配置 Slave 从服务器

my.ini 添加

```ini
[mysqld]
server-id=12
relay-log-index=slave-relay-bin.index
relay-log=slave-relay-bin
## 以下配置不是必须的
# 下面的配置可能导致无法启动 MySQL
master-host=192.168.100.4
master-port=3306
master-user=repl
master-password=pw
# 需要同步的数据库，如果没有本行即表示同步所有
replicate-do-db=DB2
# 忽略的数据库
replicate-ignore-db=mysql
# 并行
slave-parallel-type=LOGICAL_CLOCK
# 一般建议设置4-8，太多的线程会增加线程之间的同步开销
slave-parallel-workers=8
master_info_repository=TABLE
# 基于 binlog，保证 crash safe slave，配置下面 2 行（第二行都需要）
relay_log_info_repository=TABLE
relay_log_recovery=ON
#### GTID
gtid_mode=on
enforce_gtid_consistency=on
### binlog
## 级联同步设置必要参数
log-slave-updates=1
# 自动删除 binlog 文件
expire_logs_days=7
##
sync-master-info=1
# 基于 GTID，保证 crash safe slave，配置下面 2 行
sync_binlog=1
innodb_flush_log_at_trx_commit=1
## relay log
# slave 加上 skip_slave_start=1 避免启动后还使用老的复制协议
skip_slave_start=1
read_only=on
# 超时
slave_net_timeout=30
# 从库复制跳过错误
slave-skip-errors=1062,1053,1146,1213,1264,1205,1396
sql_mode=
```

重启 MySQL 服务

连接 Master

```mysql
change master to 
master_host='192.168.100.4', # Master 服务器 IP
master_port=3306,
master_user='repl',
master_password='mysql',
MASTER_AUTO_POSITION=1, # GTID 模式为 1（不需要下面 2 行），常规模式为 0
master_log_file='master-bin.000001', # Master 服务器产生的日志，参考上面的查看 Master 日志
master_log_pos=0
# 多源复制加上通信渠道
FOR CHANNEL 'master1';
```
启动 Slave
```mysql
start slave
# 单独启动加上需要同步的通道
FOR CHANNEL 'master1';

SHOW SLAVE STATUS
FOR CHANNEL 'master1'
\G;

# 查看 sql 慢查询语句
show processlist;
```

```mysql
# 跳过一步操作继续执行，将同步指针向下移动一个
stop slave;
set global sql_slave_skip_counter=1;
start slave;

### 跳过复制错误让复制继续下去
set sql_log_bin=0;
# 其他语句，例如：CREATE TABLE
set sql_log_bin=1;

## 修复 GTID 复制错误，执行出错的事务
stop slave;
set gtid_next='e10c75be-5c1b-11e6-ab7c-000c296078ae:6';
begin;
commit;
# gtid_next 方法一次只能跳过一个事务
set gtid_next='AUTOMATIC';
start slave;

### 从库 gtid_purged 设为和主库 gtid_executed 一样跳过不一致的 GTID 使复制继续下去
reset master;
set GLOBAL gtid_purged='e10c75be-5c1b-11e6-ab7c-000c296078ae:1-10';
show master status;
start slave;
```



## 常见问题

### Could not initialize master info structrue

```mysql
stop slave;
reset slave;
# 然后重复上面的查看 Master 日志，连接 Master
start slave;
```

### ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv option so it cannot execute this statement

```ini
secure-file-priv=""
```

##### 参考：

- [mysql5.7导出数据提示--secure-file-priv选项问题的解决方法](http://www.php.cn/mysql-tutorials-401995.html)



### 主从配置

1. 一主一从/一主多从

   级联复制解决多从对主库的压力，弊端是同步延迟比较大

2. 主主复制（双主复制）

   各自同步的数据库不同

3. 多主一从（多源复制）



### 变量

```mysql
show variables like '%relay%';
```

| 变量名                |                                                              |
| --------------------- | ------------------------------------------------------------ |
| max_relay_log_size    | 如为 0，则默认值为 max_binlog_size(1G)                       |
| relay_log_purge       | 自动清空不再需要，默认启用                                   |
| relay_log_recovery    | 保证 relay log 的完整性，建议开启                            |
| relay_log_space_limit | 中继日志最大限额。此设置存在主库崩溃，从库中继日志不全的情况，不推荐使用 |
| sync_relay_log        | 为 0 时，不马上刷入中继日志，安全性降低，但减少磁盘 I/O，建议采用默认值 |
| sync_relay_log_info   | 同上                                                         |



### 参考：

[MySQL Reference Manual / Replication](https://dev.mysql.com/doc/refman/5.7/en/replication.html)

[Mysql主从配置，实现读写分离](https://www.cnblogs.com/alvin_xp/p/4162249.html)

[MySQL主从复制与主主复制](https://www.cnblogs.com/phpstudy2015-6/p/6485819.html)

[【实操笔记】MySQL主从同步功能实现](https://www.cnblogs.com/hellxz/p/8378601.html)

[Mysql主从同步(1)-主从/主主环境部署梳理](https://www.cnblogs.com/kevingrace/p/6256603.html)

[Mysql主从同步（复制）](https://www.cnblogs.com/kylinlin/p/5258719.html)

[mysql主从复制实现数据库同步](https://www.cnblogs.com/rwxwsblog/p/4542417.html)

- [MySQL 主从复制原理不再难](https://www.cnblogs.com/rickiyang/p/13856388.html)
- [MySQL主从复制什么原因会造成不一致，如何预防及解决？](https://www.modb.pro/db/12026)
- [与MySQL传统复制相比，GTID有哪些独特的复制姿势?](https://dbaplus.cn/news-11-857-1.html)



# 读写分离

MySQL Router <- MySQL Proxy

[MyCat](https://github.com/MyCATApache/Mycat-Server) <- Cobar <-  [Amoeba](https://sourceforge.net/projects/amoeba/)

[Atlas](https://github.com/Qihoo360/Atlas)



##### 参考：
- [MySQL读写分离](https://www.cnblogs.com/phpstudy2015-6/p/6687480.html)
- [MySQL Router](https://www.jianshu.com/p/7fc8d77bea59)



# 索引优化

##### 参考：

- [一个案例彻底弄懂如何正确使用 mysql inndb 联合索引](https://mengkang.net/1302.html)
- [MySQL索引总结](https://zhuanlan.zhihu.com/p/29118331)



### 组合索引

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

##### 参考：

[MySQL使用组合索引](http://www.luckybird.me/mysql%e4%bd%bf%e7%94%a8%e7%bb%84%e5%90%88%e7%b4%a2%e5%bc%95.html)



### 全文索引

|            | MyISAM | InnoDB |
| ---------- | ------ | ------ |
| 5.6 以前   | 是     | 否     |
| 5.6 及以后 | 是     | 是     |

从 MySQL 5.7.6 开始，MySQL内置了 ngram 全文解析器，用来支持中文、日文、韩文分词

#### 全文检索分词数

```
n=1: '生', '日', '快', '乐' 
n=2: '生日', '日快', '快乐' 
n=3: '生日快', '日快乐' 
n=4: '生日快乐'
```

1. 启动mysqld命令时

```
mysqld --ngram_token_size=2
```

2. 修改MySQL配置文件

```ini
[mysqld]
ngram_token_size=2
```
```ini
# 单个汉字
ngram_token_size=1
ft_min_word_len=1
```
命令查看

```mysql
show variables like '%ft%';
# 修改完参数以后，一定要修复下索引
repair table test quick;
```
```ini
# MyISAM
ft_min_word_len = 4;
ft_max_word_len = 84;

# InnoDB
innodb_ft_min_token_size = 3;
innodb_ft_max_token_size = 84;
```




#### 创建全文索引

字段类型为 CHAR、VARCHAR 或者 TEXT

InnoDB 和 MyISAM 引擎

MATCH (title,body) 字段名一致 ft_index (title,body)

不能够跨多个表进行检索

先导入数据再在表上创建全文索引的方式快

测试表里至少要有 4 条以上的记录

```mysql
CREATE FULLTEXT INDEX ft_index ON articles (title,body) WITH PARSER ngram;
```

```mysql
ALTER TABLE articles ADD FULLTEXT INDEX ft_index (title,body) WITH PARSER ngram;
```

```mysql
CREATE TABLE articles (
    id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
    title VARCHAR (200),
    body TEXT,
    FULLTEXT (title, body) WITH PARSER ngram
) ENGINE = INNODB;
```

#### 全文检索模式

|              |                       |      | 操作符等复杂查询 |
| ------------ | --------------------- | ---- | ---------------- |
| 自然语言模式 | NATURAL LANGUAGE MODE | 默认 | 否               |
| 布尔         | BOOLEAN MODE          |      | 是               |
|              | QUERY EXPANSION       |      |                  |

```mysql
# 获取相关性的值，0 表示无相关性
SELECT id,title,
MATCH (title,body) AGAINST ('手机' IN NATURAL LANGUAGE MODE) AS score
FROM articles
ORDER BY score DESC;
```
运算符的使用方式：
```
'apple banana' 
无操作符，表示或，要么包含apple，要么包含banana

'+apple +juice'
必须同时包含两个词

'+apple macintosh'
必须包含apple，但是如果也包含macintosh的话，相关性会更高。

'+apple -macintosh'
必须包含apple，同时不能包含macintosh。

'+apple ~macintosh'
必须包含apple，但是如果也包含macintosh的话，相关性要比不包含macintosh的记录低。

'+apple +(>juice <pie)'
查询必须包含apple和juice或者apple和pie的记录，但是apple juice的相关性要比apple pie高。

'apple*'
查询包含以apple开头的单词的记录，如apple、apples、applet。

'"some words"'
精确匹配，不分词
使用双引号把要搜素的词括起来，效果类似于like '%some words%'，
例如“some words of wisdom”会被匹配到，而“some noise words”就不会被匹配。
```



```mysql
select title from page where match(title) against('bmw') limit 10;
select title from page where match(title) against('*bm*' in boolean mode) limit 3;
select title from page where match(title) against('+meizu -pro' in boolean mode) limit 3;
```

##### 参考：

[MySQL 5.7 中文全文检索使用教程](https://www.jianshu.com/p/c48106149b6a)



# 查询优化和存储优化

##### 参考：

- [MySQL查询优化](https://www.cnblogs.com/phpstudy2015-6/p/6509331.html)
- [MySQL 优化](https://github.com/judasn/Linux-Tutorial/blob/master/markdown-file/Mysql-Optimize.md)
- [Mysql 大表分页查询优化（一）](https://mengkang.net/1299.html)



(NOT) EXISTS 替代 (NOT) IN



# 批量插入

```sh
mysql -uroot -D toutiao -p < article1000.sql
```

[MYSQL批量插入数据库实现语句性能分析](https://www.cnblogs.com/caicaizi/p/5849979.html)

[MYSQL开发性能研究——批量插入的优化措施](https://www.cnblogs.com/aicro/p/3851434.html)



# 派生版本

MariaDB

Percona Server for MySQL



# 事务

- 脏读
- 幻读
- 不可重复读