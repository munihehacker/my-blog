---
title: "制作自己的composer包，搭建satis私有仓库"
description: 
date: 2023-01-30T11:55:51+08:00
image: composer-title.jpg
math: 
license: 
hidden: false
comments: true
draft: true
tags: ["PHP", "Composer", "Git"]
categories: ["杂技浅尝"]
---

composer相当于PHP 包管理器

# 前提条件
安装composer  
![](composer-version.png)
安装Git
![](git-v.png)
# 制作composer包
先找到一个空目录，执行composer 初始化命令：
```
composer init 
```

然后出现一连串的询问：
1.This command will guide you through creating your composer.json config.`
Package name (<vendor>/<name>) :

填写自己的包名,我填写的是：koala9527/composer-build

2.Description []:

需要填写这个包的描述信息，我填写的是：just test

3.Author [**** <koala9217@qq.com>, n to skip]:

提示填写作者信息，可以直接跳过输入n

4.Minimum Stability []:

提示填写最小稳定版本，可输入内容为：stable, RC, beta, alpha, dev，稳定性依次从左到右从大到小，越左边越稳定bug越少，自己测试的包就输入：dev

5.Package Type (e.g. library, project, metapackage, composer-plugin)[]: library

提示填写项目类型

> library: 这是默认类型，它会简单的将文件复制到 vendor 目录。
project: 这表示当前包是一个项目，而不是一个库。
metapackage: 当一个空的包，包含依赖并且需要触发依赖的安装，这将不会对系统写入额外的文件。因此这种安装类型并不需要一个 dist 或 source。
composer-plugin: 一个安装类型为 composer-plugin 的包，它有一个自定义安装类型，可以为其它包提供一个 installler。详细请查看 自定义安装类型。
>
这里就是填的默认类型

5.License[]:
提示填写开源许可证
参考下图：
![](License.png)

填写一个最宽松的MIT

6.Would you like to define your dependencies (require) interactively [yes]？

提示是否需要设置依赖环境或者其他包，输入yes

7.Search for a package：

提示搜索哪个依赖?依赖PHP,输入php

8.Enter the version constraint to require (or leave blank to use the latest version)：

需要哪个版本？注意最好是精确输入自己电脑环境一样的PHP版本，我这里是7.4.30，不然会出现一下报错：
![](composer-init-error.png)

9.重复询问7和8的问题，直接回车跳过

10.Add PSR-4 autoload mapping? laps namespace "Koala9527\Composerbuild"to the entered relative path. [src/, n to skip]:

是否添加PSR-4自动加载映射，将命名空间 “Koala9527\Composerbuild “映射到输入的相对路径， 这里输入了n 跳过，在后面composer.json自己增加autolaod的配置

接下来就会预览这个composer包composer.json的预览内容,下面会再次询问是否确认生成

11.Do you confirm generation[yes]?
 
 确认生成 yes

12. Would you like to install dependencies now [yes]?

是否先择安装依赖，输入yes ,没有其他依赖会直接完成


最后整个过程截图：
![](composer-init.png)

生成的文件目录结构：

```
  |-- composer-build',
      |-- composer.json',
      |-- composer.lock',
      |-- package-lock.json',
      |-- vendor',
          |-- autoload.php',
          |-- composer',
              |-- autoload_classmap.php',
              |-- autoload_namespaces.php',
              |-- autoload_psr4.php',
              |-- autoload_real.php',
              |-- autoload_static.php',
              |-- ClassLoader.php',
              |-- installed.json',
              |-- installed.php',
              |-- InstalledVersions.php',
              |-- LICENSE',
              |-- platform_check.php',
  ''
```
此时这个composer包没有任何内容，我们需要给他添加一个测试的功能代码

新建功能代码之前需要在composer.json添加pst-4自动加载配置，在项目根目录composer.json添加以下代码：
```
    "autoload": {
        "psr-4": {
            "Koala9527\\ComposerBuild\\": "src/ComposerBuild"
        }
    }
```
然后在项目更目录新建src 目录，在src目录下新建ComposerBuild目录，然后再在ComposerBuild目录下新建Test.php文件，文件内容为：
```
<?php
namespace ComposerBuild;
class Test
{
    function test()
    {
        echo "Form Test -> test()";
    }
}
```

然后执行` composer dump-autoload`命令 自动加载类

最后在根

# 上传GitHub
项目需要公开

# 搭建satis仓库


# 参考教程
https://hub.docker.com/r/composer/satis  
https://zhuanlan.zhihu.com/p/542952527