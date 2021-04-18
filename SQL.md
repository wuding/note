# SQL

Structured Query Language 结构化查询语言



**RDBMS**

Relational Database Management System 关系型数据库管理系统



## 数据库

```mysql
# 选择数据库
USE db_name;

# 设置使用的字符集
SET names utf8;
```



### 创建

```sql
CREATE DATABASE dbname
```



### 修改

```sql
ALTER DATABASE
```



### 删除

```sql
DROP DATABASE dbname;
```



## 表

### 创建

```sql
CREATE TABLE table_name
(
    -- SQL Server / Oracle 直接使用 UNIQUE 和 PRIMARY KEY, FOREIGN KEY, CHECK
    primary_keyname int NOT NULL AUTO_INCREMENT
     [UNIQUE|PRIMARY KEY|FOREIGN KEY REFERENCES tbl_nm2(col_nm)],
	column_name1 data_type(size) constraint_name,
    col_nm varchar(255) NOT NULL,
    -- 默认值
    col_nm2 varchar(255) DEFAULT 'default_value'|GETDATE(),
    -- 索引
    UNIQUE (column_name),
    -- 多列
    CONSTRAINT index_name UNIQUE|PRIMARY KEY (col1, col2),
    -- 主键
    PRIMARY KEY (primary_key),
    -- 通过 CONSTRAINT 定义多列
    -- 外键
    [CONSTRAINT index_name] FOREIGN KEY (col_nm) REFERENCES table_name2(col_nm),
    -- 限制
    [CONSTRAINT index_name] CHECK (primary_key > 0 AND col_nm = 'value')
);
```



#### 拷贝

```sql
CREATE TABLE new_table
AS
SELECT * FROM old_table;
```



### 修改

```sql
ALTER TABLE table_name
ADD|MODIFY column_name data_type(size) constraint_name;

-- 修改数据类型
ALTER TABLE table_name
ALTER COLUMN column_name data_type(size) constraint_name;
```



#### 自动增量

```sql
ALTER TABLE table_name
AUTO_INCREMENT = number;
```



#### 默认值

```sql
-- 设置
ALTER TABLE table_name
ALTER column_name SET DEFAULT 'default_value';

-- 撤销
ALTER TABLE table_name
ALTER column_name DROP DEFAULT;
```



### 删除

```sql
DROP TABLE table_name;
```



#### 删除列

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```



### 清空

```sql
TRUNCATE TABLE table_name;
```



### 约束 Constraints

| 约束        | 描述                      |
| ----------- | ------------------------- |
| NOT NULL    | 不能存储 NULL             |
| UNIQUE      | 唯一得值                  |
| PRIMARY KEY | NOT NULL 和 UNIQUE 的结合 |
| FOREIGN KEY | 外键                      |
| CHECK       | 值符合条件                |
| DEFAULT     | 默认值                    |



### 通用数据类型

#### 字符串

| 数据类型     | 描述       | 备注            |
| ------------ | ---------- | --------------- |
| CHARACTER(n) | 固定长度 n | 最多 255 个字符 |
| CHAR         | 同上       |                 |
| VARCHAR(n)   | 最大长度 n | 最多 255 个字符 |



#### 二进制串

| 数据类型     | 描述       |
| ------------ | ---------- |
| BINARY(n)    | 固定长度 n |
| VARBINARY(n) | 最大长度 n |



#### 布尔

| 数据类型 | 描述            |
| -------- | --------------- |
| BOOLEAN  | TRUE 或者 FALSE |



#### 整数

| 数据类型    | 描述           |
| ----------- | -------------- |
| INTERGER(p) | 精度 p 默认 10 |
| SMALLINT    | 精度 5         |
| BIGINT      | 精度 19        |



#### 精确数值

| 数据类型      | 描述            |
| ------------- | --------------- |
| DECIMAL(p, s) | 精度 p 小数位 s |
| NUMERIC       | 精度 5          |



#### 近似数值

| 数据类型        | 描述                   |
| --------------- | ---------------------- |
| FLOAT(p)        | 尾数精度 p 默认 16     |
| DOUBLE(size, d) | size 最大位数 d 小数位 |
| REAL            | 尾数精度 7             |



#### 日期时间

| 数据类型  | 描述            |
| --------- | --------------- |
| DATE      | 精度 p 小数位 s |
| TIME      | 精度 5          |
| TIMESTAMP | 年月日时分秒    |



#### 其他

| 数据类型 | 描述               |
| -------- | ------------------ |
| INTERVAL | 区间               |
| ARRAY    | 固定长度的有序集合 |
| MULTISET | 年月日时分秒       |
| XML      | 存储 XML           |



### MySQL 数据类型

#### 文本

| 数据类型   | 描述                      |
| ---------- | ------------------------- |
| TINYTEXT   | 最多 255 个字符           |
| TEXT       | 最多 65535 个字符         |
| MEDIUMTEXT | 最多 16,777,215 个字符    |
| LONGTEXT   | 最多 4,294,967,295 个字符 |



#### 二进制串

| 数据类型   | 描述                    |
| ---------- | ----------------------- |
| BLOB       | 最多 65535 字节         |
| MEDIUMBLOB | 最多 16,777,215 字节    |
| LONGBLOB   | 最多 4,294,967,295 字节 |



#### 枚举列表

| 数据类型            | 描述               |
| ------------------- | ------------------ |
| ENUM(x, y, z, etc.) | 最多 65535 个值    |
| SET                 | 最多 64 个列表项目 |



#### 整数

| 数据类型         | 描述                                                         | 备注         |
| ---------------- | ------------------------------------------------------------ | ------------ |
| TINYINT          | 带符号 -128 到 127，无符号 0 至 255                          |              |
| SMALLINT(size)   | 带符号 -32,768 到 32,767，无符号 0 至 65,535                 | size 默认 6  |
| MEDIUMNINT(size) | 带符号 -8,388,608 到 8,388,607，无符号 0 至 16,777,215       | size 默认 9  |
| INT(size)        | 带符号 -2,147,483,648 到 2,147,483,647，<br/>无符号 0 至 4,294,967,295 | size 默认 11 |
| BIGINT(size)     | 带符号 -9,223,372,036,854,775,808 <br/>到 9,223,372,036,854,775,807，<br>无符号 0 至 18,446,744,073,709,551,615 | size 默认 20 |

例如值为 10

int(9) 显示结果为 000000010

int(3) 显示结果为 010

显示长度不一样，都占用 4 个字节



#### 日期时间

| 数据类型  | 描述                                           |
| --------- | ---------------------------------------------- |
| DATE      | 1000-01-01 至 9999-12-31                       |
| DATETIME  | 同上                                           |
| TIMESTAMP | UTC 1970-01-01 00:00:01 至 2038-01-09 03:14:07 |
| TIME      | -838:59:59 至 838:59:59                        |
| YEAR      | 4 位 1901 至 2155，2 位 (19)70 至 (20)69       |




## 视图

### 创建更新

```sql
CREATE [OR REPLACE] VIEW view_name AS
SELECT column_name
FROM table_name
WHERE condition;
```



### 删除

```sql
DROP VIEW view_name;
```



## 索引

### 创建

```sql
CREATE [UNIQUE] INDEX index_name
ON table_name (column_name1, column_name2);
```



#### 唯一值

```sql
ALTER TABLE table_name
ADD UNIQUE (column_name);
```



### 修改



### 删除

```sql
ALTER TABLE table_name
DROP INDEX|FOREIGN KEY|CHECK index_name;
```



#### 主键

```sql
-- MySQL
ALTER TABLE table_name
DROP PRIMARY KEY;

-- SQL Server / Oracle
ALTER TABLE table_name
DROP CONSTRAINT index_name;
```



## 增查改删

### 增

```sql
-- 无需指定列名
INSERT INTO table_name
VALUES (value1, value2, ...);

-- 指定列名
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);

-- 使用 SET
INSERT INTO table_name
SET column1 = value1, column2 = value2;
```



#### 插入选择

```sql
INSERT INTO table2 [(column_name)]
SELECT *, column_name FROM table1;
```



#### 选择插入

```sql
-- 没有数据返回则创建空表
SELECT *
INTO new_table
FROM old_table
[WHERE 1 = 0];
```



### 查

```sql
SELECT [DISTINCT] *, column_name, column_name AS alias_col,
CONCAT(column1, '_', column2) AS alias_column
FROM table_name [AS] alias_tbl
WHERE alias_tbl.column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value
ORDER BY column_name, column_name [ASC|DESC]
LIMIT number;
```



#### 数目

```sql
-- MySQL
SELECT column_name
FROM table_name
LIMIT [offset, ]number;

-- SQL Server
SELECT TOP number PERCENT column_name
FROM table_name;

-- Oracle
SELECT column_name
FROM table_name
WHERE ROWNUM <= number;
```



#### 运算符

| 运算符  |      | 描述           | 示例                      |
| ------- | ---- | -------------- | ------------------------- |
| =       |      | 等于           |                           |
| <>      | !=   | 不等于         |                           |
| >       |      | 大于           |                           |
| <       |      | 小于           |                           |
| >=      |      | 大于等于       |                           |
| <=      |      | 小于等于       |                           |
| BETWEEN |      | 范围内         | BETWEEN value1 AND value2 |
| LIKE    |      | 搜索           | LIKE 'prefix%'            |
| IN      |      | 指定列的多个值 | IN (value1, value2, ...)  |
| AND     |      | 并             |                           |
| OR      |      | 或             |                           |
| NOT     |      |                |                           |
| REGEXP  |      |                | REGEXP '^[ABc]'           |



#### 通配符

| 通配符                     | 描述                | 示例           |
| -------------------------- | ------------------- | -------------- |
| %                          | 替代 0 个或多个字符 |                |
| _                          | 替代一个字符        |                |
| [charlist]                 | 列中的任一字符      | [abc] 或 [a-c] |
| [^charlist] 或 [!charlist] | 不在列中的任一字符  |                |



#### 操作符

| 操作符        | 描述        |
| ------------- | ----------- |
| IS [NOT] NULL | 是否为 NULL |
| [NOT] EXISTS  | 是否有记录  |



#### 连接

```mysql
SELECT *
FROM table_name
INNER JOIN tbl_name
ON tbl_name.col = table_name.column_name;
```

| 连接               | 描述               |
| ------------------ | ------------------ |
| [INNER] JOIN       | 表中有至少一个匹配 |
| LEFT [OUTER] JOIN  | 即使右表没有匹配   |
| RIGHT [OUTER] JOIN | 即使左表没有匹配   |
| FULL [OUTER] JOIN  | 其中一个表存在匹配 |



#### 联合

```sql
-- UNION ALL 允许重复的值
SELECT column_name
FROM table1
WHERE ...
UNION [ALL]
SELECT column_name
FROM table12
WHERE ...
ORDER BY column_name;
```



### 改

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
[WHERE some_column = some_value];
```



### 删

```sql
DELETE [*] FROM table_name
[WHERE some_column = some_value];
```



## 函数

### 日期

| 函数名                                 | 描述             | 示例                           |
| -------------------------------------- | ---------------- | ------------------------------ |
| NOW()                                  | 当前的日期和时间 | 2021-04-18 06:41:49            |
| CURDATE()                              | 当前的日期       | 2021-04-18                     |
| CURTIME()                              | 当前的时间       | 06:41:49                       |
| DATE(datetime)                         | 提取日期         |                                |
| EXTRACT(unit FROM datetime)            | 返回部分         |                                |
| DATE_ADD(datetime, INTERVAL expr type) | 添加时间间隔     |                                |
| DATE_SUB(datetime, INTERVAL expr type) | 减去时间间隔     |                                |
| DATEDIFF(date1, date2)                 | 时间差           |                                |
| DATE_FORMAT(date, format)              | 格式化           | DATE_FORMAT(NOW(), '%Y-%m-%d') |



#### 单位和类型

| Unit / Type | 描述 |
| ----------- | ---- |
| YEAR        |      |
| MONTH       |      |
| DAY         |      |



#### 格式

| 格式 | 描述                   |
| ---- | ---------------------- |
| a    | 缩写星期名             |
| b    | 缩写月名               |
| c    | 月，数值               |
| D    | 带有英文前缀的月中的天 |
| d    | 月的天，数值（00-31）  |



### 检测

| 函数名                           | 备注       |
| -------------------------------- | ---------- |
| IFNULL(old_value, default_value) |            |
| COALESCE()                       | 同上       |
| ISNULL()                         | SQL Server |
| NVL()                            | Oracle     |



### 聚合

| 函数名                        | 描述                    |
| ----------------------------- | ----------------------- |
| AVG(column_name)              | 平均值                  |
| COUNT([DISTINCT] column_name) | 匹配的行数，NULL 不计入 |
| FIRST(column_name)            | 仅 Access 支持          |
| LAST                          | 同上                    |
| MAX(column_name)              | 最大值                  |
| MIN(column_name)              | 最小值                  |
| SUM(column_name)              | 总和                    |



### 标量

| 函数名                             | 描述                     |
| ---------------------------------- | ------------------------ |
| UCASE(column_name)                 | 转为大写                 |
| LCASE                              | 转为小写                 |
| MID(column_name, start [, length]) | 提取字符，start 起始值 1 |
| SubString                          |                          |
| LENGTH(column_name)                | 长度                     |
| ROUND(column_name，decimals)       | 小数位，默认保留 0 位    |
| NOW                                |                          |
| FORMAT(column_name，format)        |                          |
