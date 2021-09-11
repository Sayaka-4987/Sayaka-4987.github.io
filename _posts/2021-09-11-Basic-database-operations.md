---
layout:     post   				        # 使用的布局（不需要改）
title:      数据库常用命令 				# 标题 
subtitle:   从删库到跑路					# 副标题
date:       2021-09-11 				    # 时间
author:     YXWang 					    # 作者
header-img: img/post-bg-database.jpg	# 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - 施工中
    - 数据库系统原理
---

# 数据库常用命令

写在最前面：

- 转载自各种网络教程，整理自用；
- 还是老一套免责声明：我太菜了，没法保证内容不出错，欢迎各位神仙大佬拍砖；
- SQL 的关键字默认不区分大小写，本文只是为了 **突出显示哪些是 SQL 的关键字** 才采用大写~~（可把我别扭坏了）~~；



## 对数据库的操作

### `CREATE` 增加

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

### `CREATE` 增加

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

示例，位置默认是 `AFTER` 最后一个字段：

```sql
ALTER TABLE runoob_table ADD COLUMN runoob_likes int unsigned;
```

#### 修改字段

通常是修改属性或者修改数据类型：

```sql
ALTER TABLE 表名 MODIFY [字段名] [数据类型] [列属性];
```

#### 重命名字段

```sql
ALTER TABLE 表名 CHANGE [旧字段] [新字段名] [新数据类型(必选)] [属性];
```

#### 删除字段

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

```sql
UPDATE 表名 SET 字段1=新值1, 字段2=新值2 [WHERE Clause];
```

















