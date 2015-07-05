---
layout: post
title:  "Linux cmd tips"
keywords: "linux cmd"
description: ""
category: "wiki" 
tags: [linux ]
---

Linux 有用命令介绍 

<!-- more -->

1. 在其它目录运行一个命令，然后返回当前工作目录

	如果你想在其它目录运行一个命令，然后返回当前工作目录，要实现这样的目地，
	你只需要将命令放在一个**圆括号**里。

	`$ (cd ~; ls)`

2. 命令行常用快捷键:
	* CTRL  + U					– 剪切光标前的内容
	* CTRL  + K 				– 剪切光标至行末的内容
	* CTRL  + Y 				– 粘贴
	* CTRL  + E 				– 移动光标到行末
	* CTRL  + A 				– 移动光标到行首
	* ALT   + F 				– 跳向下一个空格
	* ALT   + B 				– 跳回上一个空格
	* ALT   + Backspace			– 删除前一个单词
	* CTRL  + W					– 剪切光标后一个单词
	* Shift + Insert			– 向终端内粘贴文本
	* Ctrl  + R					- 在历史命令中搜索
	
	`man readine`				# 查看bash中的默认热键绑定

	`man ascii`					# 获得一个ASCII表格


3. 浏览文件系统

	**ranger** 是浏览文件系统的好帮手。

	ranger 用 Python 完成，默认为使用 Vim 风格的按键绑定，比如 hjkl（上下左右），dd（剪切），yy（复制）等等。
	功能很全，扩展/可配置性也非常不错。类似MacOS X下Finder（文件管理器）的多列文件管理方式。支持多标签页。实时预览文本文件和目录.

4. sudo !!

	sudo !! 以sudo的形式运行上一条命令。

	例如：

	`apt-get install ranger` 一定会出现"Permission denied",**除非你已经登录
	了足够高权限的用户**

	这是如果运行`sudo !!`，上一条命令就会变成`sudo apt-get install ranger`，*so nice*

5. vim 用root方式保存文本

	`:w !sudo tee %`

	命令`:w !{cmd}`，让 vim 执行一个外部命令{cmd}，然后把当前缓冲区的内容从 stdin 传入。

	tee 是一个把 stdin 保存到文件的小工具。

	而 %，是vim当中一个只读寄存器的名字，总保存着当前编辑文件的文件路径。
	所以执行这个命令，就相当于从vim外部修改了当前编辑的文件.
	
	*good*

6. 创建目录树

	`mkdir -p project/{lib/ext,bin,src,doc/{html,info,pdf},demo/stat/test}`

	**注：目录格式当中不要有空格，否则会与期望的相悖**

7. 重复上次命令行的参数

	`ALT+.` or `ESC+.`

8. 替换前一条命令里的部分字符串

	`^old^new`

9. 在远程机器上运行一段脚本

	`ssh user@server bash < /path/to/local/script.sh`

10. 比较一个远程文件和本地文件

	`ssh user@host cat /path/to/remoteile | diff /path/to/localfile -`

11. vim 一个远程文件

	`vim scp://username@host//path/to/file`
	


 未完待续。。。

