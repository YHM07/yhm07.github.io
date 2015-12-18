---
layout: post
title: "CentOS 配置ShadowSocks服务.md"
category: "Linux"
tags: [CentOS6.7,ShadowSocks]
---

参考：

* [Shadowsocks 使用说明](https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)

* [配置文件说明](https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File)

**ShadowSocks** 一个可以穿透防火墙的快速代理。

实验环境

```bash
$ cat /etc/centos-release 
CentOS release 6.7 (Final)
```

<!-- more -->

## ShadowSocks 服务端安装
---

CentOS

```bash 
$ sudo yum install python-setuptools && easy_install pip 
$ sudo pip install shadowsocks
```

附:Debian/Ubuntu安装

```bash
$ sudo apt-get install python-pip
$ sudo pip install shadowsocks
```

## ShadowSocks 服务端配置
---

```bash 
$ cat /etc/shadowsocks.json 
{
	"server":"0.0.0.0",
	"server_port":8989,
	"local_address": "127.0.0.1",
	"local_port":1080,
	"password":"password",
	"timeout":600,
	"method":"aes-256-cfb"
}
```

>
* server: 服务器端ip,可以用**0.0.0.0**
* server_port: 服务端端口号
* local_port: 本地端端口号
* password：设定密码
* timeout：设定超时时间(秒)
* method：加密方式,默认**aes-256-cfb**

## ShadowSocks 服务启动/关闭
---

```bash
# sudo ssserver -c /etc/shadowsocks.json -d start
# sudo ssserver -c /etc/shadowsocks.json -d stop 
```

如果没有配置文件可以使用命令行方式启动

	ssserver -p 443 -k passwd -m rc4-md5 

如果要后台运行,

	sudo ssserver -p 443 -k password -m rc4-md5 --user nobody -d start 

如果要停止

	sudo sserrver -d stop 

帮助文档

	sserrver --help 

日志文件:
	
	sudo less /var/log/shadowsocks.log 

## ShadowSocks 客户端配置
---

客户端下载:

* [ishadowsocks](http://www.ishadowsocks.com/)
* [shadowsocks](https://shadowsocks.org/en/download/clients.html)
* [shadowsocks-gui](https://github.com/shadowsocks/shadowsocks-gui)

对于linux操作系统,如*ubuntu,centos*可以按照**ShadowSocks 服务端安装**安装，这样默认会安装**sslocal**命令,关于该命令的使用如下:

帮助文档
	
	sslocal --help 

Proxy options:
>
* -c CONFIG              path to config file
* -s SERVER_ADDR         server address
* -p SERVER_PORT         server port, default: 8388
* -b LOCAL_ADDR          local binding address, default: 127.0.0.1
* -l LOCAL_PORT          local port, default: 1080
* -k PASSWORD            password
* -m METHOD              encryption method, default: aes-256-cfb
* -t TIMEOUT             timeout in seconds, default: 300
* --fast-open            use TCP_FASTOPEN, requires Linux 3.7+

启动ShadowSocks客户端

	sslocal -s server-ip -p server-port -l 1080 -k password -t 600 -m aes-256-cfb 

为了简化一些默认参数只要服务器端没有改变,客户端可以不用重复说明

	sslocal -s server-ip -p server-port -k password 

配合**nohup**和**&**可以后台运行,关闭终端不影响使用

	nohup sslocal -s server-ip -p server-port -l 1080 -k password -m aes-256-cfb &

## 附录
---

* [CentOS6.6安装ShadowSocks服务端][1]
* [在 centos6 上部署 shadowsocks 服务端][2]
* [Centos 7安装配置Shadowsocks][3]

[1]: http://www.centoscn.com/image-text/install/2015/0510/5399.html
[2]: https://vfasky.com/2013-05-17/shadowsocks-centos6/
[3]: https://www.ifshow.com/centos-7-installation-and-configuration-shadowsocks/
