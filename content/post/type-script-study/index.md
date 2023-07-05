---
title: "TypeScript语法简要总结"
description:
date: 2023-07-05T21:57:05+08:00
image: typescript-logo.png
math:
license:
hidden: false
comments: true
draft: false
tags: ["TypeScript"]
categories: ["前端"]
---

## 原因

想学UniApp不想用Vue2 ,想学习Vue3 ,但是前端必须是学习typeScript，Vue3 是用TypeScript重构的。

TypeScript 是JavaScript 的超集。

本篇文章只会作为快查手册，尽量减少废话。

## 变量定义

### 引用类型
TypeScript 主要是要做数据类型的约束，避免出BUG
变量后面加上冒号加上类型：

```typescript
// 字符串
const str: string = 'Hello World'

// 数值
const num: number = 1

// 布尔值
const bool: boolean = true
```

ts 也会自动推到原始数据类型，所以上面的例子也可以省略：

```typescript
const str = 'Hello World'
const num = 1
const bool = true
```

原始数据类型 是一种既非对象也无内置方法的数据.  
原始数据类型：
|原始数据类型 | JavaScript | TypeScript|
| --- | ---| ---|
|字符串 |String | string|
|数值| Number| number |
|布尔值| Boolean| boolean|
|大整数| BigInt| bigint|
|符号| Symbol| symbol|
|不存在| Null | null|
|未定义| Undefined |undefined|

布尔值，大整数，符号 还是很少见的
### 引用类型
主要是要注意引用类型，数组
举几个例子：
```typescript
// 字符串数组
const strs: string[] = ['Hello World', 'Hi World']

// 数值数组
const nums: number[] = [1, 2, 3]

// 布尔值数组
const bools: boolean[] = [true, true, false]
```

数组也会自动推导：
```typescript
const strs = ['Hello World', 'Hi World']
const nums = [1, 2, 3]
const bools = [true, true, false]
```
需要数组里的元素有值
也可以这样:
```typescript

const nums: number[] = [1, 2, 3]
// 如果是空的就必须要指定数组的元素类型
```


### 对象

对象的类型定义有两个语法： type 和 interface
定义对象：
```typescript

type Data = {
    name: string,
    age: number,
}

```

```typescript
interface  Data  {
    name: string,
    age: number,
}

```

interface 是比较常用的，也叫接口，通畅使用大驼峰写法（每个单词首字母大写），普通变量使用小驼峰（除了第一个单词首字母小写，其他每个单词都是大写）
声明对象，
```typescript
const  myData: Data= {
    name: 'aha',
    age: 18,
}
```

#### 可选属性
上面在接口定义元素时每个元素都是必须的，声明时不给某个元素赋值会报错，此时就有一个`可选属性`的方法：
```typescript
interface  Data  {
    name: string,
    age?: number,
}

const  myData: Data= {
    name: 'aha',
}
console.log(myData)
```

#### 调用自身接口
举个例子：
```typescript
interface Data {
  name: string
  // 这个属性引用了本身的类型
  list: Data[]
}
const myData:Data = {
    name: 'Boss',
    list:[
        {
            'name':'A',
            list:[],
        },
        {
            'name':'B',
            list:[],
        }
    ]
}
```



