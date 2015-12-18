---
layout: post
title:  "linux学习笔记七--linux文件查找"
keywords: ""
description: ""
category: "Linux" 
tags: [find|locate]
---

**@(linux)[find|locate]**


### vim编辑器 2015/11/4


vim(Vi(Visiual Interface) iMproved)

>
1. vim +#: 打开文件,并定位到第#h行
2. vim + 打开文件,定位到文件最后一行
3. vim +/PATTERN 打开文件,定位到第一次被PATTERN匹配到的行的行首

## 文件查找
---

### locate

**locate:非实时,模糊匹配,根据全系统文件数据库进行查找,速度快**

	updatedb: 手动生成文件数据库

<!-- more -->

### find

**find: 实时查找,精确,通过遍历指定目录中所有文件完成查找,速度慢**

	find 查找路径 查找标准 查找到以后的处理动作

>
* - 查找路径 默认当前目录
* - 查找标准 默认指定路径下所有文件
* - 处理动作 默认为显示

匹配标准

>
- -name 'FILENAME' 对文件名作精确查找
	- 文件名通配:
		* \* 任意长度的任意字符
		* ?
		* []
- -iname "FILENAME" 文件名匹配不区分大小写
- -regex PATTERN 基于正则表达式进行文件名匹配
- -user USERNAME 根据属主查找
- -grop GROUPNAME 根据属组查找
- -uid UID 根据UID查找
- -gid GID 根据GID查找
- -nouser 查找没有属主的文件
- -nogroup 查找没有属组的文件
- -type 
	* f 普通文件
	* d 目录
	* c 字符设备
	* b 块设备
	* l 链接
	* p pipe
	* s socket
- -size 
	* [+|-]#k 
	* [+|-]#M
	* [+|-]#G		
	* [+|-]#c     # bytes
- 组合条件
	* -a 
	* -o
	* -not # 非
- -mtime [+|-]days
- -ctime
- -atime 
- -mmin [+|-]minutes
- -cmin
- -amin 
- -perm 
	* MODE # 精确匹配
	* /MODE # 任意一位匹配即满足
	* -MODE # 文件权限能完全包含此MODE时才满足条件

action

>
- pirnt 
- -ls 类似ls -l 的形式显示每一个文件的详细
- -exec COMMAND {}\;
- -ok COMMAND {}\; # need confirm 每一次操作都需要用户确认

## 练习
---

```bash 
1. 查找/var目录下属主为root并且属组为mail的所有文件
	find /var -user root -a -group mail
2. 查找/usr目录下不属于root,bin,或student的文件
	find /usr -not \( -user root -o -user bin -o -user student \)
3. 查找/etc目录下最近一周内容修改过并不属于root及student用户的文件
	find /etc -mtime -7 -a -not \( -user root -o -user student \)
	find /etc -mtime -7 -not -user root -a -not -user student 
4. 查找当前系统上没有属主或属组且最近一天内曾被访问过的文件,并将其属主属组均修改为root
	find / \( -nouser -o -nogroup \) -a -atime -1 -exec chown root:root {} \;
5. 查找/etc目录下大于1M的文件,并将其文件名写入/tmp/etc.largefile文件中
	find  /etc -size +1M > /tmp/etc.largefile
6. 查找/etc目录下所有用户都没有写权限的文件,显示出其详细信息
	find /etc -not -perm /222 -ls
```

## 附录: 
---

* [Linux find用法示例](http://blog.csdn.net/YHM07/article/details/45102511)



