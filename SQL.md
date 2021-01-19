# SQL

Structured Query Language 结构化查询语言



#### RDBMS

Relational Database Management System 关系型数据库管理系统



## 目录



## 数据库

```mysql
# 选择数据库
use db_name;

# 设置使用的字符集
set names utf8;

# 创建数据库
create database dbname;

# 修改数据库
alter database
```



## 表

```mysql
CREATE TABLE table_name
(
    primary_key int NOT NULL,
	column_name1 data_type(size) constraint_name,
    col_nm varchar(255) NOT NULL,
    col_n2m varchar(255) DEFAULT 'default_value'|GETDATE(),
    UNIQUE (column_name),
    CONSTRAINT index_name UNIQUE|PRIMARY KEY (col1, col2),
    PRIMARY KEY (primary_key),
    [CONSTRAINT index_name] FOREIGN KEY (col_nm) REFERENCES table_name2(col_nm),
    [CONSTRAINT index_name] CHECK (primary_key > 0 AND col_nm = 'value')
);

create table newtable
AS
SELECT * FROM oldtable;

alter table table_name
MODIFY column_name data_type(size) constraint_name;

ALTER TABLE table_name
ALTER column_name SET DEFAULT 'default_value';

ALTER TABLE table_name
ADD UNIQUE (column_name);

ALTER TABLE table_name
DROP INDEX|FOREIGN KEY|CHECK index_name;

ALTER TABLE table_name
DROP PRIMARY KEY;

ALTER TABLE table_name
ALTER column_name DROP DEFAULT;

drop table
```



### 约束 Constraints

|             |                           |      |
| ----------- | ------------------------- | ---- |
| NOT NULL    | 不能存储 NULL             |      |
| UNIQUE      |                           |      |
| PRIMARY KEY | NOT NULL 和 UNIQUE 的结合 |      |
| FOREIGN KEY |                           |      |
| CHECK       |                           |      |
| DEFAULT     |                           |      |



## 索引

```mysql
create [UNIQUE] index index_name
ON table_name (column_name1, column_name2);

drop index
```



## 增查改删

```mysql
insert into table_name
VALUES (value1, value2, ...);

insert into table_name (column1, column2, ...)
VALUES (value1, value2, ...);

INSERT INTO table2 [(column_name)]
SELECT *, column_name FROM table1;

select *, column_name, column_name AS alias_name
from table_name [AS] alias_name
WHERE column_name operator value
ORDER BY column_name, column_name ASC|DESC
LIMIT number;

# 列值不重复
select DISTINCT column_name, column_name
from table_name;

SELECT *
INTO 

update table_name
SET column1 = value1, column2 = value2, ...
[WHERE some_column = some_value];

delete FROM table_name
[WHERE some_column = some_value];

DELETE [*] FROM table_name;
```



### 运算符

| 运算符  |      | 描述           | 示例                      |
| ------- | ---- | -------------- | ------------------------- |
| =       |      | 等于           |                           |
| <>      | !=   |                |                           |
| >       |      |                |                           |
| <       |      |                |                           |
| >=      |      |                |                           |
| <=      |      |                |                           |
| BETWEEN |      | 范围内         | BETWEEN value1 AND value2 |
| LIKE    |      |                | LIKE 'prefix%'            |
| IN      |      | 指定列的多个值 | IN (value1, value2, ...)  |
| AND     |      |                |                           |
| OR      |      |                |                           |
| NOT     |      |                |                           |
| REGEXP  |      |                |                           |



### 通配符

| 通配符                     | 描述                | 示例           |
| -------------------------- | ------------------- | -------------- |
| %                          | 替代 0 个或多个字符 |                |
| _                          | 替代一个字符        |                |
| [charlist]                 | 列中的任一字符      | [abc] 或 [a-c] |
| [^charlist] 或 [!charlist] | 不在列中的任一字符  |                |



### 连接

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



### 联合

```mysql
# UNION ALL 允许重复的值
SELECT column_name
FROM table1
UNION [ALL]
SELECT column_name
FROM table12;
```

