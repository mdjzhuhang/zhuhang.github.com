---
layout: post
title: mysql basic
excerpt: 下载 <a href=“https://dev.mysql.com/downloads/”>MySQL Community Server</a>，安装、配置、基本操作、语法、锁、事物
category: backend
---


### 下载

下载 MySQL Community Server

#### 版本

- MySQL Community Server，这个不要钱

- MySQL Enterprise，这个要掏钱，不过可以打电话咨询问题，也就是电话技术支持。

- MySQL Cluster，这个单独是没法用的，要在1或2的基础上用。当然用来平衡多台数据库的。

- MySQL Workbench，这是个好东西，用来设计数据库的

#### package

- DMG 的安装比较简单，只需要双击安装文件即可

- Compressed Tar 的安装请自行查找资料

### 安装

下载完成后，双击pkg文件安装，一路继续，标准安装，占用1.17GB。

当弹出一个MYSQL Installer提示框的时候，复制粘贴记下弹出框的密码：

`2018-01-05T02:29:54.982655Z 1 [Note] A temporary password is generated for root@localhost: @@@@`

### 配置

1、进入系统偏好设置

2、点击mysql

3、开启mysql服务

4、配置环境变量

```
$ mysql -u root -p
command not found
```

- 进入/usr/local/mysql/bin，编辑.bash_profile
```
$ vim ~/.bash_profile
```
- 添加：`export PATH=${PATH}:/usr/local/mysql/bin`

- 按esc，然后输入wq保存。

```
$ source ~/.bash_profile
```

### 操作

#### 登录
```
$ mysql -u root -p
```
退出 mysql> 命令提示窗口可以使用 exit 命令。

#### 修改密码
```
$ SET PASSWORD FOR 'root'@'localhost' = PASSWORD('newpass');
```
#### 更改mysql文件存放目录

mysql的配置文件

- linux 是 my.cnf，一般会放在 /etc/my.cnf，/etc/mysql/my.cnf

- windows 是 my.ini，一般会在安装目录的根目录

show variables like 'datadir%'”

### mysql 语法

### mysqladmin
- `creat XXX` 创建数据库

- `drop XXX` 删除数据库

- `use XXX- 选择数据库
- [数据类型](http://www.360sdn.com/mysql/2013/0511/78.html) ：大致分为三类：数值、日期/时间、字符串(字符)
- [创建数据表 CREAT TABLE](http://www.runoob.com/mysql/mysql-create-tables.html)

- 删除数据表：`DROP TABLE table_name ;`

- 插入数据：
```
INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN );
```
如果数据是字符型，必须使用单引号或者双引号，如："value"。

- [查询数据 SELECT](http://www.runoob.com/mysql/mysql-select-query.html)

#### delimiter

在命令行客户端中，如果有一行命令以分号结束，那么回车后，mysql将
会执行该命令

默认情况下，不可能等到用户把语句全部输入完之后，再执行整段语句。 因为mysql一遇到分号，它就要自动执行。

#### source
我们可以在 MySQL 数据库中执行在文件中的 SQL 代码。source 指令的缩写形式是：`\.`。
```
mysql> \. d:\pr_stat_agent.sql   
Query OK, 0 rows affected (0.00 sec)   
```

### mysql 锁、事务

在使用SQL时，大都会遇到这样的问题，你Update一条记录时，需要通过Select来检索出其值或条件，然后在通过这个值来执行修改操作。

但当以上操作放到多线程中并发处理时会出现问题：某线程select了一条记录但还没来得及update时，另一个线程仍然可能会进来select到同一条记录。

一般解决办法就是使用锁和事物的联合机制

例：
```
BEGIN TRAN

SELECT * FROM table WITH(TABLOCKX) 
// 或
SELECT * FROM table WITH(UPDLOCK, READPAST) 

UPDATE ....
COMMIT TRAN
```
