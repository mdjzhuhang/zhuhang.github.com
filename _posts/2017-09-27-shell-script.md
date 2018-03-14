---
layout: post
title: shell bash
excerpt: Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。<br>Shell 脚本（shell script），是一种为 shell 编写的脚本程序。本文出现的 "shell编程" 都是指 shell 脚本编程，不是指开发 shell 自身。
category: tools
---
## shell bash

Shell 编程跟 java、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。

Linux 的 Shell 种类众多，常见的有：

Bourne Shell（/usr/bin/sh或/bin/sh）

Bourne Again Shell（/bin/bash）--默认（bash）

C Shell（/usr/bin/csh）

K Shell（/usr/bin/ksh）

Shell for Root（/sbin/sh）
……

### bash
```
#!/bin/bash
echo "Hello World !"
```

`#!` 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。

### 运行 Shell 脚本的方法
1、作为可执行程序
将上面的代码保存为 test.sh，并 cd 到相应目录：

```
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```

一定要写成 ./test.sh，而不是`test.sh`，运行其它二进制的程序也一样，直接写 `test.sh`，linux 系统会去 PATH 里寻找有没有叫 `test.sh` 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 `test.sh` 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。

2、作为解释器参数
这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：
```
/bin/sh test.sh
/bin/php test.php
```
这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用


### 定义变量

变量名不加 $ 符号，变量名和等号之间不能有空格
```
your_name="runoob.com"
```
可以用语句给变量赋值，如：将 /etc 下目录的文件名循环出来。
```
for file in `ls /etc`
```
### 使用变量
变量名前面加美元符号即可

推荐给所有变量加上花括号，帮助解释器识别变量的边界

已定义的变量，可以被重新定义

如：
```
your_name="qinjx"
echo $your_name
echo ${your_name}
your_name="aaaaa"
echo $your_name
```


```
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done
```
如果不给skill变量加花括号，写成echo "I am good at $skillScript"，解释器就会把$skillScript当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。


readonly 命令可以将变量定义为只读变量，如：`readonly myUrl`

unset 命令可以删除变量。`unset variable_name`变量被删除后不能再次使用，unset 命令不能删除只读变量

[shell 字符串、数组](http://www.runoob.com/linux/linux-shell-variable.html)

### 字符串
```
# 单引号：原样输出，变量无效，其中不能再出现单引号
str='this is a string'
# 双引号：可以有变量，可以出现转义字符
str="Hello, I know your are \"$your_name\"! \n"

# 拼接字符串
your_name="qinjx"
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting $greeting_1

# 获取字符串长度
string="abcd"
echo ${#string} #输出 4
```

### 数组
```
# 定义
数组名=(值1 值2 ... 值n)

# 读取
${array_name[index]}

# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}

# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```
#### 注释
以"#"开头的行就是注释，sh里没有多行注释。

#### 传递参数
[在执行 Shell 脚本时，向脚本传递参数](http://www.runoob.com/linux/linux-shell-passing-arguments.html)

#### 运算符
[运算符](http://www.runoob.com/linux/linux-shell-basic-operators.html)

#### 命令
echo：用于字符串的输出

printf：用于输出

test：用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试
