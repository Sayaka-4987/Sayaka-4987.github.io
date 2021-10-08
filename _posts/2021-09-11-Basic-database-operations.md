---
layout:     post   				        # 使用的布局（不需要改）
title:      数据库常用命令 				# 标题 
subtitle:   从删库到跑路					# 副标题
date:       2021-09-11 				    # 时间
author:     YXWang 					    # 作者
header-img: img/post-bg-database.jpg	# 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - 数据库系统原理
---

# 数据库常用命令

写在最前面：

- 转载自各种网络教程，整理自用；
- 还是老一套免责声明：我太菜了，没法保证内容不出错，欢迎各位神仙大佬拍砖；
- 若出现报错 `ERROR 1064` 请首先注意关键字的字母 **拼写问题**；
- SQL 的关键字默认不区分大小写，本文只是为了 **突出显示哪些是 SQL 的关键字** 才采用大写~~（可把我别扭坏了）~~；



## 数据定义语言（DDL）

| 操作对象 | 创建            | 删除          | 修改           |
| -------- | --------------- | ------------- | -------------- |
| 模式     | `CREATE SCHEMA` | `DROP SCHEMA` | SQL 标准不提供 |
| 表       | `CREATE TABLE`  | `DROP TABLE`  | `ALTER TABLE`  |
| 视图     | `CREATE VIEW`   | `DROP VIEW`   | SQL 标准不提供 |
| 索引     | `CREATE INDEX`  | `DROP INDEX`  | `ALTER INDEX`  |



### 模式的定义和删除

在 MySQL 中，SCHEMA 和 DATABASE 是等价的，其他产品中 SCHEMA 一般是被 DATABASE 包含；

#### 创建模式 `CREATE SCHEMA <模式名> AUTHORIZATION <用户名>` 

例：

```sql
CREATE SCHEMA "S-T" AUTHORIZATION WANG; 
```

- 只有具有数据库管理员权限的用户才能创建模式
- 若不指定模式名，模式名隐含为用户名
- 定义模式是定义了一个命名空间
- 允许在定义模式的子句中创建基本表、视图、定义授权
  `CREATE SCHEMA <模式名> AUTHORIZATION  <用户名> [<表定义>|<视图定义>|<授权定义>]` 
  就是 <用户名> 后先不接分号，而是表定义、视图定义的语句；



#### 删除模式 `DROP SCHEMA <模式名> <CASCADE|RESTRICT>`  

例：

```sql
DROP SCHEMA ZAHNG RESTRICT
```

- CASCADE 和 RESTRICT 必选其一：
  - CASCADE（级联）表示删除模式时把该模式中所有数据库对象也全部删除；
  - RESTRICT（限制）表示如果该模式下已有数据库对象，则拒绝该删除语句的执行；



### 基本表的定义、删除与修改

#### 创建基本表 `CREATE TABLE` 

基本格式：

```sql
CREATE TABLE <表名> (
    <列名><数据类型> [列级完整性约束条件], 
    <列名><数据类型> [列级完整性约束条件], 
    <列名><数据类型> [列级完整性约束条件], 
    ...
    [表级完整性约束条件]
); 
```

例：

```sql
CREATE TABLE Course (
    Cno CHAR(4) PRIMARY KEY, 	-- 列级完整性约束条件：Cno 是主码
    Cname CHAR(40) NOT NULL, 	-- 列级完整性约束条件：Cname 不能为空
    Cpno CHAR(4), 
    Ccredit SMALLINT, 
    FOREIGN KEY (Cpno) REFERENCES Course(Cno) 	-- 表级完整性约束条件：Cpno是外码，它参照表是自身
);
```

- 完整性约束条件被存在数据字典中
- 涉及多个属性列的完整性约束条件 **必须定义在表级**
- 保留字不能用作表名、列名
- 参照表和被参照表可以是同一个表
- 常用的完整性约束：
  - 主码约束 `PRIMARY KEY` 
  - 唯一性约束 `UNIQUE` 
  - 非空值约束 `NOT NULL` 
  - 参照完整性约束 `FOREIGN KEY REFERENCES` 
  - 检查子句 `CHECK` （例：`CHECK ((grade IS NULL) OR (grade BETWEEN 0 AND 100))` ）



#### 定义基本表所属模式的三种方法

每一个基本表都属于某一个模式，一个模式包含多个基本表；

- 方法一：在表名中显式给出模式名 
- 方法二：在创建模式语句中同时创建表 
- 方法三：设置所属的模式 



#### SQL 通用数据类型表 

| 数据类型                           | 描述                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| CHARACTER(n)                       | 固定长度 n 的字符/字符串，                                   |
| VARCHAR(n) 或 CHARACTER VARYING(n) | 最大长度 n 的字符/字符串，可变长度                           |
| BINARY(n)                          | 固定长度 n 的二进制串                                        |
| BOOLEAN                            | 存储 TRUE 或 FALSE 值                                        |
| VARBINARY(n) 或 BINARY VARYING(n)  | 最大长度 n 的二进制串，可变长度                              |
| INTEGER(p)                         | 精度 p 的整数值（没有小数点）                                |
| SMALLINT                           | 精度 5 的整数值（没有小数点）                                |
| INTEGER                            | 精度 10 的整数值（没有小数点）                               |
| BIGINT                             | 精度 19 的整数值（没有小数点）                               |
| DECIMAL(p,s)                       | 精度 p，小数点后位数 s 的精确数值<br>例如：decimal(5,2) 是一个小数点前有 3 位数，小数点后有 2 位数的数字 |
| NUMERIC(p,s)                       | 精度 p，小数点后位数 s 的精确数值（与 DECIMAL 相同）         |
| FLOAT(p)                           | 尾数精度 p ，近似数值<br/>采用以 10 为基数的指数计数法的浮点数<br/>该类型的 size 参数由一个指定最小精度的单一数字组成 |
| REAL                               | 尾数精度 7 的近似数值                                        |
| FLOAT                              | 尾数精度 16 的近似数值                                       |
| DOUBLE PRECISION                   | 尾数精度 16 的近似数值                                       |
| DATE                               | 日期类型，存储年、月、日的值，格式 `YYYY-MM-DD`              |
| TIME                               | 时间类型，存储小时、分、秒的值，格式 `HH:MM:SS`              |
| TIMESTAMP                          | 时间戳类型，存储年、月、日、小时、分、秒的值                 |
| INTERVAL                           | 时间间隔类型，由一些整数字段组成，代表一段时间，取决于区间的类型 |
| ARRAY                              | 固定长度的有序集合                                           |
| MULTISET                           | 可变长度的无序集合                                           |
| XML                                | 存储 XML 数据                                                |



#### 删除基本表 `DROP TABLE <CASCADE|RESTRICT>` 

CASCADE 和 RESTRICT 必选其一：

- CASCADE（级联）表示删除该表时把相关的依赖对象（例如视图）同时删除；
- RESTRICT（限制）表示如果还有视图、触发器、存储过程、函数依赖该表时，则拒绝删除操作；

 

#### 修改基本表 `ALTER TABLE` 

基本格式：

```sql
ALTER TABLE <表名>
    [ADD [COLUMN] <新列名><数据类型> [完整性约束]]
    [ADD <新表级完整性约束>]
    [DROP [COLUMN] <要删除的列名> [CASCADE|RESTRICT]]
    [DROP CONSTRAINT <要删除的完整性约束名> [RESTRICT|CASCADE]]
    [ALTER COLUMN <要修改的列名><数据类型>];
```

例：
```sql
ALTER TABLE course ADD UNIQUE(course_name)
```



### 索引的建立、删除

#### 建立索引  `CREATE [UNIQUE] [CLUSTER] INDEX <索引名> ON <表名>(<列名>[<次序>], ... ); `

例：

```sql
CREATE UNIQUE INDEX SCno ON SC(Sno ASC, Cno DESC)
```

- 索引由数据库管理员或表的创建者按需建立，能加快数据库查询速度
- 但索引需要占用存储空间，会降低数据库增加、修改数据的速度
- 索引需要随着基本表更新而维护
- 有些 DBMS 会自动为 PRIMARY KEY 和 UNIQUE 创建索引









## 对数据库的操作

### `CREATE` 创建

#### 新建数据库

```sql
CREATE DATABASE 数据库名；
```

可后接库选项：

```sql
CREATE DATABASE 
IF NOT EXISTS 数据库名 	-- 如果数据库不存在则创建，存在则不创建
DEFAULT CHARSET utf8 COLLATE utf8_general_ci;	-- 设定编码集为utf8
```



### `DROP` 删除

```sql
DROP DATABASE 数据库名;
```



### `SHOW` 查询

#### 查看所有的数据库

注意查询这里的 `DATABASES` 是复数形式，SQL 关键字加不加 s 的规律和英语的自然语法一样：

```sql
SHOW DATABASES;
```

#### 模糊查询指定的数据库

```sql
SHOW DATABASES LIKE "pattern";
```

- `_` 表示匹配单个字符（暂时没发现这个怎么写才对？）
- `%` 表示匹配多个字符，使用示例如下：

```sql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> SHOW DATABASES LIKE "%schema%";
+---------------------+
| Database (%schema%) |
+---------------------+
| information_schema  |
| performance_schema  |
+---------------------+
2 rows in set (0.00 sec)
```



### `ALTER` 修改

数据库的名字不可修改，仅允许修改字符集（CHARSET）和校对集（COLLATE）选项：

```sql
ALTER DATABASE 数据库名 [库选项名=值];
```



使用示例：

```sql
mysql> ALTER DATABASE RUNOOB CHARSET = gbk;
Query OK, 1 row affected (0.02 sec)
```



## 对表的操作

### `CREATE` 创建

#### 通用语法

需要先 `use 数据库名` 进入一个数据库：

```sql
CREATE TABLE 表名 (字段名 字段类型);
```



也可以用 `.` 运算符将数据库表创建到指定的数据库下:

```sql
CREATE TABLE 数据库名.表名 (字段名 字段类型);
```



#### 设置更多属性

注意这里都不是单引号，是键盘上 1 左边那个 \` 反单引号（backquote），否则会报错 `ERROR 1064 (42000): You have an error in your SQL syntax；`

```sql
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,	-- 设置该列为自增，一般用于主键，数值会自动加 1 
   `runoob_title` VARCHAR(100) NOT NULL,	-- 设置字段的属性为 NOT NULL, 在操作数据库时如果输入该字段的数据为 NULL, 就会报错
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )				-- 设置主键
)ENGINE=InnoDB DEFAULT CHARSET=utf8;		-- ENGINE 设置存储引擎，CHARSET 设置编码
```



### `DROP` 删除

可以一次性删除多张表:

```sql
drop table 表名1,表名2....;	
```



### `SHOW` 查询

需要先 `use 数据库名` 进入一个数据库；

#### 查看所有表

```sql
SHOW TABLES;
```

#### 模糊查询指定的表

pattern 模式写法和对数据库一样；

- `_` 表示匹配单个字符（暂时没发现这个怎么写才对？）
- `%` 表示匹配多个字符

```sql
SHOW TABLES LIKE "pattern";
```



### `DESC` 查看表中的字段信息 

`DESC` 、 `DESCRIBE` 、 `SHOW`  三个关键字均可，但写法有细微区别：

```sql
DESC 表名;

DESCRIBE 表名;

SHOW COLUMNS FROM 表名;
```



### `ALTER` 修改

需要先 `use 数据库名` 进入一个数据库，或把 `表名` 换成能指定操作的表在哪个数据库里的 `数据库名.表名` ：

#### 修改表名和选项

改名：

```sql
RENAME TABLE 旧表名 TO 新表名;
```

修改属性选项：

```sql
ALTER TABLE 表名 CHARSET=GBK;
```

#### 增加字段

```sql
ALTER TABLE 表名 ADD COLUMN [字段名] [数据类型] [列属性] [位置];
```



示例，增加名为 runoob_likes 的字段，位置默认是 `AFTER` 最后一个字段：

```sql
ALTER TABLE runoob_table ADD COLUMN runoob_likes int unsigned;
```

#### 修改字段

通常是修改属性或者修改数据类型：

```sql
ALTER TABLE 表名 MODIFY [字段名] [数据类型] [列属性];
```

#### 重命名字段

使用 `CHANGE` 关键字：

```sql
ALTER TABLE 表名 CHANGE [旧字段] [新字段名] [新数据类型(必选)] [属性];
```

#### 删除字段

使用 `DROP` 关键字：

```sql
ALTER TABLE 表名 DROP [字段名]
```



## 对表中信息的操作

### `INSERT` 插入数据

```sql
INSERT INTO 表名 (字段名1, 字段名2 ...) VALUES (值1, 值2, 值3 ..);
```

可以批量写入，也可以不给出所有值，示例：

```sql
INSERT INTO product (pname, price) VALUES
	('智能机器人',25999.22),
	('彩色电视',1250.36),
	('沙发',58899.02)

INSERT INTO runoob_table (runoob_title, runoob_author, submission_date) VALUES
    ("Hello MySQL!", "Alice", curtime());
```

### 删除表中数据

#### `DELETE` 关键字

`DELETE` 会一条一条删除指定的数据，而且不会清空自动自增的 `auto_increment` 记录数；

```sql
DELETE FROM 表名 [WHERE 条件];
```

#### `TRUNCATE` 关键字

`TRUNCATE` 会直接将表删除，重新建表：

```sql
TRUNCATE TABLE 表名;
```



### `SELECT` 查询数据

通用语法：

```sql
SELECT [字段名列表，或 * ] FROM 表名 [WHERE 条件] [LIMIT N] [OFFSET M]
```

#### `WHERE` 关键词指定查询条件

- 支持 "where 列名 = 值" 这种名等于值的查询形式；
- 支持一般的比较运算符，例如  =、>、<、>=、<、!= ，以及一些扩展运算符 is [not] null、in、like 等；
- 可以对查询条件使用 or 和 and 进行  **组合查询** ；

```sql
SELECT * FROM students WHERE name="张三";

SELECT * FROM students WHERE id<5 AND age>20; 　-- 查询id小于5且年龄大于20的所有人信息:
```



### `UPDATE` 更新数据

通用语法：

```sql
UPDATE 表名 SET 字段1=新值1, 字段2=新值2 [WHERE Clause];
```

















