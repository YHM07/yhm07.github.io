---
layout: post
title:  "Linux(CentOS) 磁盘分区管理"
keywords: ""
description: ""
category: "Linux" 
tags: [linux, partition]
---

# Linux(CenOS)磁盘分区管理

## Linux支持的文件系统类型

如果想要知道linux支持的文件系统有哪些，可以查看下面这个目录

	ls -l /lib/modules/$(uname -r)/kernel/fs

目前系统已加载到内存中支持的文件系统则有:

	cat /proc/filesystems

Linux支持的文件系统类型介绍
* linux/Unix: ext2, ext3, ext4, reiserfs(处理小型文件系统强), xfs(CentOS), jfs
* 光盘文件系统: ISO9660
* 集群文件系统: GFS2(RedHat), OCFS2(Oricle)
* 网络文件系统: NFS, CIFS(通用互联网文件系统)
* windows文件系统: VFAT(FAT, FAT32), NTFS


<!-- more -->

## 磁盘分区简介

首先介绍一下硬盘类型:
* IDE, ATA (速率：133Mbps,并行总线)
* SCSI (速率: 320Mbps, 并行总线)
* SATA (速率: 6Gbps, 串行总线)
* USB2.0 (速率: 480Mbps)
不同硬盘类型,在linux系统上有不同命名方式,IDE类型硬盘:**/dev/hda**; 
SATA,USB,SCSI以及SAS都以**/dev/sda**方式命名.

另外,硬盘出厂之前首先会对硬盘以扇区为最小单位进行低级格式化,每个扇区512字节.其中,编号为'0'的那一块扇区称为"引导扇区(boot sector)",其中的446字节为'bootloader',而另外64字节为磁盘分区表,每16个字节标识一块分区,因此每个硬盘最多有四个主分区.

## 磁盘分区,格式化,挂/卸载以及磁盘查看

linux与分区相关的文件有'/etc/fstab'. '/etc/mtab', '/proc/partitions', '/proc/mounts'.
其中,'/etc/fstab'存储的是系统开机默认要挂载的文件系统,因此如果希望新分区
开机自动挂载,可以将新分区按照特定格式写入该文件.该文件包括6个字段,分别是:
* 第一列: 磁盘设备文件名或该设备的Label(可以通过dump2fs命令查看)
* 第二列: 挂载点(mount point),即挂载新分区的目录
* 第三列: 磁盘分区的文件系统类型
* 第四列: 文件系统参数
* 第五列: 能否被dump备份命令作用,'0'代表不做dump备份,'1'每天进行dump备份,'2'不定日期做dump备份操作
* 第六列: 是否以fsck检验扇区,'0'不做检验,'1'首先检验,只有根目录才可以标记为'1','2-9'其他要教研的文件系统编号可以是2-9,按编号大小顺序依次校验.

其中,'/etc/mtab'存储的是系统已经挂载的分区信息。

### 磁盘分区

fdisk [options] device

> -l 查看分区表

	fdisk /dev/sdb # 假设有一块硬盘为sdb,在使用该命令的时候sdb后没有数字

> fdisk 命令参数:

> n 新建分区

> d 删除分区

> l 列出已知的分区类型

> p 显示分区表

> t 更改分区系统类型,比如83(一般的linux类型),8e(lvm)

> w 写入分区表

> q 退出分区操作

### 磁盘格式化

mkfs [-t 文件系统类型] 设备文件名

mke2fs [-b Block Size] [-L 卷标] [-j 加入journal ]


### 磁盘挂/卸载

	mount [options] 设备文件名 mount_point

	umount 设备文件名 or mount_point

options:

> -a 依照配置文件'/etc/fstab'的数据将所有未挂载的磁盘挂载到系统

> -l 单纯输入mount会显示目前挂载的信息,加上'-l'可增加label名称

> -n 不将系统实时的挂载情况写入'/etc/mtab'

> -L 利用卷标名称(Label)进行挂载

> -o 额外的挂载参数,各个参数之间用','隔开

	* ro, rw: 挂载文件系统只读(ro),可读写(rw)
	* async, sync: 异步写入(默认,async), 同步写入(sync)
	* auto,noauto: 是否允许被'mount -a'自动挂载(默认auto)
	* dev, nodev: 是否允许此分区上可以创建设备文件,dev为允许
	* suid,nosuid: 是否允许此分区上含有'suid/sgid'文件格式
	* exec,noexec: 是否允许此分区上含有可执行binary文件
	* user,nouser: 是否允许此分区让任何用户执行'mount',一般来说，mount 仅有root可以运行.
	* defaults: 默认值为rw, suid, dev, exec, auto, nouser, and async
	* remount: 重新挂载

### 磁盘查看

	bldid 设备文件名 # 查看对应设备信息

	e2label 设备文件名 [Label] # 查看制定设备卷标或设置卷标

	tune2fs -l # 查看对应文件系统的所有信息,包括是否有坏块 -L Label # 修改文件系统的卷标 

	dump2fs -h 设备文件名 # 查看文件系统超级快或块组相关信息

## 内存交换空间(swap)的构建

**虚拟内存必须是独立文件系统**

首先,利用'fdisk 设备文件名'创建新分区,并将新分区的系统ID修改为'82',然后开始构建swap内存交换空间.

	mkswap 设备文件名 [-L Label] 

	swapon 设备文件名 # 启用该交换空间

	swapon -a # 启用所有标记为swap的独立分区

	swapoff 设备文件名 # 关闭该交换空间

	swapoff -a # 关闭所有标记为swap的独立分区

## 参考

1. 鸟哥的Linux私房菜-基础学习篇(第三版)

2. [文件系统及磁盘分区高级管理-\[马哥高薪Linux运维视频课程3][1]

[1]: http://edu.51cto.com/course/course_id-834.html
