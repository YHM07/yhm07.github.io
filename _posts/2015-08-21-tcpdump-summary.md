---
layout: post
title:  "tcpdump summary"
keywords: ""
description: ""
category: "linux" 
tags: [tcpdump, linux, cmd]
---

**tcpdump**是一个抓包工具，用于抓取互联网上传输的数据包；更详细的说，
tcpdump是一个嗅探器(sniffer),利用以太网的特性，通过将网卡适配器(NIC)
置于混杂模式(promiscuous)来获取传输在网络中的信息。

<!-- more -->

**注：使用tcpdump抓包，需要工作在root账户下，因为只有root才有权限将网卡
变更为"混杂模式"。**

*以下内容参考：[神探tcpdump系列][1]*
[1]: http://roclinux.cn/?p=2474

- **事例**

`# tcpdump -i eth0 -nn -X 'port 53' -c 1`

> -i选项

> 是interface的意思，指定tcpdump监听那一块网卡，这在一台服务器上有多块网卡是很有必要。

> -nn选项

> 当tcpdump遇到协议号或端口号时，不要将这些号码转换成对应的协议名称或端口名称。

> -X选项

> 需要把协议头和包内容原原本本的显示出来，tcpdump会以16进制或ASCII的形式显示。

> -c选项

> 是count的含义，tcpdump会抓取指定数量的包。

> 'port 53'

> 过滤规则，指只抓取源端口或目的端口是53(DNS)的数据包。

- **不同选项**

> -e选项

> 增加以太网(Ethernet)帧头部信息。

> -l选项

> 使得输出变为行缓冲。这样可以确保tcpdump遇到的内容一旦是换行符即将缓冲的
内容输出到标准输出，以便于利用管道或重定向方式进行后续处理。

- 实例：对于tcpdump输出的内容，提取每一行的第一个域，即"时间域" ，输出。

`# tcpdump -i eth0 -l | awk '{print $1}'`

> -t选项

> 输出时不打印时间戳

> -v选项

> 输出更详细的信息

> -F选项

> 指定过滤表达式所在的文件

- 实例：

`cat filter.txt`

port 53

`# tcpdump -i eth0 -nn -X -c 1 -t -F filter.txt`

> -w选项 

> 将流量保存到文件中

> -r选项

> 读取raw packets文件

- 实例

`# tcpdump -i eth0 -w flowdata`

`# tcpdump -r flowdata` 

- 过滤表达式

> 过滤表达式起到网络包过滤的作用，分三种过滤条件：类型，方向和协议。可通过man pcap-filter查看详细说明。

1. **实例**

- 只抓取UDP包

`# tcpdump -i eth0 -c 10 'udp'`

- 抓取某一源机器或目的机器的网络包

`# tcpdump -i eth0 'dst 8.8.8.8'`

- 查看某一端口的网络包，比如查看目的机器53或80端口

`# tcpdump -i eth0 'dst port 53 or dst port 80'`

**注：tcpdump 也支持如下类型：host指定主机名或IP地址；net指定网段；
portrange指定端口区域，比如src or dst portrange 6000-6008**

- 抓取通过eth0网卡，来源是iplayboy.tk或目标是iplayboy.tk的网络包

`# tcpdump -i eth0 'host iplayboy.tk'`

- 抓取通过eth0网卡，且iplayboy.tk和baidu.com之间通讯的网络包，或者是
iplayboy.tk和qiyi.com之间通讯的网络包

`# tcpdump -i eth0 host iplayboy.tk and (baidu.com or qiyi.com)`

- 抓取ftp端口和ftp数据端口的网络包

`# tcpdump -i eth0 'port ftp or ftp-data'`

- 抓取iplayboy.tk 和 baidu.com 之间建立TCP三次握手中的第一个包，即带有SYN
标记位的网络包，另外，目的主机不是qiyi.com

`# tcpdump 'tcp[tcpflags] & tcp-syn != 0 and not dst host qiyi.com'`

- 打印IP包长超过576字节的网络包

`# tcpdump 'ip[2:2] > 576'`

- 打印广播包或多播包，同时数据链路层不是通过以太网媒介进行的

`# tcpdump 'ether[0] & 1 = 0 and ip[16] >= 224'`


