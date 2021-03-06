---
layout: post
title:  "编译安装python3.4过程当中的一些问题"
keywords: ""
description: ""
category: "Python" 
tags: [python,pip,centos]
---

## 安装环境：

---
- **CentOS Linux release 7.1.1503 (Core)**

- **python版本2.7.5**

## Install pip

---
参考安装说明[Install pip][1]安装pip。

	wget https://bootstrap.pypa.io/get-pip.py 

	python get-pip.py 

## TroubleShooting

---
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

---
首先需要确认系统是否已经安装**MySQL-python**

	rpm -qa | grep MySQL-python

如果没有安装，需要首先安装**MySQL-python**

	yum -y install MySQL-python

其次，安装MySQL工具 

	pip install MySQL-python

附：ubuntu14.04 LTS 的解决方案

	sudo apt-get install python-dev

	sudo apt-get install mysql-server

	sudo apt-get install libmysqlclient-dev

	pip install MySQL-python

### 安装过程当中缺少一些文件

---
> c/\_cffi_backend.c:13:17: fatal error: ffi.h: No such file or directory

解决方案：

	yum -y install libffi-devel

> build/temp.linux-x86_64-2.7/\_openssl.c:400:25: fatal error: openssl/aes.h: No such file or directory

解决方案：

	yum -y install openssl-devel

### pip 命令自动补全

---
	pip completion --bash >> ~/.profile

or 

	pip completion --bash >> ~/.bashrc

然后：

	source ~/.profile 

or 

	source ~/.bashrc

**本人采用的是第二种情况，能够正确补全**

### ipython 安装后不能智能补全

---
首先，编译安装了python3.4, 然后利用virtualenv, 创建了独立的python3.4的开发环境

	virtualenv --no-site-packages --python=python3.4 ~/.virtualenvs/py3env

利用pip安装ipython，结果出现了Waring，而且没有高亮显示，不能TAB补全。

> WARNING: IPython History requires SQLite, your history will not be saved.

> WARNING: Readline services not available or not loaded.

> WARNING: The auto-indent feature requires the readline library

解决方案

---

- 安装SQLite-devel

```
	yum install sqlite-devel
```

- 安装readline-devel
 
```
	yum install readline-devel 
```

- 重新编译安装python3.4，成功。




## 参考

---
- [pip 安装说明][1]
- [Python包管理工具--pip][2]
- [pip 用户手册][3]

[1]: https://pip.pypa.io/en/stable/installing/
[2]: http://lesliezhu.github.io/public/2014-11-08-pip.html
[3]: https://pip.pypa.io/en/stable/



