# How_To_Start_Docker

###Docker 介绍
Docker 是一个开源项目，诞生于 2013 年初，最初是 dotCloud 公司内部的一个业余项目。它基于 Google 公司推出的 Go 语言实现。 项目后来加入了 Linux 基金会，遵从了 Apache 2.0 协议，项目代码在 GitHub 上进行维护。

Docker 自开源后受到广泛的关注和讨论，Redhat 已经在其 RHEL6.5 中集中支持 Docker；Google 也在其 PaaS 产品中广泛应用。

Docker 项目的目标是实现轻量级的操作系统虚拟化解决方案。 Docker 的基础是 Linux 容器（LXC）等技术。

什么情况用docker比较合适？
* 同台服务器上跑多个服务，需要隔离应用及资源使用
* 需要快速部署各种配置不同需求的服务器
* 尽可能发挥服务器的作用(合理利用资源）

###Docker 基本概念

####Docker 镜像
Docker 镜像就是一个只读的模板，镜像可以用来创建 Docker 容器。

Docker 提供了一个很简单的机制来创建镜像或者更新现有的镜像，用户甚至可以直接从其他人那里下载一个已经做好的镜像来直接使用。
####Docker 容器
容器是从镜像创建的运行实例。它可以被启动、开始、停止、删除。每个容器都是相互隔离的、保证安全的平台。
####Docker 仓库
仓库是集中存放镜像文件的场所。

####镜像
* 获取镜像
  
  可以使用 docker pull 命令来从仓库获取所需要的镜像。
* 创建
  
  使用 docker images 显示本地已有的镜像。
  
  #####直接修改已有镜像
  
  docker run -t -i 镜像名 /bin/bash
  
  #####利用DockerFile修改已有镜像
  
  Dockerfile 基本的语法是
  
   使用#来注释
   
   FROM 指令告诉 Docker 使用哪个镜像作为基础
   
   接着是维护者的信息
   
   RUN开头的指令会在创建中运行，比如安装一个软件包，在这里使用 apt-get 来安装了一些软件
* 保存及载入
   
   如果要导出镜像到本地文件，可以使用 docker save 命令。
   
   可以使用 docker load 从导出的本地文件中再导入到本地镜像库
* 删除

   如果要移除本地的镜像，可以使用 docker rmi 命令。注意 docker rm 命令是移除容器。

####容器

容器是 Docker 又一核心概念。

简单的说，容器是独立运行的一个或一组应用，以及它们的运行态环境。对应的，虚拟机可以理解为模拟运行的一整套操作系统（提供了运行态环境和其他系统环境）和跑在上面的应用。
* 启动和停止

当利用 docker run 来创建容器时，Docker 在后台运行的标准操作包括：

1.  检查本地是否存在指定的镜像，不存在就从公有仓库下载
2.  利用镜像创建并启动一个容器
3.  分配一个文件系统，并在只读的镜像层外面挂载一层可读写层
4.  从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
5.  从地址池配置一个 ip 地址给容器
6.  执行用户指定的应用程序
7.  执行完毕后容器被终止

如果需要让 Docker 容器在后台以守护态（Daemonized）形式运行。此时，可以通过添加 -d 参数来实现。

可以使用 docker stop 来终止一个运行中的容器。

* 导出和导入容器

如果要导出本地某个容器，可以使用 docker export 命令。

可以使用 docker import 从容器快照文件中再导入为镜像.

####仓库

可以分为公有仓库及私有仓库

共有仓库即公网提供的可供下载镜像的仓库。

私有仓库可以通过docker-registry, 这官方提供的工具，用于构建私有的镜像仓库。

###DockerFile
使用 Dockerfile 可以允许用户创建自定义的镜像。
* 基本结构
Dockerfile 由一行行命令语句组成，并且支持以 # 开头的注释行。

一般的，Dockerfile 分为四部分：基础镜像信息、维护者信息、镜像操作指令和容器启动时执行指令。

```Swift
# This dockerfile uses the ubuntu image
# VERSION 2 - EDITION 1
# Author: docker_user
# Command format: Instruction [arguments / command] ..

# Base image to use, this must be set as the first line
FROM ubuntu

# Maintainer: docker_user <docker_user at email.com> (@docker_user)
MAINTAINER docker_user docker_user@email.com

# Commands to update the image
RUN echo "deb http://archive.ubuntu.com/ubuntu/ raring main universe" >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf

# Commands when creating a new container
CMD /usr/sbin/nginx
```

* 基本指令

指令的一般格式为 INSTRUCTION arguments，指令包括 FROM、MAINTAINER、RUN 等。

#####FROM

格式为 FROM image 或FROM image:tag。

第一条指令必须为 FROM 指令。并且，如果在同一个Dockerfile中创建多个镜像时，可以使用多个 FROM 指令（每个镜像一次）。

#####MAINTAINER

格式为 MAINTAINER name，指定维护者信息。

#####RUN

格式为 RUN command 或 RUN ["executable", "param1", "param2"]。

前者将在 shell 终端中运行命令，即 /bin/sh -c；后者则使用 exec 执行。指定使用其它终端可以通过第二种方式实现，例如 RUN ["/bin/bash", "-c", "echo hello"]。

#####CMD

支持三种格式

CMD ["executable","param1","param2"] 使用 exec 执行，推荐方式；
CMD command param1 param2 在 /bin/sh 中执行，提供给需要交互的应用；
CMD ["param1","param2"] 提供给 ENTRYPOINT 的默认参数；
指定启动容器时执行的命令，每个 Dockerfile 只能有一条 CMD 命令。如果指定了多条命令，只有最后一条会被执行。

如果用户启动容器时候指定了运行的命令，则会覆盖掉 CMD 指定的命令。

#####ENV

格式为 ENV key value。 指定一个环境变量，会被后续 RUN 指令使用，并在容器运行时保持。

#####COPY

格式为 COPY src dest。

复制本地主机的 src（为 Dockerfile 所在目录的相对路径）到容器中的 dest。
