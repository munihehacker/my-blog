---
title: "Mysql Psot"
description: 
date: 2023-02-24T10:23:44+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: true
---


# Mysql 版本

目前都是使用的8.0，了解一下8.0对于之前的旧版本的新特性

8.0版本默认字符集将由 latin1 改为 utf8mb4

参考链接：[The MySQL 8.0.0 Milestone Release is available](https://dev.mysql.com/blog-archive/the-mysql-8-0-0-milestone-release-is-available/)


# 储存引擎

MySQL 5.5 版本之后开始采用 InnoDB 为默认存储引擎，之前版本默认的存储引擎为 MyISAM

存储引擎操作的对象是表，存储引擎的功能是接收上层传下来的指令，然后对表中的数据进行读取或写入的操作

区别：

InnoDB 支持事务，MyISAM 不支持（InnoDB每个语句就是一个事务）

InnoDB支持表级锁、行级锁，默认为行级锁；而 MyISAM 仅支持表级

InnoDB 必须要有主键，MyISAM可以没有主键（InnoDB不建索引的话会隐藏的生成一个 6 byte 的 int 型的索引作为主键索引。）

InnoDB是聚集索引，MyISAM是非聚集索引（索引和数据文件是分离）（聚集索引查询数据速度快，使用较少的内存）

# 查询语句生命周期

1.sql通过网络通信协议，从客户端发磅到服务端。

2.服务端收到sql后，查询缓存是否有匹配结果，若存在缓存，则直接取出缓存中结果返回，

否则进行下一阶段。

3.服务端对sql进行解析、预处理，然后生成查询计划。

4.执行引擎根据优化器生成的查询计划，调用存储引擎的API接口，拿到查询结果，然后返回

给客户端，同是缓存查询结果，不过缓存查询结果需要手动配置才会生效。



# 常用的数据类型
## 提前了解字符集

Mysql常见字符集：
* utf8mb4
utf8字符集，使用1～4个字节表示字符。mb4就是most bytes 4的意思，专门用来兼容四字节的unicode，utf8mb4是utf8的超集,兼容性比价好，可能会耗费更多的存储空间
```
CREATE TABLE `user` (
  `id` int unsigned NOT NULL AUTO_INCREMENT COMMENT '主键',
  `name` varchar(10) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '姓名',
    ...
  UNIQUE KEY `id_card` (`id_card`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=147492 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
* utf8 1～3字节
  
* utf8mb3 等价于 utf8

中文是占 3 个字节，其他数字、英文、符号占一个字节。emoji 符号占 4 个字节，一些较复杂的文字、繁体字也是 4 个字节

navicate 新建数据库需要置顶字符集和排序规则：

如果创建数据库时没有显式的指定字符集和比较规则，则该数据库默认用服务器的字符集和比较规则

测试服库默认字符集和排序规则：

utf8mb4

utf8mb4_0900_ai_ci

常见排序规则  utf8mb4_unicode_ci  ，utf8mb4_general_ci

常见字符集： utf8mb4，utf8

需要统一，建库的时候统一字符集和排序规则  

如果创建表时没有显式的指定字符集和比较规则，则该表默认用数据库的字符集和比较规则

比较规则的作用通常体现比较字符串大小的表达式以及对某个字符串列进行排序中

回顾一下计算机储存单位：

1个bit 只能存 0或者1

8位(bit)=1字节(Byte)

1024字节=1KB

int   

整数值，支持2147483648～2147483647（如果是UNSIGNED，

为0～4294967295）的数

tinyint

 整数值， 128～127（如果为UNSIGNED，为0～255）的数值

char  

1～255个字符的定长串。它的长度必须在创建时指定，否则MySQL

假定为CHAR(1)

varchar 

长度可变，最多不超过255字符数。如果在创建时指定为VARCHAR(n)，

则可存储0到n个字符的变长串（其中n≤255）

utf-8 编码 中1个汉字1个字母 就是占用1个字符数（实测）

text  

最大长度为64 K的变长文本 指针类型

DATE  

表示1000-01-01～9999-12-31的日期，格式为

YYYY-MM-DD

TIME

格式为HH:MM:SS

datetime  

DATE和TIME的组合

ENUM

接受最多64 K个串组成的一个预定义集合的某个串

decimal

精度可变的浮点值

varchar 和text 小数据量的查询和插入几乎性能一样

# 索引，索引类型，特点

B+树

最左 匹配原则

联合索引

联合索引有哪些好处？

(1) 减少开销。

建一个联合索引(tcol01, tcol02, tcol03)，相当于建立三个索引(tcol01)、(tcol01, tcol02)、(tcol01, tcol02, tcol03)的功能。每个索引都会占用写入开销和磁盘开销，对于大量数据的表，使用联合索引会大大的减少开销。

(2) 覆盖索引。

对联合索引(tcol01, tcol02, tcol03)，如果有如下的SQL，

(3) 效率高。

多列条件的查询下，索引列越多，通过索引筛选出的数据就越少。

# 锁，事务

事务功能就是让数据库操作符合现实世界中状态转换的规则,特性

原子性

隔离性

一致性

持久性

# 当前的数据库配置情况

内网机器测试服：

100.66.130.52（192.168.31.83）

配置情况：

容器环境

E5-2678 v3 @ 2.50GHz  * 2    二十四核心  64G RAM

使用情况：/disk01/shu_mysqlDB  183M

39个库

线上数据库1：

rm-wz9szmo511acr5c3kko.mysql.rds.aliyuncs.com

配置情况：

使用  47.107.158.24  SSH 通道跳转链接

19个库

线上数据库2：

8.129.117.240

配置情况：

20个库

# 探索：

主从，日志，备份，迁移，恢复，分库分表

# 讨论&优化方案

禁止建立预留字段？这种在那个情况有没有用呢？

冷热数据分离，不会经常读取的数据拎出来？

代码中一些问题，尽量避免地一些问题，归纳总结

1.储存相同数据的列名和列类型必须一致

2.尽量减少 select *  

3.尽量用数值代替字符串类型，字符串需要逐个对比每一个字符，数值只需要比较一次。增加储存开销和查询性能

4.varchar 代替char ,

5.避免使用EMUN 枚举类型，order by 效率低。

6.where 避免使用null ，也就是设计字段时默认值避免使用null ，is null ，is not null 优化器会放弃查询索引，使用null 查询也不直观

7.避免使用where !=  ,where <> 很可能会让索引时生效

8.优先使用inner join ，只保留两表完全匹配的结果集。

9.避免长事务，会进行加锁，影响相关业务

10.联合索引长度小的列放在左侧，最左特性

11.控制表的数据量，修改表结构，加索引，备份，恢复会出现问题

归档方案，分库分表

12.所有的表和字段都要有注释，

13.大表经常查询要确认好索引，可以拿出来讨论啊，排序，主要where 条件加索引

14.修改删除重要数据，要先备份

15.尽量把列的默认值定义为not NULL

16.伪删除设计？

17.索引不适合建立在大量重复的数据的字段上，例如性别，月份，不适合

# 参考：

MySQL 是怎样运行的：从根儿上理解 MySQL

MySQL 性能调优必知必会

《高性能MySQL 第3版》

《MySQL必知必会》

