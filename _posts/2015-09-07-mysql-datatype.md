---
layout: post
title:  "MySQL学习笔记-数据类型"
keywords: ""
description: ""
category: "MySQL" 
tags: [mysql, 数据类型]
---

# 操作数据库学习笔记#
---

**MySQL默认用户_root_, 默认端口号_3306_**

## MySQL 数据类型
---

### 整型(分有符号整型和无符号整型)
---

> TINYINT(一个字节)

> SMALLINT(两个字节)

> MEDIUMINT(三个字节)

> INT(四个字节)

> BIGINT (八个字节)

<!-- more -->

### 浮点型
---

> FLOAT[(M,D)] M是数字总位数，D是小数位数

> DOUBLE[(M,D)] M是数字总位数，D是小数位数

### 日期类型
---

> YEAR

> TIME

> DATE

> DATETIME

> TIMESTAMP


### 字符型
---

> CHAR(M) 定长类型

> VARCHAR(M) 变长类型

> TINETEXT

> TEXT

> MEDIUMTEXT

> LONGTEXT

> ENUM('val1', 'val2',...)

> SET('val1','val2',...)
