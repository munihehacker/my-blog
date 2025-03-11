---
title: "Cursor Auto Free使用教程"
description: 
date: 2025-03-11T10:34:20+08:00
image: title.png
math: 
license: 
hidden: false
comments: true
draft: false
---

## 

先介绍一下Cursor吧，Cursor 是一款基于 VS Code 的现代AI编程工具，它不仅继承了 VS Code 的强大特性，还提供了AI智能编程辅助功能，能帮助开发者更高效地完成日常开发工作。  

#TODO 首页截图

最近用Cursor用的比较多，想给大家分享如何最低成本使用Cursor，在linux.do 论坛上找到了很多分享如何使用Cursor技巧的文章。收益匪浅，其中最著名的还是cursor-auto-free这个项目，能自动走注册流程，无限体验新手14天pro体验

#TODO 首页付费

每个新账号有50次慢速高级请求，这个完全足够去做一个中小型项目的demo了。还有无限制的自动补全也挺好用的

开源地址：https://github.com/chengazhen/cursor-auto-free

6.2k star

我想整理一下逻辑原理流程
关闭Corsor
启动浏览器注册流程
填入邮箱，邮箱接受
CF自动邮箱前缀
转发到固定邮箱读取验证码
验证获取成功更换机器码。自动登录

重要的是过验证码


