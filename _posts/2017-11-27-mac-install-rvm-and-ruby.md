---
layout: post
title: Mac安装rvm升级ruby
date: 2017-11-27
excerpt: RVM能在系统中安装和管理多个 Ruby 版本。同时还能管理不同的 gem 集。支持 OS X、Linux 和其它类 UNIX 操作系统。curl安装rvm，rvm install 版本号
category: tools
---

## 安装 RVM
- 从服务器导公钥，需安装gpg，可以不用。
```
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

curl是利用URL语法在命令行方式下工作的开源文件传输工具

```
$ curl -sSL https://get.rvm.io | bash -s stable
```


- rvm官方推荐的方式安装
```
$ curl -L get.rvm.io | bash -s stable
```

- 载入 RVM 环境

```
$ source ~/.rvm/scripts/rvm
```
或者关闭终端，再重开一个

解释一下：
```
$ cat ~/.bash_profile
```
文件里要包含
```
[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session. 
[[condition]]
```
两层的方括号中间括着条件返回条件是不是真。-s是判断给定的文件是否存在的命令。这样一来，不就是在`[[ -s "$HOME/.vrm/scripts/vrm"]]`判断刚才安装的RVM是否存在吗？

接下来的&&符号是“短路的与”，当前面的条件是真的时候，执行后面的语句，返回这两个语句是不是全是真。在这里，利用了“短路”特性。也就是说当RVM已经安装的话，执行后面的.

`"$HOME/.rvm/scripts/rvm"`命令。这条命令和`source "$HOME/.rvm/scripts/rvm"`是一个意思：加载rvm的启动脚本

- 检查一下是否安装正确
```
$ rvm -v
rvm 1.22.17 (stable) by Wayne E. Seguin <wayneeseguin@gmail.com>, Michal Papis <mpapis@gmail.com> [https://rvm.io/]
```

`rvm requirements`命令，可以察看安装各版本时候的前提条件。

### 用 RVM 安装 Ruby
列出已知的 ruby 版本:
```
$ rvm list known
No rvm rubies installed yet. Try 'rvm help install'.
```

因为rvm不会管理系统自带的ruby

选择现有的 rvm 版本来进行安装（下面以 rvm 2.4.2 版本的安装为例）
```
$ rvm install 2.4.2

Searching for binary rubies, this might take some time.
No binary rubies available for: osx/10.12/x86_64/ruby-2.4.2.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for osx.
About to install Homebrew, press `Enter` for default installation in `/usr/local`,
type new path if you wish custom Homebrew installation (the path needs to be writable for user)
```

直接按回车，然后提示一堆，可能要装xcode命令行工具，输密码，安装homebrew。

Homebrew是一款Mac OS平台下的软件包管理工具，大部分情况下，它把软件包安装到 /usr/local/Cellar/ 下，然后在 /usr/local/bin 下建立符号链接。
