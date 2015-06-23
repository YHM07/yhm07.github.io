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
	如果你想在其它目录运行一个命令，然后返回当前工作目录，要实现这样的目地，你只需要将命令放在一个**圆括号**里。
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

3. 浏览文件系统
	**ranger** 是浏览文件系统的好帮手。

4. sudo !!
	sudo !! 以sudo的形式运行上一条命令。例如：
	`apt-get install ranger` 一定会出现"Permission denied",**除非你已经登录了足够高权限的用户**
	这是如果运行`sudo !!`，上一条命令就会变成`sudo apt-get install ranger`，*so nice*

5. 未完待续。。。
 
