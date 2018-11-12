MySQL
======



# 下载

https://dev.mysql.com/downloads/mysql/
-> https://dev.mysql.com/downloads/windows/installer/5.7.html
--> https://dev.mysql.com/downloads/file/?id=467606
---> https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.17.0.msi

https://dev.mysql.com/downloads/mysql/8.0.html
-> https://dev.mysql.com/downloads/windows/installer/8.0.html

*https://dev.mysql.com/downloads/installer/



# 安装 MySQL

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
```



## 帮助

```sh
mysql --help
```



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