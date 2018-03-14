---
layout: post
title: mysql workbench
excerpt: <a href=“https://www.jianshu.com/p/dc58a4efdd84”>创建数据库、表，写入数据</a>，导入导出.sql文件、.cvs文件
category: tools
---

#### 建数据库 Schemas
[collation 字符序、对方法](https://www.cnblogs.com/yjf512/p/4233601.html)

[charset和collation](http://zhongwei-leg.iteye.com/blog/899227)

- character set：我们常看到的 utf-8, GB2312, GB18030 都是相互独立的 character set. 即对 Unicode 的一套编码。
- collation：用于指定数据集如何排序，以及字符串的比对规则。unicode 是国际化最佳的选择。当然为了提高性能，有些情况下还是使用 latin1 比较好。
```
mysql 有两个支持 unicode 的 character set:
1、ucs2: 使用 16 bits 来表示一个 unicode 字符。
2、utf8: 使用 1~3 bytes 来表示一个 unicode 字符
```
[charset 和 collation 设置](https://www.2cto.com/database/201302/189920.html)：多个级别：服务器级、数据库级、表级、列级和连接级 

#### 建表Table

基本字段类型标识 [intrinsic column flags]

- PK: primary key (column is part of a pk) 主键
- NN: not null (column is nullable) 非空
- UQ: unique (column is part of a unique key) 唯一
- AI: auto increment (the column is auto incremented when rows are inserted) 自增

扩展数据类型标记 [additional data type flags, depend on used data type] 
- BIN: binary (if dt is a blob or similar, this indicates that is binary data, rather than text) 二进制 (比text更大的二进制数据)
- UN: unsigned (for integer types) 整数
- ZF: zero fill (rather a display related flag) 值中最有意义的字节总为0，并且不保存。带有小数占位符的数据相当于金额类型的数据。 

[创建 Model，生成 sql 语句](http://blog.csdn.net/soulandswear/article/details/60966808)


## 操作.sql 文件

数据库执行语句的脚本文件

mysql数据库导出的备份文件


[导入导出 .sql文件](http://blog.csdn.net/risoben/article/details/51077415)

新建数据库 --> management --> export/import --> 选择sql文件和操作的哪个数据库 --> refresh all

[open .sql文件](https://jingyan.baidu.com/article/546ae1853d0fab1148f28c7f.html)

新建数据库 --> open .sql 文件--> 第一行加`use dbname;` --> 点击⚡️--> refresh all

## 导入cvs文件

table 右键 Table Data Import Wizard

选择新建表时，导入后第一行会是列名，同时会是第一行row

设置主键，然后删除第一行

- 没有主键时无法删除
- workbench--preference--sql edit--safe update 要取消勾选，否则也无法删除

关闭这个表的窗口，会弹出是否保持，save changes


