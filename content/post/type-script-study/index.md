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
interface Data {
    name: string,
    age: number,
}

```

interface 是比较常用的，也叫接口，通畅使用大驼峰写法（每个单词首字母大写），普通变量使用小驼峰（除了第一个单词首字母小写，其他每个单词都是大写）
声明对象，

```typescript
const myData: Data = {
    name: 'aha',
    age: 18,
}
```

#### 可选属性

上面在接口定义元素时每个元素都是必须的，声明时不给某个元素赋值会报错，此时就有一个`可选属性`的方法：

```typescript
interface Data {
    name: string,
    age?: number,
}

const myData: Data = {
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

const myData: Data = {
    name: 'Boss',
    list: [
        {
            'name': 'A',
            list: [],
        },
        {
            'name': 'B',
            list: [],
        }
    ]
}
```

#### 接口的继承

类似于类的继承，继承父接口的所有属性，并进行追加属性

```typescript
interface Data {
    name: string,
    age: number,
}

interface TestData extends Data {
    isTrue: Boolean
}

const myData: TestData = {
    name: '蓝色妖姬',
    age: 80,
    isTrue: true
}
console.log(myData)  // { name: '蓝色妖姬', age: 80, isTrue: true }


```

如果不需要父接口的属性可以使用Omit 来去除父接口其中的属性  (如果有多个属性，用 | 来分隔开)

```typescript
interface Data {
    name: string,
    age: number,
    say: string
}

interface TestData extends Omit<Data, 'age' | 'say'> {
    isTrue: Boolean
}

const myData: TestData = {
    name: '切尔西',
    isTrue: true
}
console.log(myData)  // { name: '切尔西', isTrue: true }
```

### 类

对javascript 的类没有印象和的可以参考阮一峰的教程： [Class 的基本语法](https://es6.ruanyifeng.com/#docs/class)   
跟其他的语言的类的用法基本一致  
通过class关键字，可以定义类  
一般用类定义内置方法,内置变量来使用类，其中constructor() 是类的构造方法（就是实例化类之后自动执行的方法）

```typescript
// 类的定义也是大驼峰
class MyClass {
    constructor(name: string) {
        this.name = name
        console.log(name, '类被初始化！~')
    }

    name: string

    say(data: string) {
        console.log('我是', this.name, ',我要说:', data)
    }
}

const newClass = new MyClass('test');
newClass.say('牛逼')
// 打印结果：
// test 类被初始化！~
// 我是 test ,我要说: 牛逼
```

在Vue2中常见的this ，一般也是代表某个类，也就是当前组件实例。组件实例有很多属性和方法，例如：

* this.data是一个指向组件数据对象的引用，它包含了组件实例中定义的所有响应式数据。
* this.$props: 一个对象，包含当前组件接收的所有 props。可以通过this.$props.propName来访问父组件传递过来的 prop 值。
* this.$emit(event, payload): 触发当前组件上的自定义事件。可以使用该方法向父组件发送消息或触发其他组件监听的事件。
* this.$refs: 一个对象，包含通过 ref 注册的所有子组件或 DOM 元素的引用。可以使用this.$refs.refName来访问子组件或 DOM 元素。
* this.$watch(expressionOrFn, callback, [options]): 监听数据变化并触发回调函数。可以用于监听响应式数据或计算属性的变化。
* this.$nextTick(callback): 在下次 DOM 更新循环结束之后执行回调函数。当需要在更新后执行一些操作时，可以使用该方法。
* this.$router: 可以访问 Vue Router 的实例，用于进行路由导航操作。
* this.$route: 当前路由信息的对象，包含当前路径、参数等信息。
* this.$store: Vuex 状态管理库的实例，用于访问全局状态和派发/提交 mutations 和 actions。

#### 类的继承

最常见的一种使用发放，类的继承示例：

```typescript
// 这是一个基础类
class Base {
    name: string

    constructor(userName: string) {
        this.name = userName
    }
}

// 这是另外一个类，继承自基类
class Say extends Base {
    sayName() {
        console.log('我的名字是：', this.name)
    }
}

// 这个变量拥有上面两个类的所有属性和方法
const testName: Say = new Say('Honda')
testName.sayName()  // 我的名字是： Honda

```

#### 类的静态方法&静态属性

静态方法就是可以不用实例化可以直接调用类的静态方法和属性，常用于一些工具函数，不依赖于类的实例状态

```typescript
class TestClass {
    static myAge: number = 18

    static say() {
        console.log('牛逼666')
    }
}

TestClass.say() // 牛逼666
console.log(TestClass.myAge) // 18

```

必须保证子类在父类之后定义。

* 继承的时候必须在前面定义父类，后面写子类，不然会报错
* 类的方法内部如果含有this，它默认指向类的实例
* 箭头函数内部的this总是指向定义时所在的对象（箭头函数相当于匿名函数，并简化了函数定义，箭头函数没有原型prototype，因此箭头函数没有this指向，如果箭头函数的外层是一个普通函数，在定义的时候会继承它的this）

有些复杂的东西记录一下，后面遇到真实开发问题再填坑

#### 接口继承类

```typescript
class Person {
    name: string;

    constructor(name: string) {
        this.name = name;
    }

    sayHello() {
        console.log(`Hello, I'm ${this.name}`);
    }
}

interface IPerson extends Person {
}

const person: IPerson = new Person("John");
person.sayHello(); // Hello, I'm John
```

### 联合类型

大多数是指一个方法接受参数的时候可以接受多种数据类型，例如：

```typescript
function say(word: number | string) {
    console.log(`say: ${word}.`)
}

say(888)  // say: 888.
say('北京申奥成功了！')  // say: 北京申奥成功了！.
```

## 函数
函数非常重要，在TypeScript 有多种写法：
```typescript
// 函数声明，最基础用法
function add1(sum1: number, sum2: number): number {
    return sum1 + sum2
}

// 函数表达式，新手觉得难以理解的方法
const add2 = function (sum1: number, sum2: number): number {
    return sum1 + sum2
}

// 箭头函数，新手觉得更难以理解的方法
const add3 = (sum1: number, sum2: number): number => sum1 + sum2

// 对象里的方法
const obj = {
    add4(sum1: number, sum2: number): number {
        return sum1 + sum2
    }
}
//...
```
无返回值的函数返回值使用`void`代替。 void 和 null 、 undefined 不可以混用，如果的函数返回值类型是 null ，那么是真的需要 return 一个 null 值。

函数直接使用`return `返回,没有任何内容函数指定的返回值可以用`void`  

### 可选参数
入参的时候是可选的：
```typescript

function add5(sum1: number, sum2: number, isAdd?: boolean): number {
  return isAdd ? x + y : x - y
}
```

### 异步函数
异步函数这里有个非常重要的概念
异步函数的返回值需要用到Promise<T> 类型来定义它的返回值：
```typescript

function queryData(): Promise<string> {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('Hello World')
    }, 3000)
  })
}

queryData().then((data) => console.log(data))
// resolve指定返回的类型，如果没有resolve那么就要指定 Promise<void>
```

### 函数本身的类型
上面的匿名函数就没有类型，也就是箭头函数：
```typescript
const add = (num1: number, num2: number): number => x + y
// 左边只指定了add这个变量但是没有类型，原因是因为函数体会自动推导
```
### 任意值
TypeScript 也叫AnyScript，有很多不会写强类型语言的新手喜欢使用any 来表示函数的返回值或者入参，开发中应尽量避免使用

any 是一切类型的父类型，也是一切类型的子类型
```typescript
function say(msg: any) {
    console.log(String(msg))
}

```
遇到其他问题我们扩展讲讲，展示就只展示一个简单的例子

### 例外情况
很多npm 包都符合TypeScript 类型，但是有些个别的没有符合，把npm 包放在.ts文件运行就会报错，需要找到TypeScript 支持的特定的npm 包才可以。

### tsconfig.json
安装：
```shell
npm install -g typescript
```
初始化项目：
```typescript
tsc --init
```
```shell
Created a new tsconfig.json with:
                                                                               TS
  target: es2016
  module: commonjs
  strict: true
  esModuleInterop: true
  skipLibCheck: true
  forceConsistentCasingInFileNames: true


You can learn more at https://aka.ms/tsconfig.json

```
目录接口：
```shell
hello-node
│ # 构建产物
├─dist
│ # 依赖文件夹
├─node_modules
│ # 源码文件夹
├─src
│ # 锁定安装依赖的版本号
├─package-lock.json
│ # 项目清单
├─package.json
│ # TypeScript 配置
└─tsconfig.json
```
tsconfig.json：
```json
{
    "compilerOptions": {
        "target": "es6",
            "module": "es6",
            "outDir": "./dist"
    }
}

```
完整选项和功能参考官网：[tsconfig - typescriptlang](https://www.typescriptlang.org/tsconfig)
但是现在项目初始化都是使用vue-cli来新建，需要参考其他文档

未完待续。。。
