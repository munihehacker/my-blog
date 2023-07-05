---
title: "Uniapp 从零启动到 全局获取Openid的封装"
description: 
date: 2023-06-29T11:43:52+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: true
tags: ["uniapp","小程序"]
categories: ["前端"]
---

## 前言
最近在研究uniapp，一个可以生成多端的应用的工具，我主要是主要是生成微信小程序和支付宝小程序，自己也是在这方面工作，只不过是后端相关，最近开始研究前端相关的内容，已经上架了两个小程序，写的很拉跨，很多冗余的代码。因为没有参考成熟的项目，需要什么加什么，没有考虑封装共用，每一个页面都有请求接口需要获取用户的openid，js就写得很长。

今天尝试一下从0开始新建一个Uniapp项目到获取Openid得全局封装，说明一下没有参考其他教程项目，仅仅是个人的习惯方案。
