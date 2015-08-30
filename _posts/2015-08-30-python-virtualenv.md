---
layout: post
title: "创建Python虚拟环境---Virtualenv"
category: "Python"
tags: [Python, virtualenv, virtualenvwrapper]
---

# Virtualenv

**[Virtualenv][1]**用于创建独立的Python环境，多个Python相互独立，互不影响，它能够：

:one: 在没有权限的情况下安装新套件;

:two: 不同应用可以使用不同的套件版本;

:three: 套件升级不影响其他应用

[1]: https://virtualenv.pypa.io/en/latest/

**注：以下操作均在Ubuntu 14.04.3 LTS 下完成**

参考：[创建虚拟Python环境----virtualenv](http://lesliezhu.github.io/public/2014-11-11-virtualenv.html)

<!-- more -->

## 安装

	sudo pip install virtualenv

## Virtualenv基本使用

	virtualenv ENV

or
	
	# 为环境指定Python解释器
	virtualenv --no-site-packages --python=python3.4 py3env

**注：以上命令中的*ENV*以及*py3env*均是目录,运行以上命令会在当前文件夹下生成对应目录*ENV* 和 _py3env_,`--no-site-packages`指定不包括依赖于系统的安装包**

## 激活虚拟环境

	source ENV/bin/active

or
	
	source python3/bin/active

## 离开虚拟环境

	deactive

## 删除虚拟环境

**直接删除目录即可**

	rm -rvf ENV python3

# Virtualenvwapper

**[Virtualenvwapper][2]**是Virtualenv的扩展包，用于方便管理虚拟环境，它能够:

:one: 将所有虚拟环境整合在一个目录下;

:two: 管理(新增、删除、复制)虚拟环境

:three: 切换虚拟环境

[2]: http://virtualenvwrapper.readthedocs.org/en/latest/index.html

## 安装Virtualenvwapper

	# 首先，要确保virtualenv已经安装
	sudo pip install virtualenvwapper 

## 加载virtualenvwrapper

在`$HOME/.bashrc`中添加：

	export WORKON_HOME=$HOME/.virtualenvs
	# export PROJECT_HOME=$HOME/Devel
	source /usr/local/bin/virtualenvwrapper.sh 

然后，运行

	source ~/.bashrc

## 创建虚拟环境目录

默认创建的虚拟环境位于`~/.virtualenvs`目录下，可以通过环境变量`$WORKON_HOME`来定制。如果`~/.virtualenvs/`目录存在,继续，否则创建目录用来存放虚拟环境。
	mkdir $HOME/.virtualenvs

## 创建虚拟环境

	mkvirtualenv py2env

创建后，虚拟环境会自动激活,注意shell提示符的改变

> (py2env)ubuntu@ubuntu-virtual-machine:~$

## 列出所有虚拟环境

	lsvirtualenv 

## 激活虚拟环境

	workon py2env

## 进入虚拟环境目录

	cdvirtualenv

## 进入虚拟环境的site-packages目录

	cdsitepackages

## 列出site-packages目录的所有软件包

	lssitepackages

## 停止虚拟环境

	deactive

## 删除虚拟环境

	rmvirtualenv py2env

# 重建Python环境

## 冻结

所谓`冻结(freeze)`环境，就是将当前环境的软件包等固定下来。

	pip freeze > requirements.txt

## 重建

`重建(rebuild)`环境就是在部署的时候，在生产环境安装对应版本的软件包，不要出现版本兼容等问题。

	pip install -r requirements.txt


# 附录

> [The Hitchhiker's Guide to Python](http://docs.python-guide.org/en/latest/dev/virtualenvs/)

> [virtualenv](https://virtualenv.pypa.io/en/latest/)

> [virtualenvwapper](http://virtualenvwrapper.readthedocs.org/en/latest/index.html)

