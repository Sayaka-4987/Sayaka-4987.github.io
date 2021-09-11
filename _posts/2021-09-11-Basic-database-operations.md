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
CREATE DATABASE the_database_name；
```

可后接库选项：

```sql
CREATE DATABASE 
IF NOT EXISTS the_database_name 				-- 如果数据库不存在则创建，存在则不创建
DEFAULT CHARSET utf8 COLLATE utf8_general_ci;	-- 设定编码集为utf8
```



### `DROP` 删除

```sql
DROP DATABASE the_database_name;
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

```SQL
ALTER DATABASE the_database_name [库选项名=值];
```

使用示例：

```sql
mysql> ALTER DATABASE RUNOOB CHARSET = GBK;
Query OK, 1 row affected (0.02 sec)
```



## 对表的操作

### `CREATE` 增加

#### 通用语法

需要先 `use database_name` 进入一个数据库：

```SQL
CREATE TABLE the_table_name (column_name column_type);
```

也可以用 `.` 运算符将数据库表创建到指定的数据库下:

```sql
CREATE TABLE database_name.table_name (column_name column_type);
```



#### 设置更多属性

注意这里都不是单引号，是键盘上 1 左边那个 \` 反单引号（backquote），否则会报错 ERROR 1064 (42000): You have an error in your SQL syntax；

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
drop table table_name1,table_name2....;	
```



### `SHOW` 查询

需要先 `use database_name` 进入一个数据库；

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

#### 查看表中的字段信息 

`DES` 、 `DESCRIBE` 、 `SHOW`  三个关键字均可，但写法有细微区别：

```sql
DESC the_table_name;

DESCRIBE the_table_name;

SHOW COLUMNS FROM the_table_name;
```



### `ALTER` 修改

需要先 `use database_name` 进入一个数据库，或把 `table_name` 换成能指定操作的表在哪个数据库里的 `database_name.table_name` ：

#### 修改表名和选项

改名：

```sql
RENAME TABLE old_name TO new_name;
```

修改属性选项：

```sql
ALTER TABLE the_table_name CHARSET=GBK;
```

#### 增加字段

```SQL
ALTER TABLE the_table_name ADD COLUMN [字段名] [数据类型] [列属性] [位置];
```

示例，位置默认是 `AFTER` 最后一个字段：

```sql
ALTER TABLE runoob_table ADD COLUMN runoob_likes int unsigned;
```

#### 修改字段

通常是修改属性或者修改数据类型：

```sql
ALTER TABLE the_table_name MODIFY [字段名] [数据类型] [列属性];
```

#### 重命名字段

```sql
ALTER TABLE the_table_name CHANGE [旧字段] [新字段名] [数据类型] [属性];
```

#### 删除字段

```sql
ALTER TABLE the_table_name DROP [字段名]
```



## 对表中信息的操作









