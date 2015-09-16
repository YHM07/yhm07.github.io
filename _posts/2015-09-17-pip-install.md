---
layout: post
title:  "pip安装过程当中的一些问题"
keywords: ""
description: ""
category: "python" 
tags: [python,pip,centos]
---

## 安装环境：

- CentOS Linux release 7.1.1503 (Core)

- python版本2.7.5

## Install pip

参考安装说明[Install pip][1]安装pip。

## TroubleShooting

在安装完成pip之后，在使用pip安装一些软件包的时候发现存在一些意想不到的问题，大多都是因为依赖的原因，列举如下。

### InsecurePlatformWaring问题

利用pip安装一些python工具的时候，比如安装**ipython**会出现这样的问题：

> /home/centos/.virtualenvs/py2env/lib/python2.7/site-packages/pip/\_vendor/requests/packages/urllib3/util/ssl\_.py:90: InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. For more information, see https://urllib3.readthedocs.org/en/latest/security.html#insecureplatformwarning.

解决方案：

<!-- more -->

---
	
	yum install python-devel libffi-devel openssl-devel

之后安装：

	pip install pyopenssl ndg-httpsclient pyasn1

附：ubuntu14.04 LTS 的解决方案

	apt-get install libffi-dev libssl-dev 

之后安装:
	
	pip install pyopenssl ndg-httpsclient pyasn1


### 安装MySQL-python

首先需要确认系统是否已经安装**MySQL-python**

	rpm -qa | grep MySQL-python

如果没有安装，需要首先安装**MySQL-python**

	yum -y install MySQL-python

其次，安装MySQL工具 

	pip install MySQL-python

### 安装过程当中缺少一些文件

> c/\_cffi_backend.c:13:17: fatal error: ffi.h: No such file or directory

	yum -y install libffi-devel

> build/temp.linux-x86_64-2.7/\_openssl.c:400:25: fatal error: openssl/aes.h: No such file or directory

	yum -y install openssl-devel

### pip 命令自动补全

	pip completion --bash >> ~/.profile

or 

	pip completion --bash >> ~/.bashrc

然后：

	source ~/.profile 

or 

	source ~/.bashrc

**本人采用的是第二种情况，能够正确补全**


## 参考

- [pip 安装说明][1]
- [Python包管理工具——Pipip][2]
- [pip 用户手册][3]

[1]: https://pip.pypa.io/en/stable/installing/
[2]: http://lesliezhu.github.io/public/2014-11-08-pip.html
[3]: https://pip.pypa.io/en/stable/



