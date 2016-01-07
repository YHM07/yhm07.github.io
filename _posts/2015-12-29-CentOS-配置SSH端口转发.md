---
layout: post
title: "CentOS配置SSH端口转发"
category: "Linux"
tags: [CentOS,SSH]
---

# ssh 端口转发

**参考：**
* [IBM:实战 SSH 端口转发](https://www.ibm.com/developerworks/cn/linux/l-cn-sshforward/)
* [小马过河: SSH 远程端口转发](http://lvii.github.io/system/2013/10/08/ssh-remote-port-forwarding/)
* [ssh端口转发实验](https://gist.github.com/suziewong/4420286)
* [SSH25个命令 + 深入SSH端口转发细节](http://blog.csdn.net/moubenmao_jun/article/details/10392061)

ssh 命令参数

> - -f 后台认证用户/密码，通常和-N连用，不用登录到远程主机;
> - -p 被登录的ssd服务器的sshd服务端口;
> - -L 本地机(客户机)的某个端口转发到远端指定机器的指定端口。工作原理是这样的, 本地机器上分配了一个 socket 侦听 port 端口，一旦这个端口上有了连接, 该连接就经过安全通道转发出去, 同时远程主机和 host 的 hostport 端口建立连接。可以在配置文件中指定端口的转发. 只有 root 才能转发特权端口;
> - -R 远程主机(服务器)的某个端口转发到本地端指定机器的指定端口. 工作原理是这样的, 远程主机上分配了一个 socket 侦听 port 端口,一旦这个端口上有了连接, 该连接就经过安全通道转向出去, 同时本地主机和 host 的 hostport 端口建立连接.可以在配置文件中指定端口的转发. 只有用 root 登录远程主机才能转发特权端口。
> - -D指定一个本地机器 “动态的'’ 应用程序端口转发.工作原理是这样的, 本地机器上分配了一个 socket 侦听 port 端口, 一旦这个端口上有了连接, 该连接就经过安全通道转发出去,根据应用程序的协议可以判断出远程主机将和哪里连接. 目前支持 SOCKS4 协议, 将充当 SOCKS4 服务器. 只有 root才能转发特权端口. 可以在配置文件中指定动态端口的转发。
> - -C压缩数据传输。
> - -N不执行脚本或命令，通常与-f连用。
> - -g允许远程主机连接到建立的转发的端口，如果不加这个参数，只允许本地主机建立连接。

e.g. 从某主机的 80 端口开启到本地主机 8080 端口的隧道

	ssh -N -L8080:localhost:80 远程主机
	
现在你可以直接在浏览器中输入http://localhost:8080 访问这个网站。

经常用到的三个转发命令是：

```bash
ssh -C -f -N -g -L listen_port:DST_Host:DST_port user@Tunnel_Host
ssh -C -f -N -g -R listen_port:DST_Host:DST_port user@Tunnel_Host
ssh -C -f -N -g -D listen_port user@Tunnel_Host
```

* **端口转发** port forwarding 只需要与服务器对应的端口建立 TCP 连接即可
* **隧道** tunnel 可不一样，需要依赖虚拟网卡 tun 设备来通讯

## 实例

iptables rules

```bash
[root@localhost ~]# iptables -t filter -nvL
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
 3079  200K ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0           state RELATED,ESTABLISHED 
   0     0 ACCEPT     icmp --  *      *       0.0.0.0/0            0.0.0.0/0           
   7   420 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0           
   3   180 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0           state NEW tcp dpt:22 
6668  663K REJECT     all  --  *      *       0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohibited
```

### 本地端口转发

情景1：

 > web服务器因为防火墙，如**iptables**的原因，80端口对外不开放，只允许本机访问，因此，外界所有主机均无法访问web服务器。现在希望主机**192.168.0.151**访问web服务。


 
解决方案:

在主机**192.168.0.151**运行以下命令：
	 
	 ssh -C -f -N -L 8080:localhost:80 root@192.168.0.161

or

	ssh -C -f -N -L 8080:192.168.0.161:80 root@192.168.0.161

然后通过**http://localhost:8080**即可访问web服务。

情景2：

 > web服务器因为防火墙，如**iptables**的原因，80端口只允许主机**192.168.0.151**访问，现在希望主机**192.168.0.181**访问web服务。

iptables rules

```bash
[root@localhost ~]# iptables -t filter -I INPUT 5 -p tcp -s 192.168.0.151 --dport 80 -m state --state NEW -j ACCEPT
```

解决方案:

在主机**192.168.0.181**运行以下命令：

	ssh -C -f -N -L 8080:192.168.0.161:80 root@192.168.0.151

然后通过**http://localhost:8080**即可访问web服务。

情景3：

 > web服务器因为防火墙，如**iptables**的原因，80端口只允许主机**192.168.0.151**访问，现在希望主机**192.168.0.181**访问web服务，另外，**192.168.0.100**也能够访问web服务，但是该主机没有ssh客户端。

解决方案:

在主机**192.168.0.181**运行以下命令：

	ssh -C -f -N -g -L 8080:192.168.0.161:80 root@192.168.0.151

然后即可在主机**192.168.0.100**上，通过**http://192.168.0.181:8080**访问web服务。

注：以上ssh参数-g的使用需要设置/etc/ssh/sshd_conf文件的**GatewayPorts yes**


###  远程端口转发

情景1：

> web服务器(**192.168.0.161**)因为防火墙，如**iptables**的原因，外网主机**192.168.0.181**无法访问该web服务器，但是反向可以访问，即web服务器可以访问主机**192.168.0.181**的ssh服务，现在希望主机**192.168.0.181**访问web服务。

解决方案：

在主机**192.168.0.161**，即web服务器上运行以下命令：

	ssh -C -f -N -R 8080:localhost:80 root@192.168.0.181

or

	ssh -C -f -N -R 8080:192.168.0.161:80 root@192.168.0.181

然后即可通过**http://localhost:8080**访问web服务。
