---
layout: post
title:  "ubuntu14.04 搭建FTP服务器 -- vsftpd的安装和配置"
keywords: "ubuntu14.04 vsftpd"
description: "how to setup vsftpd on ubuntu14.04"
category: "Wiki" 
tags: [ubuntu14.04, vsftpd]
---

### 更新源列表

	sudo apt-get update

### 安装vsftpd

	sudo apt-get install vsftpd

### 测试vsftpd是否安装成功，并顺利启动

`pgrep vsftpd`,如果显示一个数字，表示vsftpd服务已经安装成功，并且顺利启动。该数字就是vsftpd服务的进程号。如果没有任何线索，可能服务并没有顺利启动，或者没有安装成功。*请自行查明原因*.

<!-- more -->

### FTP服务需求

>
* vsftpd 使用stand alone 的方式启动。（或 super daemon）
* 禁止匿名用户访问
* 使用当地时间，取代GMT时间
* 系统账号不可以登陆主机（即UID小于500的账号）
* 允许本地用户访问,用户为uftp，其余用户无法访问
* 用户新增目录，文档，umask默认为022

### 配置vsftpd

**vsftpd配置采用*name=value*的方式，等号两端没有空格**

#### vsftpd的有关配置文件

	* /etc/vsftpd.conf					# vsftpd 的主要配置文件
	* /etc/pam.d/vsftpd					# vsftpd 使用pam模块时相关配置文件
	* /etc/ftpusers						# vsftpd 指定无法登陆FTP的系统账号
	* /etc/vsftpd.chroot_list			# 该文件默认没有，需自己创建
	* /etc/vsftpd.user_list				# 该文件默认没有，需自己创建
	* 其他

#### **修改配置文件/etc/vsftpd.conf**

	* listen=YES						# vsftpd 以stand alone方式启动
	* anonymous_enable=NO				# 禁止匿名用户访问
	* local_enble=YES					# 允许本地用户访问FTP
	* write_enable=YES					# 允许用户上传文件
	* local_umask=022					# 上传文件的默认掩码
	* use_localtime=YES					# 使用当地时间
	* chroot_local_user=YES				# 禁止用户访问上层目录
	* allow_writeable_chroot=YES		# 
	* userlist_enable=YES				#
	* userlist_deny=NO					#
	* userlist_file=/etc/vsftpd.user_list  
	* 其余配置保持默认

### 新建用户uftp

	sudo adduser uftp

然后会要求输入一些个人信息，也可以为空。最后输入密码，密码以****显示，然后再次确认密码，该账户和密码用于以后登陆FTP，下载或上传文件。然后就会在/home/目录下新建文件夹**uftp**，该文件夹即为后期FTP服务器的文件存储目录。默认权限为755.

### 除uftp用户外，禁止其余用户访问FTP服务

设置配置文件*/etc/vsftpd.conf*,将*userlist_enable=YES*的注释去掉，并创建文件*/etc/vsftpd.user_list*,然后将*/etc/password*文件中除uftp外的用户添加到*/etc/vsft.user_list*文件当中。

**技巧：使用vim 打开/etc/vsftpd.user_list文件，然后切换到命令行模式`：r /etc/password`,这样所有的用户就会全部写入该文件，然后选中所有文件`EscggVg`,，然后进入命令行模式，`Esc:s/:.\*$//g`,这样所有的用户就会被提取出来**

## 出现的问题及解决方案

* 问题： **在创建用户的时候，如果指定登陆shell为*/usr/sbin/nologin*,后期登陆FTP服务器的时候会报错**		

解决方案：

  如果改为*/bin/bash*,就没有问题。（原因不详）

* 问题： **如果*chroot_local_user=YES*,没有被注释掉，那么在用*FileZilla*登陆FTP服务器的时候,会出现错误:**

> 500 OOPS: vsftpd: refusing to run with writable root inside chroot ()

 是因为：
 
> Add stronger checks for the configuration error of running with a writeable root directory inside a chroot(). This may bite people who carelessly turned on chroot_local_user but such is life.

vsftpd增强了安全检查，如果用户限定了在其主目录，禁止访问上级目录，则该用户的主目录不能再有写权限。如果检测发现有写权限，就会报该错误。

解决方案：

- 可以用命令`chmod a-w /home/user`去除用户主目录的写权限。（该方案未实施）
- 在配置文件`/etc/vsftpd.conf`中添加*allow_writeable_chroot=YES*。（本文采用的是该方案）

	
	
