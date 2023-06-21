---
title: "用ARM盒子 再次搭建一个K8s 集群"
description: 
date: 2023-06-21T11:09:58+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---

## 前言
两年前买了四台树莓派4B 组了一个K8s 集群，在知乎，掘金发了两篇搭建的教程，一直都有人收藏，网上的教程也很少，今年年初看到树莓派溢价非常多，几乎没有做什么应用搭建在上面，就拿过来组集群写些教程使用了，所以索性咸鱼几乎翻倍都出了。
最近看到随身wifi棒子特别的火，又有人搭建利用随身wifi的Docker Swarm集群，自己有想动手弄一个，又在咸鱼淘了四个处理器为RK3568的机器，处理器配置比树莓派配置要好，但是只有4G内存，主要是才90元一个，买了12V DC电源。不到400。
![img.png](img.png)

系统是自带的Armbian:
```shell
root@panther-x2-main:~# uname -a
Linux panther-x2-main 6.1.28-rockchip64 #1 SMP PREEMPT Thu May 11 14:04:52 UTC 2023 aarch64 aarch64 aarch64 GNU/Linux
```
Armbian是轻量级的Debian系统，Linux有两大分支系列，一个是Debian 一个是Redhat。每个系列又有不同的更细的分支。
Redhat是商业公司维护的发行版本，Debian是社区组织维护的发行版本
一般个人折腾用Debian 社区强大，公司的生产环境用Centos。

![img_1.png](img_1.png)

## 开始



