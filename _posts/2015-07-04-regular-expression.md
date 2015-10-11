---
layout: post
title:  "Linux regular expression"
keywords: "linux regular expression"
description: ""
category: "Linux" 
tags: [linux, Regular Expression ]
---

Linux Shell 正则表达式


## 通配符(wildcards)
>
- * 匹配任意内容
- ？ 匹配任意一个字符
- [] 匹配括号里边的任意一个字符

通配符用来匹配符号条件的文件名，是完全匹配。

## 正则表达式(regular expression)

> 正则表达式用来在文件中匹配符合条件的字符串，正则是包含匹配。

<!-- more -->

## 基础正则表达式

>
- * 前一个字符匹配0次或任意多次
- . 匹配除了换行符外任意一个字符
- ^ 匹配行首。
- $ 匹配行尾
- [] 匹配括号中指定的任意一个字符，只匹配一个字符。
- [^] 匹配除中括号的字符以为的任意字符
- \ 转义符
- \{n\} 表示其前面的字符恰好出现n次
- \{n,\} 表示其前面的字符出现不小于n次
- \{m,n\} 表示其前面的字符出现不小于m次，不大于n次

## 扩展正则表达式


## 参考

1. [python正则表达式指南][1]

2. [正则表达式30分钟入门教程][3]

[1]: http://www.cnblogs.com/huxi/archive/2010/07/04/1771073.html

[2]: http://deerchao.net/tutorials/regex/regex.htm
	
