---
layout: post
title:  "Mac上安装homebrew"
date:   2018-02-01
excerpt: "homebrew是osx的软件管理工具，rvm install时依赖homebrew"
image: ""
category: tools
---

## 官方安装命令
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 使用镜像安装
网络太慢，git fetch… 报错，下载安装homebrew超时，所以使用镜像安装。

homebrew主要分两部分：git repo（位于GitHub）和二进制bottles（位于bintray），这两者在国内访问都不太顺畅。可以替换成国内的镜像

获取install文件
```
cd ~
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install >> brew_install
```

编辑install文件，注释／替换 ```BREW_REPO```和```CORE_TAP_REPO```

```
#!/System/Library/Frameworks/Ruby.framework/Versions/Current/usr/bin/ruby
# This script installs to /usr/local only. To install elsewhere you can just
# untar https://github.com/Homebrew/brew/tarball/master anywhere you like or
# change the value of HOMEBREW_PREFIX.
HOMEBREW_PREFIX = "/usr/local".freeze
HOMEBREW_REPOSITORY = "/usr/local/Homebrew".freeze
HOMEBREW_CACHE = "#{ENV["HOME"]}/Library/Caches/Homebrew".freeze
HOMEBREW_OLD_CACHE = "/Library/Caches/Homebrew".freeze
# BREW_REPO = "https://github.com/Homebrew/brew".freeze
BREW_REPO = "git://mirrors.ustc.edu.cn/brew.git".freeze
# CORE_TAP_REPO = "https://github.com/Homebrew/homebrew-core".freeze
CORE_TAP_REPO = "git://mirrors.ustc.edu.cn/homebrew-core.git".freeze
```


安装homebrew：
```
$ /usr/bin/ruby ~/brew_install
```

中途可能Tapping homebrew/core，下载超时，此时已经可以使用brew命令
```
$ brew -v
Homebrew 1.5.4
Homebrew/homebrew-core N/A
```

使用```brew doctor```检查homebrew，各模块是否正常。此时会重新```tapping homebrew/core```

```
Warning: Suspicious Homebrew/brew git origin remote found.
```
是因为源是镜像，所以警告不安全

```
Warning: Unbrewed header files were found in /usr/local/include.
```
是因为node不是用brew安装的，可以卸载node，重新安装

