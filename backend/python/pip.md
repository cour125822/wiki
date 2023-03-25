---
title: pip使用
description: 
published: true
date: 2023-03-06T06:57:45.730Z
tags: 
editor: markdown
dateCreated: 2023-02-26T04:20:45.843Z
---

pip 是 Python 包管理工具，该工具提供了对Python 包的查找、下载、安装、卸载的功能。

> Python 2.7.9 + 或 Python 3.4+ 以上版本都自带 pip 工具。

pip 官网：[https://pypi.org/project/pip/](https://pypi.org/project/pip/)

# 安装

可以通过以下命令来判断是否已安装：

```shell
pip --version     # Python2.x 版本命令 
pip3 --version    # Python3.x 版本命令
```

如果未安装，则可以使用以下方法来安装：

```shell
curl <https://bootstrap.pypa.io/get-pip.py> -o get-pip.py   # 下载安装脚本 
sudo python get-pip.py    # 运行安装脚本
```

用哪个版本的 Python 运行安装脚本，pip 就被关联到哪个版本，如果是 Python3 则执行以下命令：

```shell
sudo python3 get-pip.py    # 运行安装脚本。
```

一般情况 pip 对应的是 Python 2.7，pip3 对应的是 Python 3.x。

部分 Linux 发行版可直接用包管理器安装 pip，如 Arch：

```shell
sudo pacman -S python-pip
```

# pip 最常用命令

**显示版本和路径**

```
pip --version
```

**获取帮助**

```
pip --help
```

**升级 pip**

```
pip install -U pip
```

**Help**

如果这个升级命令出现问题 ，可以使用以下命令：

```
sudo easy_install --upgrade pip
```

**安装包**

```shell
pip install SomePackage							# 最新版本 
pip install SomePackage==1.0.4			# 指定版本 
pip install 'SomePackage>=1.0.4			# 最小版本
```

比如我要安装 Django。用以下的一条命令就可以，方便快捷。

```
pip install Django==1.7
```

**升级包**

```
pip install --upgrade SomePackage
```

升级指定的包，通过使用==, >=, <=, >, < 来指定一个版本号。

**卸载包**

```
pip uninstall SomePackage
```

**搜索包**

```
pip search SomePackage
```

**显示安装包信息**

```
pip show
```

**查看指定包的详细信息**

```
pip show -f SomePackage
```

**列出已安装的包**

```
pip list
```

**查看可升级的包**

```
pip list -o
```

# pip 升级

**Linux 或 macOS**

```shell
pip install --upgrade pip    # python2.x 
pip3 install --upgrade pip   # python3.x
```

**Windows 平台升级：**

```shell
python -m pip install -U pip   # python2.x 
python -m pip3 install -U pip    # python3.x
```

# pip 清华大学开源软件镜像站

使用国内镜像速度会快很多：

临时使用：

```
pip install -i <https://pypi.tuna.tsinghua.edu.cn/simple> some-package
```

例如，安装 Django：

```
pip install -i <https://pypi.tuna.tsinghua.edu.cn/simple> Django
```

如果要设为默认需要升级 pip 到最新的版本 (>=10.0.0) 后进行配置：

```
pip install pip -U pip config set global.index-url <https://pypi.tuna.tsinghua.edu.cn/simple>
```

如果您到 pip 默认源的网络连接较差，临时使用本镜像站来升级 pip：

```
pip install -i <https://pypi.tuna.tsinghua.edu.cn/simple> pip -U
```

# 注意事项

如果 Python2 和 Python3 同时有 pip，则使用方法如下：

Python2：

```
python2 -m pip install XXX
```

Python3:

```
python3 -m pip install XXX
```