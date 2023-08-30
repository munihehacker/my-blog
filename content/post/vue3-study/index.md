---
title: "Vue3 学习笔记"
description: 
date: 2023-07-05T14:39:35+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: true
---

##  参考教程：


[VUE 官网](https://cn.vuejs.org/)  

[Vue3 入门指南与实战案例](https://vue3.chengpeiquan.com/)  

[BiliBili-尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通](https://www.bilibili.com/video/BV1Zy4y1K7SH/?vd_source=d81b0f1d86958760b70b7054b0ecfb07)  


## 前言
目的时学习UniApp ，需要前置学习一下Vue3,之前只了解了Vue2觉得落后太多了。很多概念不是很清楚，这里来基本整理一遍再开始实战

Vue的工程化，单一职责，高内聚，低耦合的特点

## 语法总结
首选创建一个官方推荐的脚手架：


使用 Vite 创建项目(主流)

```
npm create vite
```

```shell
PS D:\CodeProject\VueProject> npm create vite
Need to install the following packages:
  create-vite@4.4.1
Ok to proceed? (y) y
√ Project name: ... vite-project-typescript
√ Select a framework: » Vue
√ Select a variant: » TypeScript


```
设置项目名称：vite-project，vue类型项目，使用TypeScript 然后 cd 到vite-project-typescript, 安装依赖：
```typescript
npm install
```
启动项目：
```shell
npm run dev
```
![img_2.png](img_2.png)
![img_3.png](img_3.png)
访问http://127.0.0.1:5173/：
![img.png](img.png)


此时的目录结构：
![img_4.png](img_4.png)

有几个共识的文件统一目录：
* src： 源码
* src/main.ts: 入口文件
* src/view： 路由组件
* src/components：子组件目录
* src/route: 路由目录

