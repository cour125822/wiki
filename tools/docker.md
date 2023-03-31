---
title: docker
description: 
published: true
date: 2023-03-26T08:03:14.697Z
tags: 
editor: markdown
dateCreated: 2023-03-26T07:47:25.497Z
---

# 介绍
> Docker并非是一个通用的容器工具，它依赖于已存在并运行的linux内核环境
> Docker实质上是在已经运行的Linux下制造了一个隔离的文件环境，因此它执行的效率几乎等同于所部署的Linux主机。因此，Docker必须部署在Linux 内核的系统上。如果其他系统想部署Docker就必须安装一个虚拟Linux环境。

> 在Windows上部署Docker的方法都是先安装一个虚拟机，并在安装Linux系统的的虚拟机中运行Docker。

![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303271205329.png)

## Linux容器
Linux容器(Linux Containers，缩写为 LXC)是与系统其他部分隔离开的一系列进程，从另一个镜像运行，并由该镜像提供支持进程所需的全部文件。容器提供的镜像包含了应用的所有依赖项，因而在从开发到测试再到生产的整个过程中，它都具有可移植性和一致性。
 
Linux 容器不是模拟一个完整的操作系统而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。

Docker容器是在操作系统层面上实现虚拟化，直接复用本地主机的操作系统，而传统虚拟机则是在硬件层面实现虚拟化。与传统的虚拟机相比，Docker优势体现为启动速度快、占用体积小。

## Docker 和传统虚拟化方式的不同之处：
### 实现方式
- 传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；
- 容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。
* 每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源

### 使用上
1. docker有着比虚拟机更少的抽象层
	由于docker不需要Hypervisor(虚拟机)实现硬件资源虚拟化,运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。
2. docker利用的是宿主机的内核,而不需要加载操作系统OS内核
	当新建一个容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。进而避免引寻、加载操作系统内核返回等比较费时费资源的过程,当新建一个虚拟机时,虚拟机软件需要加载OS,返回新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统,则省略了返回过程,因此新建一个docker容器只需要几秒钟。
 

> 解决了运行环境和配置问题的软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术。
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303271059215.png)

Docker的出现使得Docker得以打破过去「程序即应用」的观念。透过镜像(images)将作业系统核心除外，运作应用程式所需要的系统环境，由下而上打包，达到应用程式跨平台间的无缝接轨运作
# 优势
## 更快速的应用交付和部署
Docker化之后只需要交付少量容器镜像文件，在正式生产环境加载镜像并运行即可，应用安装配置在镜像里已经内置好，大大节省部署配置和测试验证时间。
## 更便捷的升级和扩缩容
 
随着微服务架构和Docker的发展，大量的应用会通过微服务方式架构，应用的开发构建将变成搭乐高积木一样，每个Docker容器将变成一块“积木”，应用的升级将变得非常容易。当现有的容器不足以支撑业务处理时，可通过镜像运行新的容器进行快速扩容，使应用系统的扩容从原先的天级变成分钟级甚至秒级。
## 更简单的系统运维
应用容器化运行后，生产环境运行的应用可与开发、测试环境的应用高度一致，容器会将应用程序相关的环境和状态完全封装起来，不会因为底层基础架构和操作系统的不一致性给应用带来影响，产生新的BUG。当出现程序异常时，也可以通过测试环境的相同容器进行快速定位和修复。
## 更高效的计算资源利用
Docker是内核级虚拟化，其不像传统的虚拟化技术一样需要额外的Hypervisor支持，所以在一台物理机上可以运行很多个容器实例，可大大提升物理服务器的CPU和内存的利用率

# Docker架构
>Docker是一个Client-Server结构的系统，Docker守护进程运行在主机上， 然后通过Socket连接从客户端访问，守护进程从客户端接受命令并管理运行在主机上的容器。 容器，是一个运行时环境，就是我们前面说到的集装箱。



![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303271111865.png)
## 详细
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303271110526.png)


Docker是一个C/S模式的架构，后端是一个松耦合架构，众多模块各司其职。Docker运行的基本流程为:
1. 用户是使用Docker Client 与 Docker Daemon建立通信，并发送请求给后者。
2. Docker Daemon作为Docker架构中的主体部分，首先提供 Docker Server的功能使其可以接受Docker Client的请求。
3. Docker Engine执行Docker内部的一系列工作，每一项工作都是以一个Job的形式的存在。
4. Job的运行过程中，当需要容器镜像时，则从 Docker Registry中下载镜像，并通过镜像管理驱动Graph driver将下载镜像以Graph的形式存储。5当需要为Docker创建网络环境时，通过网络管理驱动Network driver创建并配置Docker容器网络环境。
6. 当需要限制Docker容器运行资源或执行用户指令等操作时，则通过Exec driver来完成。
7. Libcontainer是一项独立的容器管理包，Network driver以及Exec driver都是通过Libcontainer来实现具体对容器进行的操作。

# Docker组成
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303271112013.png)
## 镜像(image)
是一种轻量级、可执行的独立软件包，它包含运行某个软件所需的所有内容，我们把应用程序和配置依赖打包好形成一个可交付的运行环境(包括代码、运行时需要的库、环境变量和配置文件等)，这个打包好的运行环境就是image镜像文件。
Docker 镜像（Image）就是一个只读的模板。镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器。
只有通过这个镜像文件才能生成Docker容器实例(类似Java中new出来一个对象)。
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202303271126724.png)

- 镜像操作
## 容器(container)
### 从面向对象角度
Docker 利用容器（Container）独立运行的一个或一组应用，应用程序或服务运行在容器里面，容器就类似于一个虚拟化的运行环境，容器是用镜像创建的运行实例。就像是Java中的类和实例对象一样，镜像是静态的定义，容器是镜像运行时的实体。容器为镜像提供了一个标准的和隔离的运行环境，它可以被启动、开始、停止、删除。每个容器都是相互隔离的、保证安全的平台
### 从镜像容器角度
可以把容器看做是一个简易版的 Linux 环境（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序。
- 容器操作
## 仓库（Repository）
仓库（Repository）是集中存放镜像文件的场所。
Docker公司提供的官方registry被称为Docker Hub，存放各种镜像模板的地方。
 
仓库分为公开仓库（Public）和私有仓库（Private）两种形式。
最大的公开仓库是 Docker Hub(https://hub.docker.com/)，
存放了数量庞大的镜像供用户下载。国内的公开仓库包括阿里云 、网易云等
- 仓库操作




## 其他
- 杂项命令
- 

> docker官网：http://www.docker.com
> docker 手册：https://docs.docker.com/desktop
> Docker Hub官网: https://hub.docker.com/