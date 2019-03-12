---
layout: post
title: 搭建 Python+selenium+mitmproxy 爬虫开发环境
categories: Python
description: 新手在 macOS 上搭建 Python+selenium+mitmproxy 爬虫开发环境遇到的一些坑，以及用到的工具。
keywords: Python, selenium, mitmproxy, 爬虫
---

# macOS 中搭建 Python+selenium+mitmproxy 爬虫开发环境

最近学习并使用 Python + selenium 来搭建爬虫系统，刚刚入手踩了很多的坑，写下来以备不时之需，

## 安装 Python3.*

macOS 中自带 Python2.* ，我的系统中是 Python2.7，学习了几天才发现和最新的 3.* 版本不兼容 !- -，既然是学习，还是选择了更高的 3.* 版本，况且官方也更推荐 3.* 的版本。

**下载安装：**

到这里下载适合你的版本 <https://www.python.org/downloads/mac-osx/> 直接安装就可以了。

**使用**

安装完成后想要在终端中打开 Python3.* 的解释器不能再使用 `python` 了，而是使用 `python3`，相应的 `pip` 包管理工具的命令也变成了 `pip3`：

    puthon # 打开 python2.* 的解释器界面
    pip # 打开 python2.* 的包管理工具命令
    
    puthon3 # 打开 python3.* 的解释器界面
    pip3 # 打开 python2.* 的包管理工具命令

当然我们很少直接使用 mac 终端下的 Python 环境，而是使用虚拟环境。

**搭建虚拟环境**

搭建虚拟环境的方式有两种，第一种使用 Python 虚拟环境管理包 `virtualenv` 创建，第二中是使用 IDE 创建

**第一种 virtualenv：**

下载安装 virtualenv，在终端中运行如下命令：

    pip3 install virtualenv
    
创建一个项目目录，并进入到目录：

    Mac:~ michael$ mkdir myproject
    Mac:~ michael$ cd myproject/
    Mac:myproject michael$
    
创建一个独立的 Python 运行环境，命名为venv：

    virtualenv --no-site-packages venv
    
进入项目虚拟环境：

    source venv/bin/activate
    
退出项目虚拟环境：

    deactivate
    
** 第二种 使用 IDE 创建**

我使用的 IDE 是 PyCharm，PyCharm 提供了创建虚拟环境的功能，打开 PyCharm 点击创建项目，选择相应的 Python 版本然后点击确定就可以自动创建虚拟环境：

![PyCharm](https://jichao257.github.io/images/posts/python/pycharm_venv.png)

你可以看到 IDE 依然是使用的上面的命令进行创建的，所以进入和退出虚拟环境和第一种是一样的。

**使用虚拟环境进行开发的好处**
    
当我们进入项目虚拟环境后就可以直接使用 `python` 和 `pip` 来运行和管理我们的项目了，这个时候因为是在虚拟环境中，所以依然使用的是 3.* 的环境。

使用虚拟环境的另外的好处是开发环境的隔离，以及包依赖管理更加方便，而不是一个项目依赖很多不需要的包。

我们再安装一个包管理工具 `pipreqs` 吧：

    pip install pipreqs

在项目根目录下运行下面的命令就可以自动生成我们的项目依赖

    pipreqs ./ --force

这时候我们会得到项目的依赖文件 `requirements.txt`，这样如果我们迁移项目的话使用下面的命令就可以很轻松的安装依赖了：

    pip install -r ./requirements.txt


## 参考文献