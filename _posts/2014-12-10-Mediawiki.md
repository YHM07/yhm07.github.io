---
layout: post
title: "Running MediaWiki on Ubuntu" 
keywords: "MediaWiki ubuntu"
description: "Running MediaWiki on Ubuntu" 
category: Wiki
tags: [ubuntu, MediaWiki]
---

**以下有关MediaWiki的安装参考[Community Help Wiki][1]**
[1]: https://help.ubuntu.com/community/MediaWiki

## Install pre-requisites ##
首先安装LAMP(Linux,Apache2,MySQL,PHP)服务。

	sudo apt-get install tasksel

	sudo tasksel install lamp-server

<!-- more -->

在安装lamp-server过程当中要进行以下配置:

> Configuring mysql-server 

>	- New password for the MySQL "root" user: admin

>	- Repeat password for the MySQL "root" user: admin


## Install MediaWiki ##

这里使用的是源码安装，首先下载[MediaWiki][2],然后将Mediawiki拷贝到**/var/www/**目录下并更名为wiki。至此，MediaWiki安装完毕，以下为可选安装软件。

	sudo apt-get install imagemagick mediawiki-math php5-gd

这里都没有安装。

[2]:http://www.mediawiki.org/wiki/MediaWiki "MediaWiki Official Website"

## Start your MediaWiki ##
在地址栏输入[localhost](http://localhost/wiki).

Follow the setup instructions.

若此时提示**"Not Found The requested URL /wiki was not found on this server",**这表明默认路径不对，对于**ubuntu14.04**,Apache2的默认根目录为**/var/www/html**,所以我们需要将Apache2的更目录改为我们需要的，

	sudo nano /etc/apache2/sites-enabled/000-default.conf

将其中的**DocumentRoot /var/www/html** 改为**DocumentRoot /var/www** 重启Apache服务

	sudo service apache2 restart
	
or 
	
	sudo /etc/init.d/apache2 restart

[Again](http://localhost/wiki).

Follow the setup instructions.
然后依次进行直到*Connect to database*.


>
* MySQL settings
	- Database host: localhost
	- Identify this Wiki
		- Database name: wikidb
		- Database table prefix: wiki_
	- User account for installation
		- Database username: root
		- Database password: admin
* Database settings
	- Database account for web access
		- use the same account as for installation
	- storage engine
		- InnoDB
	- Database character set:
		- Binary
* Name of Wiki
	- YHM's Blog
* Administrator account
	- Your username: admin
	- password: root
* Personal account
	- Your username: YHM07
	- password: admin




