---
layout: post
title:  "linux学习笔记七-bash"
keywords: ""
description: ""
category: "linux" 
tags: [bash]
---

# Linux 基础学习笔记七 -- bash

@(linux)[bash]

<!-- more -->
# 5-3 bash脚本编程 2015/11/1

## 条件判断

1. 条件测试类型
	* 整数测试
	* 字符测试
	* 文件测试
2. 条件测试表达式
	* [ expression ]    # [ 表达式两边必须各有一个空格 ]
	* [[ expression ]]
	* test expression
3. 整数比较
	* -eq 测试两个整数是否相等
	* -ne 测试两个整数是否不等
	* -gt 测试一个数是否大于另一个数
	* -lt 测试一个数是否小于另一个数
	* -ge 测试一个数是否大于或等于另一个数
	* -le 测试一个数是否小于或等于另一个数
4. 命令间的逻辑关系(短路运算)
	* &&
	* || 

	e.g. 如果用户存在,就显示用户已存在;否则,添加此用户
		- id user1 && echo 'user1 exist' || useradd user1
	e.g. 如果用户不存在,添加此用户;否则,显示用户已存在
		- ! id user1 && useradd user1 || echo 'user1 exist'
	e.g. 如果用户不存在,添加并设置密码而且强制用户登录更改密码,否则,显示用户已存在
		- ! id user1 && useradd user1 && echo 'user1' | passwd --stdin user1 && chage -d 0 user1 || echo 'user1 exist'
5. 算数运算
	- let 算数运算表达式; let C=$A+$B
	- $[算数运算表达式]; C=$[$A+$B]
	- $((算数运算表达式)); C=$(($A+$B))
	- expr 算数运算表达式,表达式中各操作数及运算符之间要有空格,而且要使用命令引用; C=`expr $A + $B`

# 6-1 Bash脚本编程 2015/11/2

## 测试方法
	- [ expression ]    # 命令测试
	- [[ expression ]]  # 关键字测试
	- test expression

## bash 测试脚本是否有语法错误
	- bash -n script
	- bash -x script # 调试
	- 
## 定义脚本退出状态码
	- exit #: 如果没有明确定义退出状态码,那么,最后执行的一条命令的退出状态码即为脚本的退出状态码

## bash 中常用的条件测试:

### 整数测试
	- -gt
	- -lt
	- -eg
	- -ge
	- -le
	- -ne
	
### 文件测试
	- -e FILE 测试文件是否存在
	- -f FILE 测试文件是否是普通文件
	- -d FILE 测试指定路径是否为目录
	- -r FILE 
	- -w FILE
	- -x FILE 测试当前用户对指定文件是否有读/写/执行权限

## 多分支if语句

 if expression; then
	statemant
	...
 elif expression; then
	statemant
	...
 elif expression; then
	statemant
	...
 else 
	statement
	...
 fi

## bash 变量类型
	- 本地变量(局部变量) 当前shell进程
	- 环境变量 当前shell进程及其子进程
	- 位置变量 
		- $1,$2,...
		- shift
	- 特殊变量 
		- $? 
		- $# 参数个数
		- $* 参数列表
		- $@ 参数列表

## 字符串比较

字符测试
	- == 测试是否相等,相等为真,不等为假
	- != 测试是否不等,不等为真,相等为假
	- >
	- <
	- -n string 测试指定字符串是否为空,空则真,不空为假
	- -s string 测试指定字符串是否不空,不空为真,空为假
