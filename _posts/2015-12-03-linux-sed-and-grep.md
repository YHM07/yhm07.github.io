---
layout: post
title:  "linux学习笔记三--sed and grep"
keywords: ""
description: ""
category: "linux" 
tags: [sed|grep]
---


# 6-2 流编辑器sed  2015/11/3
@(linux)[sed|grep]

<!-- more -->

## sed(stream editor)

默认不编辑原文件,仅对模式空间中的数据做处理,处理结束后,将模式空间打印至标准输出

sed 命令格式:

> sed [option] 'AddressCommnd' file,... # Address与Command之间不需要空格

1. option 
	- -n 静默模式,不再显示模式空间中的内容
	- -i 直接修改源文件
	- -e SCRIPT -e SCRIPT 
	- -f /path/to/script 
	- -r 表示使用扩展正则表达式

2. Address
	- StartLine,EndLine # 从StartLine到EndLine
	- /pattern/ # 使用正则表达式指定模式
	- /pattern1/,/pattern2/ # 第一次被pattern1匹配的行开始,至第一次陪pattern2匹配到的行结束
	- LineNumber # 指定行号
		* $ 表示最后一行
		# * $-1 表示倒数第二行
	- StarLine,+N 表示从StartLine开始,向后的N行

3. Command
	-d 删除符合条件的行
	-p 显示符合条件的行
	-a \string 在指定行后追加新行,内容为string
	-i \string 在指定行前追加新行,内容为string
	-r /path/to/somefile 将指定文件内容添加至符合条件的行处
	-w /path/to/somefile 将地址指定范围内的行保存至指定文件中
	-s/pattern/replacement/flag 查找并替换,默认替换每行中第一次被模式匹配到的串.flag: g表示全局替换;i忽略大小写;N表示替换第几个.分隔符可以是任意的,只要对应即可,如s###,s@@@.
	- \(\), 反向引用,\1,\2,..., & 引用模式中匹配到的整个字符串

## grep及正则表达式

### grep, egrep, fgrep

1. grep 根据搜索模式文本,并将符合模式的行显示出来

 pattern 文本字符或正则表达式的元字符组合而成的匹配条件

 grep [options] pattern [FILE,...]
	* -i ignore case 
	* --color=[WHEN] 
	* -v 反向查找,显示没有被模式匹配的行
	* -o 只显示被模式匹配到的字符串
	* -E 使用扩展的正则表达式(egrep)
	* -A NUM 显示pattern匹配行及其后NUM行
	* -B NUM 显示pattern匹配行及其前NUM行
	* -C NUM 显示pattern匹配行及其前后各NUM行

2. 正则表达式 REGular EXPression, REGEXP
	* 元字符：
		- . 匹配任意单个字符
		- [] 匹配指定范围内的任意单个字符
		- [^] 匹配指定范围外的任意单个字符
		- [:digit:] 数字
		- [:lower:] 小写字母
		- [:upper:] 大写字母
		- [:punct:] 标点符号
		- [:space:] 空白字符
		- [:alpha:] a-zA-Z
		- [:alnum:] a-zA-Z0-9
		
	* 匹配次数(默认贪婪模式) 
		- \* 匹配其前面的字符任意次,包括零次
		- .\* 匹配任意长度的任意字符	
		- \? 匹配其前面的字符一次或零次(可有可无)
		- \{m,n\} 匹配其前面的字符至少m次,至多n次
	* 位置锚定
		- ^ 锚定行首,此字符后的任意内容必须出现在行首
		- $ 锚定行尾,此字符前的任意内容必须出现在行尾
		- ^$ 空白行
		- \< 或 \b 锚定词首,其后面的任意字符必须作为单词首部出现
		- \> 或 \b 锚定词尾,其前面的任意字符必须作为单词尾部出现
	* 分组 \( \) 
		- 后向引用 \n 表示第n个左括号以及对应的右括号的内容

3. 扩展正则表达式 (Extend REGular EXPression)
	- + 匹配其前面的字符至少一次
	- {m,n} 匹配次数
	- () 分组, \1, \2, \3, .., \9
	- | 表示或者
	
 e.g. ifconfig | grep -E "\b([1-9]|[1-9][0-9]|1[0-9][0-9]|2[01][0-9]|22[0-3]\b)(\.\b([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-4])\b){2}\.\b([1-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-4])\b"

 e.g. ifconfig | grep -E "(\<([1-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5]\>)\.){3}(\<[0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5]\>)"

## 练习1

1. 删除/etc/grub.conf文件中行首的空白字符
	- sed -r 's/^[[:space:]]+//' /etc/grub.conf
	- awk '{sub(/^[[:space:]]\*/, ""); print}' /etc/grub.conf
2. 替换/etc/inittab文件中'id:3:initdefault:'一行中的数字为5
	- sed 's/id:3:initdefault:/id:5:initdefault:/' /etc/inittab
	- sed 's/\(id:\)[[:digit:]]\(initdefault:\)/\15\2/' /etc/inittab
3. 删除/etc/inittab文件中的空白行
	sed '/^$/d' /etc/inittab
4. 删除/etc/inittab文件中的开头的#
	sed 's/^#//' /etc/inittab
5. 删除/etc/inittab文件中的开头的#,要求#后必须有空白字符
	sed -r 's/^#[[:space:]]+//' /etc/inittab	
6. 删除某文件中以空白字符后跟#类的行中的开头的空白字符及#
	sed -r 's/^[[:space:]]+#//' dir.name # -r 使用扩展的正则表达式
7. 取出一个文件路径的目录名称
	- sed 's#\(^/.*\)/\(.*\)#\1#' dir.name
	- echo "/etc/rc.d/" | sed -r 's#^(/.*/)[^/]+/?#\1#'
	
## 字符串比较

字符测试
	- == 测试是否相等,相等为真,不等为假
	- != 测试是否不等,不等为真,相等为假
	- >
	- <
	- -n string 测试指定字符串是否为空,空则真,不空为假
	- -s string 测试指定字符串是否不空,不空为真,空为假

## 练习2

1. 传递一个用户名参数给脚本,判断此用户的用户名跟其基本组的组名是否一致,并将结果显示出来
\#!/bin/bash
set -o nounset                        # Treat unset variables as an error

if [ $# -lt 1 ]; then
        echo 'Usage: ./uid-test.sh USERNAME'
        exit 1
fi

USERNAME=$1

if ! id $USERNAME &> /dev/null ; then
        echo "$USERNAME does not exist"
        exit 2
elif [ `id -un $USERNAME` -eq `id -gn $USERNAME` ]; then
        echo 'good guy'
else
        echo 'bad guy'
fi

2. 传递三个参数给脚本,第一个为整数,第二个为算数运算符,第三个为整数,要求将计算结果显示出来,并保留两位精度
\#!/bin/bash
set -o nounset                        # Treat unset variables as an error

if [ $# -ne 3 ]; then
        echo "Usage: ./calc.sh first oper second"
        exit 1
fi

FIRST=$1
OPERATOR=$2
SECOND=$3
\#RESULT=$(($FIRST$OPERATOR$SECOND))
\#echo $RESULT
echo "scale=2;$FIRST$OPERATOR$SECOND" | bc

3. 传递三个参数给脚本,参数均为用户名,将此用户的账户信息提取出来,存放到/tmp/user.info,并显示行号
\#!/bin/bash
set -o nounset                        # Treat unset variables as an error

if [ $# -lt 1 ]; then
        echo 'Usage: ./userinfo.sh USERNAME,...'
        exit 1
fi

for  USER in $* ; do 
        finger $USER >> /tmp/user.info
done

cat -n /tmp/user.info > /tmp/user.info.bak
mv /tmp/user.info.bak /tmp/user.info
sed -i 's/^[[:space:]]*//' /tmp/user.info
4. 判断当前主机的CPU生产商,其信息为/proc/cpuinfo文件中vendor_id;如果生产商为AuthenticAMD,就显示为AMD公司;如果生产商为GenuineIntel,就显示为Intel公司,否则显示非主流公司
\#!/bin/bash
set -o nounset                        # Treat unset variables as an error

CPUINFO=\`grep 'vendor_id' /proc/cpuinfo | cut -d: -f2 | sed 's/^[[:space:]]*//'\`
if [ $CPUINFO = 'AuthenticAMD' ]; then
        echo 'AMD'
elif [ $CPUINFO = 'GenuineIntel' ]; then
        echo 'Intel'
else
        echo 'other'
fi
5. 给脚本传递三个参数,判断其中的最大数或最小数
\#!/bin/bash
set -o nounset                        # Treat unset variables as an error

if [ $# -ne 3 ]; then
        echo 'Usage: num-decision.sh num1 num2 num3'
        exit 1
fi

if [ $1 -gt $2 ]; then
        if [ $1 -gt $3 ]; then
                echo "$1 is the max number!"
        else
                echo "$3 is the max number!"
        fi
else
        if [ $2 -gt $3 ]; then
                echo "$2 is the max number!"
        else
                echo "$3 is the max number!"
        fi
fi

## 练习3

1. 向每个用户问好
	awk 'BEGIN{FS=":";}{print "Hello",$1,",your shell is ",$7;}END{print NR}'

2. 统计SHELL为/bin/bash的用户个数,并问好
#!/bin/bash
set -o nounset                        # Treat unset variables as an error

FILE=/etc/passwd
USERNAME=`cut -d: -f1,7 $FILE`
#USERSHELL=`cut -d: -f7 $FILE`
declare -i COUNT=0
for uname in $USERNAME; do 
        NAME=`echo $uname | cut -d: -f1 `
        USHELL=`echo $uname |cut -d: -f2 `
        if [ $USHELL == "/bin/bash" ]; then
                echo "Hello $NAME, your shell: $USHELL"
                COUNT+=1
        fi
done
echo "user's num is $COUNT"

3. 添加10个用户user1~user10,密码同用户名,只有用户不存在的时候才添加
#/bin/bash
set -o nounset                        # Treat unset variables as an error

if [ $# -lt 2 ]; then
        echo "Usage: user-handle.sh [add|del] USERNAME,..."
        exit 1
fi

if [ $1 == 'add' ]; then 
        shift
        for USERNAME in "$@"; do 
                if ! id "$USERNAME" &> /dev/null; then
                        useradd $USERNAME
                        echo "$USERNAME" | passwd --stdin $USERNAME &> /dev/null 
                        chage -d 0 $USERNAME # 登陆更改密码
                        echo "$USERNAME is added!"
                else
                        echo "$USERNAME has already exist!"
                fi
        done 
else
        shift
        for USERNAME in "$@"; do
                userdel -r $USERNAME
                echo "$USERNAME has been deleted!"
        done
fi
4. 计算100以内所有能被3整除的正整数的和
#!/bin/bash 
set -o nounset                        # Treat unset variables as an error

declare -i SUM=0
for i in `seq 1 100`; do 
        if [ $(($i%3)) -eq 0 ]; then
                SUM=$SUM+$i
        fi
done
echo "$SUM"
5. 统计默认SHELL为bash和/sbin/nologin用户以及各类用户总数
#!/bin/bash
set -o nounset                        # Treat unset variables as an error

declare -i BC=0
declare -i NC=0
for USERS in `cat /etc/passwd`; do 
        USERRSHELL=`echo "$USERS" | cut -d: -f7`
        if [ "$USERRSHELL" == "/bin/bash" ]; then
                BINUSERTMP=`echo "$USERS" | cut -d: -f1`
                if [ $BC -eq 0 ];then
                        BINUSER=$BINUSERTMP
                else
                        BINUSER+=",$BINUSERTMP"
                fi
                BC+=1
        elif [ "$USERRSHELL" == "/sbin/nologin" ]; then
                TMP=`echo "$USERS" | cut -d: -f1`
                if [ $NC -eq 0 ];then
                        NOLOGINUSER=$TMP
                else
                        NOLOGINUSER+=",$TMP"
                fi
                NC+=1
        fi
done

echo "BASH, ${BC}users, they are:"
echo "$BINUSER"
echo "NOLOGIN, ${NC}users,they are:"
echo "$NOLOGINUSER"
