---
layout: post
title: "Ubuntu14.04安装Jekyll"
category: "linux"
tags: [Jekyll, Ruby]
---

**Jekyll**可以独立的在本地运行，这样方便构建_GitHub_个人博客,官方介绍[安装][1]完成Jekyll只需要几分钟时间，但实际操作的过程当中发现存在很多问题，记录下来以备参考。
[1]: http://jekyll.bootcss.com/docs/installation/


## 安装依赖工具

---

**安装Jekyll最简单的方式是通过__RubyGems__安装，但这需要以下的依赖包：__Ruby__,__RubyGems__和__node.js__。以下介绍__ubuntu__系统安装。**

```
	$ sudo apt-get install ruby ruby1.9.1 ruby1.9.1-dev node.js
```

注：CentOS7.1 上安装node.js

首先需要安装EPEL库

	sudo yum install epel-release

然后安装node.js

	sudo yum -y install nodejs


## 安装Jekyll

---
终端运行以下命令

```
	$ gem install Jekyll
```

**如果遇到问题可能时因为没有安装必要的依赖,请认真检查错误提示，或查看[troubltshooting][3]**
[3]: http://jekyll.bootcss.com/docs/troubleshooting/

<!-- more -->

## 附加功能

---

**如果希望文章通过__highlight__标签实现代码高亮，需要安装__pygments__.**

```
	$ sudo apt-get install python-pygments
```


## Troubltshooting

---
**如果按照上述操作，理论上Jekyll已经正确安装，可以运行`jekyll -v`查看版本号
，切换到github目录运行`jekyll serve`就可以在本地查看博客**

但是，万事总有**但是**,在本机运行`jekyll serve`时出现错误。

> Celluloid 0.17.0 is running in BACKPORTED mode. [ http://git.io/vJf3J2 ]
> jekyll 2.5.3 | Error: wrong number of arguments (2 for 1) ]

百思不得其解，经google后解决问题。原链接是[Error while trying to run "Jekyll Serve"][2]
[2]: https://talk.jekyllrb.com/t/error-while-trying-to-run-jekyll-serve/933/3

解决方案如下：

- 首先，运行`jekyll serve --trace`查看问题所在，找到可能是版本问题。
- 其次，运行`gem list --local`查看本地已安装文件以及其版本号。
- 发现，Celluloid存在两个版本，分别是0.17.0 和 0.16.0,将0.17.0版本删除，解决问题。`gem uninstall elluloid`,然后选择对应的0.17.0版本将其删除。

附：CentOS7.1 安装Jekyll出现以下问题的解决方案

	ruby -v			# ruby 2.0.0p598 (2014-11-13) [x86_64-linux]

	gem -v			# 2.4.8

> Dependency Error: Yikes! It looks like you don't have jekyll-coffeescript installed

解决方案: 运行`gem install json`即可.
