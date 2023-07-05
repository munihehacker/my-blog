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


## 前置知识TypeScript

TypeScript 主要是要做数据类型的约束，避免出BUG
变量后面加上冒号加上类型：
```
// 字符串
const str: string = 'Hello World'

// 数值
const num: number = 1

// 布尔值
const bool: boolean = true
```
ts 也会自动推到原始数据类型，所以上面的例子也可以省略：
```
const str = 'Hello World'
const num = 1
const bool = true
```

主要是要注意引用类型，数组


## 语法总结

```
# 全局安装脚手架
npm install -g create-preset
```

```

# 使用 `vue3-ts-vite` 模板创建一个名为 `hello-vue3` 的项目
preset init hello-vue3 --template vue3-ts-vite
```

使用 Vite 创建项目(主流)

```
npm create vite
```

