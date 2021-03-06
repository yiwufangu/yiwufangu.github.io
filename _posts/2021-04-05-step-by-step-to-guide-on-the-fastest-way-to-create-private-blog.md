---
layout: post
title:  "手把手教您如何快速创建个人博客"
date:   2021-04-05 -0600
categories: jekyll
tags: jekyll
---

GitHub Pages 基于Jekyll构建，使用GitHub的免费主机，你可以轻而易举地使用自定义域名在GitHub上免费发布网站。

## 一、简介

### 1.1 什么是GitHub Pages
GitHub Pages 是一个静态站点托管服务。Github Pages旨在直接从 GitHub 仓库中直接托管您的个人、组织或项目页面。了解关于 GitHub Pages 网站不同类型的更多信息，请参阅<a href='https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages'>用户、组织和项目页面</a>。

你可以使用 Jekyll Theme Chooser 在线创建和发布 GitHub Pages。

### 1.2 Jekyll 
Jekyll是一个简单静态博客网站生成器，可以将纯文本转换为静态博客网站。更多详情，可以<a href='http://jekyllcn.com/'>访问</a>。

* 简单
  不再需要数据库，不需要开发评论功能，不需要不断的更新版本——只用关心你的博客内容。
* 静态
  Markdown（或 Textile）、Liquid 和 HTML & CSS 构建可发布的静态网站。
* 博客支持
  支持自定义地址、博客分类、页面、文章以及自定义的布局设计。

几行命令，即可快速启动运行站点。
```shell
$ gem install jekyll bundler
$ jekyll new my-awesome-site
$ cd my-awesome-site

~/my-awesome-site $  bundle install
~/my-awesome-site $  bundle exec jekyll serve
```

### 1.3 RubyGems 
RubyGems是 Ruby 的一个包管理器，它提供一个分发 Ruby 程序和库的标准格式，还提供一个管理程序包安装的工具。RubyGems 旨在方便地管理 gem 安装的工具，以及用于分发 gem 的服务器。这类似于 Ubuntu 下的apt-get, Centos 的 yum，Python 的 pip。RubyGems大约创建于2003年11月，从Ruby 1.9版起成为Ruby标准库的一部分。


## 二、搭建本地调试开发环境

### 2.1 安装Ruby
大家可以在<a href='https://rubyinstaller.org/downloads/'>rubyinstaller.org</a> 自行下载，本文使用的版本是Ruby+Devkit 3.0.0-1 (x64)。

### 2.2 安装RubyGems 
点击下载<a href='https://rubygems.org/pages/download'>RubyGems</a> RubyGems, 下载完成后解压至你想放的位置，进入解压目录，执行 ruby setup.rb。本文安装版本是3.2.15。

### 2.3 安装MSYS2 
MSYS2 （Minimal SYStem 2） 是一个MSYS的独立改写版本，主要用于 shell 命令行开发环境。同时它也是一个在Cygwin （POSIX 兼容性层） 和 MinGW-w64（从"MinGW-生成"）基础上产生的，追求更好的互操作性的 Windows 软件。本文下载最新版本http://repo.msys2.org/distrib/x86_64/msys2-x86_64-20210228.exe。

### 2.4 安装Bundler Jekyll
bundler是一个管理其他Ruby gems的gem。它确保你的gems和gem版本兼容，并且确保包含每个gem需要的必须依赖。直接安装Bundler Jekyll,会提示Jekyll安装失败。
```shell
gem install bundler
gem install jekyll
```
继续执行 ```ridk install```

![img](/assets/20210405/ridk-install.png){:height="80%"}

错误：无法锁定数据库：File exists。如果你确认软件包管理器没有在运行，可以删除 /var/lib/pacman/db.lck。

① 使用msys2 mingw64打开命令窗口执行：
```shell
rm -rf /var/lib/pacman/db.lck
```
② 运行 pacman -Syu

:: Synchronizing package databases...
error: failed retrieving file 'mingw32.db' from repo.msys2.org : Resolving timed out after 10000 milliseconds

### 2.5 MSYS2更换国内源
默认的 MSYS2 源升级软件或是安装新软件的特别的慢，国内有针对于MSYS2的镜像库。这里我们使用中科大的库。
msys64\etc\pacman.d 目录下有三个文件：
* mirrorlist.msys
  将Server = http://mirrors.ustc.edu.cn/msys2/msys/$arch/ 粘贴或者移动到最Primary最上面。
* mirrorlist.mingw64
  同样于，Server = http://mirrors.ustc.edu.cn/msys2/mingw/x86_64/
* mirrorlist.mingw32
  同样于，Server = http://mirrors.ustc.edu.cn/msys2/mingw/i686/

国内镜像更新完毕后，在MSYS2 MINGW64执行如下命令，重新安装 ridk install。
```
pacman -Syu
``` 
重新安装 ```ridk install```
![img](/assets/20210405/ridk-install-pacman-syy.png)

database file for 'ucrt64' does not exist (use '-Sy' to download)

在MSYS2 MINGW64执行指令 ```pacman -Syy```

![img](/assets/20210405/ridk-install-pacman-syy.png)

重新执行 ```ridk install```

:: 正在运行事务后钩子函数...
(1/1) Updating the info directory file...
Install MSYS2 and MINGW development toolchain succeeded

安装bundler  jekyll

![img](/assets/20210405/bundler-jekyll.png)

## 三、使用Jekyll生成静态网站博客
### 3.1 如何创建站点实例

选择您想要生成站点文件的路径，执行如下命令：
```shell
$ Jekyll new my-awesome-site 
```

### 3.2 启动站点
```shell
$ cd my-awesome-site
$ jekyll server  
```
启动站点后，提示如下错误：

![img](/assets/20210405/bundle_add_webrick.png) 

安装webrick
```
bundle add webrick
```
发现从Gems默认的镜像库，获取webrick，出现网络连接错误。
![img](/assets/20210405/bundle_add_webrick.png)

### 3.3 Ruby Gems 连接到国内镜像
```
gem sources --remove https://rubygems.org/
gem sources -a https://gems.ruby-china.com
```

如果你使用Gemfile和bundler,可以用 Bundler 的Gem源镜像命令。
```
$ bundle config mirror.https://rubygems.org https://gems.ruby-china.com
```
这样你不用改 Gemfile 的 source。

重新执行 jekyll server, 在浏览器输入http://127.0.0.1:4000/ 即可浏览刚刚创建的blog。
![img](/assets/20210405/jekyll_server_browser.png)

至此，Jekyll 安装完成。

## 四、GitHub 托管 Jekyll站点

### 4.1 创建GitHub仓库

登录GitHub，使用账号用户名创建仓库“用户名.github.io”，即可使用GitHub Pages 托管服务。在空白仓库中添加默认index.html, 在浏览器访问https://用户名.github.io/ ，效果如下图所示：

![img](/assets/20210405/github-pages.png) 

### 4.2 Jekyll 主题选择

Jekyll官网提供了大量博客模板，我们可以去挑选一个自己喜欢的博客模板，然后在这个博客基础上修改。
点击前往<a href='http://jekyllthemes.org/'>jekyll 主题官网</a>。


将该主题模板源码下载下来，使用命令行进入该目录执行jekyll server，执行成功。
```
cd D:\code\GitHub\codinfox-lanyon-dev
jekyll server
```
本地访问http://127.0.0.1:4000/lanyon/
![img](/assets/20210405/github-jekyll-publish.png)

### 4.3 GitHub托管

将本地可以运行的站点文件通过GitHub Desktop上传至仓库“用户名.github.io”。





