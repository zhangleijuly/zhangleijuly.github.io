---
title: "MongoDB C语言编程初探"
date: 2022-01-07
slug: mongodb-c-start
image: "img/MongoDB.jpeg"
math: false
draft: true
categories:
    - Code
tags:
    - MongoDB
    - C
---

## 安装MongoDB

### 0.安装环境

操作系统：CentOS 7

MongoDB版本：4.4.10

### 1.下载MongoDB

打开[下载链接](https://www.mongodb.com/try/download/community)，选择MongoDB版本，安装平台选择RedHat/CentOS 7，安装包选择tgz。

### 2.安装并配置MongoDB

拷贝下载的安装包到CentOS 7，执行以下指令：

```bash
#解压安装包
tar -zxvf mongodb-linux-x86_64-rhel70-4.4.10.tgz
#安装MongoDB
mv mongodb-linux-x86_64-rhel70-4.4.10 /usr/local/mongodb
cd /usr/local/mongodb
#配置MongoDB
vi mongodb.conf
#创建存储数据和日志的文件夹
mkdir -p /usr/local/mongodb/data/db
mkdir -p /usr/local/mongodb/logs
touch /usr/local/mongodb/logs/mongodb.log
```

配置如下内容：

```
#数据存储路径
dbpath = /usr/local/mongodb/data/db
#日志存储路径
logpath = /usr/local/mongodb/logs/mongodb.log
#默认端口为27017
port = 27017
#以守护程序的方式启用，即在后台运行
fork = true
#允许远程连接，设置为127.0.0.1只允许本地连接
bind_ip=0.0.0.0
#是否需要认证，如果启用，则需要创建mongodb账号密码，使用账号密码才可以远程访问
#auth = true
```

试验环境为了方便未配置认证信息。

### 3.配置环境变量

```bash
#编辑环境变量
vi /etc/profile
```

添加如下内容：

```
#添加MongoDB目录到环境变量
export PATH=$PATH:/usr/local/mongodb/bin
```

让环境变量立刻生效：

```bash
source /etc/profile
```

### 4.启动MongoDB服务

```bash
mongod -f /usr/local/mongodb/mongodb.conf
```

成功启动后会显示如下内容：

```bash
[admin@localhost ~]# mongod -f /usr/local/mongodb/mongodb.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 9607
child process started successfully, parent exiting
```

### 5.关闭CentOS防火墙

```bash
#查看防火墙状态
firewall-cmd --state
#关闭防火墙
systemctl stop firewalld.service
#禁用防火墙开机启动
systemctl disable firewalld.service
```

关闭防火墙后其他机器才能访问数据库。

## 编译MongoDB C Driver

### 0.安装环境

操作系统：Windows 10

MongoDB C Driver版本：1.20.0

CMake版本：3.22.1

Visual Studio版本：2022

### 1.下载MongoDB C Driver

在[官方网站](http://mongoc.org/)或者[GitHub](https://github.com/mongodb/mongo-c-driver)下载最新版本的压缩包。

CMake和Visual Studio请自行下载安装。

### 2.编译MongoDB C Driver

解压MongoDB C Driver压缩包，打开Windows命令行（cmd或PowerShell），进入mongo-c-driver-1.20.0文件夹，执行以下命令：

```powershell
#创建工程文件夹
mkdir cmake-build
cd cmake-build
#编译MongoDB C Driver
cmake -G “Visual Studio 17 2022” “-DCMAKE_INSTALL_PREFIX=D:\mongo-c-driver” “-DCMAKE_PREFIX_PATH=D:\mongo-c-driver” -DCMAKE_BUILD_TYPE=Release ..
#安装MongoDB C Driver
cmake --build . --config Release --target install
```

CMake指令参数`-G`用于为当前项目指定生成器，对于Visual Studio而言包括以下选项，请根据需要输入相应参数。

```
Visual Studio 17 2022        = Generates Visual Studio 2022 project files.
                               Use -A option to specify architecture.
Visual Studio 16 2019        = Generates Visual Studio 2019 project files.
                               Use -A option to specify architecture.
Visual Studio 15 2017 [arch] = Generates Visual Studio 2017 project files.
                               Optional [arch] can be "Win64" or "ARM".
Visual Studio 14 2015 [arch] = Generates Visual Studio 2015 project files.
                               Optional [arch] can be "Win64" or "ARM".
Visual Studio 12 2013 [arch] = Generates Visual Studio 2013 project files.
                               Optional [arch] can be "Win64" or "ARM".
Visual Studio 11 2012 [arch] = Generates Visual Studio 2012 project files.
                               Optional [arch] can be "Win64" or "ARM".
Visual Studio 10 2010 [arch] = Deprecated.  Generates Visual Studio 2010
                               project files.  Optional [arch] can be
                               "Win64" or "IA64".
Visual Studio 9 2008 [arch]  = Generates Visual Studio 2008 project files.
                               Optional [arch] can be "Win64" or "IA64".
```

CMake指令参数`-DCMAKE_INSTALL_PREFIX`用于指定MongoDB C Driver的安装位置，Windows操作系统下的默认值为`C:/Program Files/${PROJECT_NAME}`。

CMake指令参数`-DCMAKE_PREFIX_PATH`用于配置CMake环境变量。

CMake指令参数`-DCMAKE_BUILD_TYPE`用于指定构建类型，典型的值包括Debug、Release、RelWithDebInfo和MinSizeRel。

最后一条指令生成当前文件夹工程的Release版本，并安装到指定的目录（这里是D:\mongo-c-driver）中。
