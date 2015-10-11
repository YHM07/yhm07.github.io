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

* linux/Unix: ext2, ext3, ext4, reiserfs(处理小型文件系统强), xfs(CentOS), jfs
* 光盘文件系统: ISO9660
* 集群文件系统: GFS2(RedHat), OCFS2(Oricle)
* 网络文件系统: NFS, CIFS(通用互联网文件系统)
* windows文件系统: VFAT(FAT, FAT32), NTFS

<!-- more -->

## 分区简介

首先介绍一下硬盘类型:
* IDE, ATA (速率：133Mbps,并行总线)
* SCSI (速率: 320Mbps, 并行总线)
* SATA (速率: 6Gbps, 串行总线)
* USB2.0 (速率: 480Mbps)
不同硬盘类型,在linux系统上有不同命名方式,IDE类型硬盘:**/dev/hda**; 
SATA,USB,SCSI以及SAS都以**/dev/sda**方式命名.

另外,硬盘出厂之前首先会对硬盘以扇区为最小单位进行低级格式化,每个扇区512字节.其中,编号为'0'的那一块扇区称为"引导扇区(boot sector)",其中的446字节为'bootloader',而另外64字节为磁盘分区表,每16个字节标识一块分区,因此每个硬盘最多有四个主分区.

## 分区配置



