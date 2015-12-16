---
layout: post
title:  "CentOS-6.7(final)安装jekyll"
keywords: ""
description: ""
category: "TroubleShooting" 
tags: [CentOS6.7, jekyll, rbenv, ruby]
---

参考：[gist:rbenv-install-and-using.md][1]
[1]: https://gist.github.com/sandyxu/8aceec7e436a6ab9621f

开发环境: 

```bash
$ cat /etc/centos-release 
CentOS release 6.7 (Final)
```

对于**CentOS 6.7**默认ruby版本为1.8.7,而安装**Jekyll**最低需要1.9.3版本的ruby,因此考虑采用**rbenv**版本管理来安装ruby.

<!-- more -->

1. [安装 rbenv][2]

rbenv的源代码托管在[github][2]，在终端中，从 github 上将 rbenv 源码 clone 到本地，然后设置 $PATH。

[2]: https://github.com/rbenv/rbenv#installing-ruby-versions

```bash
git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc 
source ~/.bash_profile 
```

2. [安装 ruby-build][3]

[ruby-build][3] 用来编译安装 Ruby 源码，如果选择手动编译，可不使用这个工具。

[3]: https://github.com/rbenv/ruby-build#readme

新版的Ruby编译安装有一些依赖，请参考 [suggested-build-environment][4]

[4]: https://github.com/rbenv/ruby-build/wiki#suggested-build-environment

首先安装依赖:

```bash 
sudo yum install -y gcc openssl-devel libyaml-devel libffi-devel readline-devel zlib-devel gdbm-devel ncurses-devel
```

然后开始安装 ruby-build：

```bash 
git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
```

3. 安装 Ruby

查看可用的 ruby版本

	rbenv install --list

这里从列表中选择 2.2.3 进行安装：

	rbenv install 2.2.3

等待一会儿，安装完毕后可以查看已经安装的所有 Ruby 版本：

	rbenv versions

4. 设置ruby以及rbenv的使用

rbenv 中的 Ruby 版本有三个不同的作用域：全局(global)，本地(local)，当前终端(shell)。

查找版本的优先级是 当前终端 > 本地 > 全局。

4.1 设置全局版本

全局版本是在没有找到“当前终端”或“本地”作用域的设置时执行。通过以下命令设置：

	rbenv global 2.2.3

4.2 设置本地版本

“本地”作用域是针对各个项目的，通过项目文件夹中的 .rbenv-version 这个文件进行管理，需要将相应的 Ruby 版本号写入这个文件。所以一般设置这个选项就可以了，这个过程可以通过以下命令执行：
	rbenv local 2.2.3

4.3 设置当前终端版本

“当前终端”作用域的优先级最高。通过以下命令设置：

	rbenv shell 2.2.3

4.4 使用系统Ruby

如果要使用系统原有的 Ruby，则通过 system 指定：

	rbenv global system

设置完毕后可以通过以下命令进行验证：

```bash 
which ruby  # ~/.rbenv/shims/ruby 

rbenv version # 2.2.3 (set by ~/.rbenv/version)
```

5. 安装 Jekyll 

	gem install jekyll 

6. Troubleshooting

> 运行`jekyll serve`时出现类似**cannot load such file -- redcarpet**字眼的错误,需要安装对于的软件,例如此时需要安装**redcarpet**

	gem install redcarpet

对应此类的错误,我依次还安装了**pygments.rb**,**jekyll-paginate**.

```bash
gem install pygments.rb 

# sudo yum install python-pygments # 如果以上命令仍没有解决关于**pygments**的错误，可能还需要执行此命令。

gem install jekyll-paginate
```

