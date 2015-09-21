---
layout: post
title:  "MySQL学习笔记-数据表基本操作"
keywords: ""
description: ""
category: "MySQL" 
tags: [mysql]
---

# 操作数据表学习笔记#
---

**MySQL默认用户_root_, 默认端口号_3306_**

### 查看数据表

	SHOW COLUMNS [FROM tbl_name]

<!-- more -->

### 插入记录

	INSERT [INTO] tbl_name [(col_name,...)] VALUES(val, ...)

### 查找记录

	SELECT expr,... FROM tbl_name

## 约束

- 约束保证数据的完整性和一致性；

- 约束分为表级约束和列级约束；(针对约束字段数目多少分类)

```
	约束只针对某一字段来进行使用称为列级约束
	
	约束针对两个或两个以上的字段来进行使用称为表级约束
```

- 约束类型：

```
	NOT NULL					(非空约束)
	
	PRIMARY KEY					(主键约束)
	
	UNIQUE KEY					(唯一约束)
	
	DEFAULT						(默认约束)
	
	FOREIGN KEY					(外键约束)
```

### PRIMARY KEY 主键约束

> 每张数据表只能存在一个主键；

> 主键保证记录唯一性；

> 主键自动为NOT NULL；

> AUTO\_INCREMENT 必须和PRIMARY KEY配合使用，但PRIMARY KEY不必要必须和AUTO\_INCREMENT配合使用。

	CREATE TABLE tbl_name(
		id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
		username VARCHAR(20) NOT NULL
	);

### UNIQUE KEY 唯一约束


> 唯一约束可以保证记录的唯一性

> 唯一约束的字段可以为空值(NULL)

> 每张数据表可以存在多个唯一约束

### DEFAULT 默认约束

> 当插入记录时，如果没有明确为字段赋值，则自动赋予默认值

### FOREIGN KEY(外键约束)

> 数据表的存储引擎只能时InnoDB

> 父表和子表必须使用相同的存储引擎，而且禁止使用临时表

> 外键列和参照列必须具有相似的数据类型，其中，数字的长度或是否有符号位必须相同；而字符的长度可以不同

> 外键列和参照列必须创建索引。如果外键列不存在索引的话，MySQL将自动创建索引

