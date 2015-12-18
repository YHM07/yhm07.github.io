---
layout: post
title:  "Linux命令行技巧"
keywords: "linux cmd"
description: ""
category: "Wiki" 
tags: [linux ]
---


## 在其它目录运行一个命令，然后返回当前工作目录

如果你想在其它目录运行一个命令，然后返回当前工作目录，要实现这样的目地，你只需要将命令放在一个**圆括号**里。

	$ (cd ~; ls)

<!-- more -->

## 命令行常用快捷键:

	CTRL  + U				– 剪切光标前的内容

	CTRL  + K 				– 剪切光标至行末的内容

	CTRL  + Y 				– 粘贴

	CTRL  + E 				– 移动光标到行末

	CTRL  + A 				– 移动光标到行首

	ALT   + F 				– 跳向下一个空格

	ALT   + B 				– 跳回上一个空格

	ALT   + Backspace		– 删除前一个单词

	CTRL  + W				– 剪切光标后一个单词

	Shift + Insert			– 向终端内粘贴文本

	Ctrl  + R				- 在历史命令中搜索

	ALT+. or ESC+.			# 重复上次命令行的参数

	^old^new				# 替换前一条命令里的部分字符串

	man readine				# 查看bash中的默认热键绑定

	man ascii				# 获得一个ASCII表格


**详细说明可以参考：[Bash Shortcuts][1] 和 [Using CLI][2]**
[1]: http://iplayboy.tk/linux/2015-08/bash-shortcuts.html
[2]: http://iplayboy.tk/linux/2015-08/using-cli.html

## 浏览文件系统

**ranger** 是浏览文件系统的好帮手。

ranger 用 Python 完成，默认为使用 Vim 风格的按键绑定，比如 hjkl（上下左右），dd（剪切），yy（复制）等等。功能很全，扩展/可配置性也非常不错。类似MacOS X下Finder（文件管理器）的多列文件管理方式。支持多标签页。实时预览文本文件和目录.

## 超级用户运行命令

	sudo !!		# 以sudo的形式运行上一条命令。

例如：

	apt-get install ranger

一定会出现**"Permission denied"**,除非你已经登录了足够高权限的用户

这时如果运行`sudo !!`，上一条命令就会变成`sudo apt-get install ranger`，*so nice*

## vim 用root方式保存文本

	:w !sudo tee %	

命令`:w !{cmd}`，让 vim 执行一个外部命令{cmd}，然后把当前缓冲区的内容从 stdin 传入。

tee 是一个把 stdin 保存到文件的小工具。

而 %，是vim当中一个只读寄存器的名字，总保存着当前编辑文件的文件路径。
所以执行这个命令，就相当于从vim外部修改了当前编辑的文件.
	
## 创建目录树

	mkdir -p project/{lib/ext,bin,src,doc/{html,info,pdf},demo/stat/test}

**注：目录格式当中不要有空格，否则会与期望的相悖**

## 重置终端

	reset 

or 

	stty sane

## 记录并回放终端会话

通过使用命令**script**和**scriptreplay**分别来记录并回放终端会话。

- 记录终端会话

```
	script -t 2> timing.log -a output.session
```

然后会在当前目录生成文件**timing.log**和**ouput.session**,开始记录输入的命令，但此时该文件为空，
只有退出**script**命令后，该文件才会存入数据。

> timing.log 记录每个命令执行的时间信息

> output.session  记录每个命令的输出

- 回放记录的会话

```
	scriptreplay timing.log output.session
```

**注：timing.log output.session 可以被任何想要在自己终端上重放会话的人使用。**

## 查看内存或CPU使用较多的进程

- 查看占用内存大小前10的进程

```
	ps -eo comm,size --sort -size | head -10
```

- 查看占用CPU前10的进程

```
	ps -eo comm,pcpu --sort -size | head -10
```


未完待续。。。

