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

