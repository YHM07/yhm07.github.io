---
layout: post
title: "CentOS配置ssh"
category: "Linux"
tags: [CentOS,SSH]
---

# 把ssh默认远程连接端口修改为2222

参考：[CentOS 6.0修改ssh远程连接端口](http://www.osyunwei.com/archives/672.html)

<!-- more -->
方法如下：

### 1、编辑防火墙配置：`vi /etc/sysconfig/iptables`

**防火墙增加新端口2222**

```bash
-A INPUT -m state --state NEW -m tcp -p tcp --dport 2222 -j ACCEPT
# ======================================================================
# Firewall configuration written by system-config-firewall
# Manual customization of this file is not recommended.
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 2222 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
```

重启防火墙,使配置生效:

	/etc/init.d/iptables restart

	service iptables restart

### 2、备份ssh端口配置文件

	cp /etc/ssh/ssh_config   /etc/ssh/ssh_configbak

	cp /etc/ssh/sshd_config  /etc/ssh/sshd_configbak

修改ssh端口为：2222

	vi /etc/ssh/sshd_config

在端口#Port 22下面增加Port 2222

	vi /etc/ssh/ssh_config

在端口#Port 22下面增加Port 2222

重启：

	/etc/init.d/sshd restart

	service sshd restart

**用2222端口可以正常连接之后，再返回去重复上面的步骤。把22端口禁用了，以后ssh就只能用2222端口连接了！增强了系统的安全性。**

### 3、禁止root通过ssh远程登录

	vi /etc/ssh/sshd_config

找到PermitRootLogin，将后面的yes改为no，把前面的注释#取消，这样root就不能远程登录了！
可以用普通账号登录进去，要用到root的时候使用命令su root 切换到root账户

### 4、限制用户的SSH访问

假设我们只要root，user1和user2用户能通过SSH使用系统，向sshd_config配置文件中添加

	vi /etc/ssh/sshd_config

	AllowUsers root user1  user2


### 5、配置空闲超时退出时间间隔

用户可以通过ssh登录到服务器，你可以设置一个空闲超时时间间隔。
打开sshd_config配置文件,设置为如下。

	vi /etc/ssh/sshd_config

	ClientAliveInterval 600
	ClientAliveCountMax 0

上面的例子设置的空闲超时时间间隔是600秒，即10分钟，
过了这个时间后，空闲用户将被自动踢出出去（可以理解为退出登录/注销）。

### 6、限制只有某一个IP才能远程登录服务器

	vi /etc/hosts.deny     #在其中加入sshd:ALL

	vi /etc/hosts.allow    #在其中进行如下设置：sshd:192.168.1.1     #(只允许192.168.1.1这个IP远程登录服务器)

最后重启ssh服务：

	/etc/init.d/sshd restart
