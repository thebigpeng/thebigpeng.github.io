---
title: Docker入门
date: 2022-05-18 00:56:52
tags:
  - Docker
categories: 项目管理
---

## Docker入门

### 1.Docker简介

docker是一种虚拟化技术，也叫容器技术，与虚拟机不同的是docker是在操作系统级别虚拟出多个运行环境，不同环境复用同一个操作系统，环境之间相互隔离，避免了系统资源的浪费，而虚拟机是在物理层面上分离出来的一个操作系统，不同虚拟机的操作系统可以不一样，虚拟机的资源开销更大。
![alt](%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%92%8C%E5%AE%B9%E5%99%A8%E7%9A%84%E5%8C%BA%E5%88%AB.png)

### 2.Docker环境搭建

首先得明确，Docker并不是容器，而是一个管理容器的引擎，要使用Docker，得安装其依赖环境。
Docker在Linux系统下性能最好，安装流程使用腾讯云服务器介绍，操作系统为CentOS 7。

#### 2.1 检查系统是否已经安装了Docker

```
yum list installed | grep docker
```

如果系统提示已经安装，可以使用使用如下命令卸载，注意卸载后所有与docker相关的东西。

```markdown
yum remove docker -y
```

#### 2.2 安装Docker

执行如下指令安装docker。

```python
yum install docker -y
```

![alt](docker安装成功.png)

安装完毕之后可以使用如下指令查看是否安装成功

```python
#查看docker版本
docker -v
```

#### 2.3 启动Docker服务

```python
#查看docker进程
ps -ef	 |grep docker
#启动
systemctl start docker
service docker start
#停止
systemctl stop docker
service docker stop
#重启
systemctl restart docker 
service docker restart
#查看服务状态
systemctl status docker
```

![alt](docker服务启动.png)

#### 2.4 查看docker信息

```python
#查看docker的系统信息
docker info 
#查看所有的帮助信息
docker
#查看某个commod指令的帮助信息
docker commod -help
```

### 3.  docker使用初体验

Docker的底层运行原理：

**Docker服务启动--->下载镜像--->启动镜像到一个容器--->在容器中运行我们的程序**

![alt](docker运行流程.png)

#### 3.1 Docker服务启动

#### 3.2 下载并运行一个镜像

Docker运行一个容器前需要在服务器本地有对应的镜像，如果本地不存在，将从镜像仓库去下载对应的镜像文件（默认仓库地址是[https://hub.docker.com](https://hub.docker.com)）。

从docker hub官网搜索要使用的镜像，也可以使用命令搜索要使用的镜像，如`docker search tomcat`，然后执行下载命令

```python
# 命令行搜索相关镜像
docker search tomcat
# 运行镜像 运行一个镜像就得到一个容器
## 前台运行
docker run tomcat 
## 后台运行
docker run -d tomcat
# 显示本地已有的镜像
## TAG：版本标签， IMAGE ID：镜像的唯一ID CREATED：镜像的创建时间（非本地时间）
docker images
```

![alt](下载tomcat镜像.png)

#### 3.3 docker的网络访问机制

我们需要对容器中运行的程序指定映射的端口供外界访问，Docker容器默认采用桥接模式与宿主机通信，需要将宿主机的ip端口映射到容器的ip端口上。 

```python 
# 查看系统中的容器运行状态
docker ps 
# 停止指定容器ID的运行
docker stop CONTAINER_ID
# 指定容器程序运行时要映射的端口  linux port:CONTAINER port
docker run -d -p 8080:8080 tomcat
```

![alt](容器端口映射.png)

#### 3.4 进入Docker容器

```python 
# 进入容器 i表示以标准输入流打开 t表示分配一个虚拟控制台
docker exec -it CONTAINER_ID bash
# 退出容器
exit
```

