# SQL

Structured Query Language 结构化查询语言



**RDBMS**

Relational Database Management System 关系型数据库管理系统



## 数据库

```mysql
# 选择数据库
use db_name;

# 设置使用的字符集
set names utf8;
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



#### 自动递增

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



## 视图



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
DROP INDEX index_name;

ALTER TABLE table_name
DROP FOREIGN KEY|CHECK index_name;
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
ORDER BY column_name, column_name ASC|DESC
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
