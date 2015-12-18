---
layout: post
title:  "linux学习笔记六--linux shell编程"
keywords: ""
description: ""
category: "Linux" 
tags: [bash]
---

**@(linux)[bash]**


### bash 变量类型

>
* - 本地变量(局部变量) 当前shell进程
* - 环境变量 当前shell进程及其子进程
* - 位置变量 
* 	- $1,$2,...
* 	- shift
* - 特殊变量 
	- $? 
	- $# 参数个数
	- $* 参数列表
	- $@ 参数列表

<!-- more -->

### 条件判断

>
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
5. 算数运算
	- let 算数运算表达式; let C=$A+$B
	- $[算数运算表达式]; C=$[$A+$B]
	- $((算数运算表达式)); C=$(($A+$B))
	- expr 算数运算表达式,表达式中各操作数及运算符之间要有空格,而且要使用命令引用; e.g. C=`expr $A + $B`

	e.g. 如果用户存在,就显示用户已存在;否则,添加此用户
		- id user1 && echo 'user1 exist' || useradd user1
	e.g. 如果用户不存在,添加此用户;否则,显示用户已存在
		- ! id user1 && useradd user1 || echo 'user1 exist'
	e.g. 如果用户不存在,添加并设置密码而且强制用户登录更改密码,否则,显示用户已存在
		- ! id user1 && useradd user1 && echo 'user1' | passwd --stdin user1 && chage -d 0 user1 || echo 'user1 exist'


### 测试方法

	- [ expression ]    # 命令测试
	- [[ expression ]]  # 关键字测试
	- test expression

### bash 测试脚本是否有语法错误

	- bash -n script
	- bash -x script # 调试
	 
### 定义脚本退出状态码

>  exit #: 如果没有明确定义退出状态码,那么,最后执行的一条命令的退出状态码即为脚本的退出状态码

## bash 中常用的条件测试:

### 整数测试

>
- -gt
- -lt
- -eg
- -ge
- -le
- -ne
	
### 文件测试

>
- -e FILE 测试文件是否存在
- -f FILE 测试文件是否是普通文件
- -d FILE 测试指定路径是否为目录
- -r FILE 
- -w FILE
- -x FILE 测试当前用户对指定文件是否有读/写/执行权限


### 字符串比较

字符测试

>
- == 测试是否相等,相等为真,不等为假
- != 测试是否不等,不等为真,相等为假
- >
- <
- -n string 测试指定字符串是否为空,空则真,不空为假
- -s string 测试指定字符串是否不空,不空为真,空为假

### 组合测试

组合测试条件

>
* -a and 与关系
* -o or 或关系
* !  not 非


## 面向过程
---

### 控制结构

>
* 顺序结构
* 选择结构
* 循环结构

**选择结构**

if 语句 

```bash 
if condition; then 
	statement
	...
fi 

if condition; then 
	statement
	...
elif condition; then
	statement
	...
else 
	statement
	...
fi 
```

case语句 

```bash 
case switch in 
value1)
	statement
	...
	;;
value2)
	statement
	...
	;;
*)	
	statement
	...
	;;
esac
```

**循环结构**

while 循环

- 特殊用法一

```bash 
while :; do 
>
done # 死循环
```

- 特殊用法二

```bash 
while read LINE; do 

 done < /path/to/somefile 
# 通过输入重定向读取文件/path/to/somefile,每次读取一行并赋值给LINE
```

## function
---

- 格式一

```bash
function FUNNAME {
	
}
```

- 格式二

```bash
FUNNAME() {
	
}
```

## 练习

```bash 
1. 写一个脚本,可以接受选项及参数,而后能获取每一个选项,及选项的参数,并能根据选项及参数做出特定的操作;比如

	e.g. adminuser.sh --add tom,jerry --del tom blarc -v|--verbose -h|--help
#!/bin/bash 
set -o nounset                        # treat unset variables as an error

\#debug() {
\#       [ "$_debug" == "true" ] && $@ || :
\#}
declare -i debug=0
declare -i adduser=0
declare -i deluser=0

\#for argv in "$@"; do 
while [ $# -gt 0 ]; do
\#       if [ -n $1 ]; then 
        case $1 in
                -v|--verbose)
                        debug=1
                        shift
                        ;;
                -h|--help)
                        echo "usage: `basename $0` --[add|del] uSERLIST -v|--verbose -h|--help"
                        shift
                        exit 0
                        ;;
                --add)
                        adduser=1
                        shift 
                        adduserlist=$1
                        shift
                        ;;
                --del)
                        deluser=1
                        shift 
                        deluserlist=$1
                        shift
                        ;;
                *)
                        echo "usage: `basename $0` --[add|del] uSERLIST -v|--verbose -h|--help"
                        exit 1
                        ;;

        esac
#       fi
done

ifs=','
if [ $adduser -eq 1 ]; then
        for user in $adduserlist; do 
                if id $user &> /dev/null; then 
                        [ $debug -eq 1 ] && echo "$user exist"
                else
                        useradd $user 
                        echo "$user" | passwd --stdin "$user" &> /dev/null 
                        chage -d 0 "$user"
                        [ $debug -eq 1 ] && echo "$user added"
                fi
        done 
fi

if [ $deluser -eq 1 ]; then 
        for user in $deluserlist; do
                if id $user &> /dev/null; then 
                        userdel -r $user 
                        [ $debug -eq 1 ] && echo "$user has deleted"
                else
                        [ $debug -eq 1 ] && echo "$user does's not exist"
                fi
        done
fi

2. 写一个脚本showlogged.sh,其用法格式为:

	showlogged.sh -v -c -h|--help

	- -h选项只能单独使用,用于显示帮助信息;
	- -c选项,显示当前系统上登陆的所有用户数;
	- 如果同时使用-v选项,则既显示同时登陆的用户数,又显示登陆用户的相关信息
	
	Logged users: 4.
	
	They are:
	root 	tty2 	Feb 18 02:41
	root 	pts/1 	Mar 8  08:36 (192.168.1.100)
	root 	pts/5 	Mar 8  08:36 (192.168.1.101)
	hadoop 	pts/6 	Mar 8  08:36 (192.168.1.103)
#!/bin/bash
set -o nounset                        # Treat unset variables as an error

declare -i CNT=0
declare -i VERBOSE=0
while [ $# -gt 0 ]; do 
    case $1 in 
            -h|--help)
                    echo "Usage: `basename $0` -v -c -h|--help"
                    exit 0
                    ;;
            -c)
                    CNT=1
                    shift 
                    ;;
            -v)
                    VERBOSE=1
                    shift 
                    ;;
            *)
                echo "Usage: `basename $0` -v -c -h|--help"
                exit 1
                ;;

    esac
done

if [ $CNT -eq 1 ]; then 
    COUNT=`who | wc -l`
    echo "Logged users: $COUNT"
    if [ $VERBOSE -eq 1 ]; then
    	echo "They are:"
    	who
	fi
fi
```

