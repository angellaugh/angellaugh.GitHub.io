---
layout: posts
title: Docker_study some points
date: 2023-04-29 20:34
updated: 2023-04-29 20:34
tags: docker
categories: Teckknowledge
---

## VM
### 完整的OS， hypervisor
利用hypervisor 做物理硬件和OS之间的中间虚拟化层，比如微软的Hyper-V。
占用宿主机的 内存，硬盘资源，像是装了个完整的OS。

### 扩展管理工具： kvm penstack

### 优点
a. 环境隔离；
b. 应用迁移 --> clone;  LNMP: 代表了一种流行的网站服务器软件组合：Linux、Nginx、MySQL 和 PHP，  应用开发env
c. 省钱

### 缺点
不管是否使用，都占用宿主机固定的resource，利用率低，无法应对瞬时并发压力。



## Docker （DotCloud, 基于linux的 容器技术LXC）
### 基于namespace的资源隔离
网络资源隔离（ip, port，网卡，接口）
进程资源隔离 （pid范围）
文件系统隔离（绝对路径）
### LXC
libcontainer, 基于LXC 用golang开发

### 优缺点
Docker 进程是对某个程序的封装，提供了独立运行的隔离环境。


## docker和vm的共同点，以及优缺点

### 共同点：

    两者都提供了一种在同一台物理机上运行多个独立环境的方式。
    都可以在宿主机上隔离应用程序和操作系统，提高软件的兼容性和安全性。
    它们都支持快速部署和管理，便于应用程序的开发、测试和发布。

### 不同点：

    实现方式：VM 在宿主机上运行完整的操作系统副本，包括内核和系统资源。而 Docker 使用容器技术，在宿主机操作系统上共享内核，每个容器只包含应用程序及其依赖项。
    资源占用：VM 通常需要更多的系统资源，因为它们运行完整的操作系统。而 Docker 容器更轻量级，启动速度更快，资源占用更低。
    系统支持：VM 可以运行不同类型的操作系统，如 Windows、Linux 和 macOS。Docker 最初仅支持 Linux，但现在也支持 Windows 和 macOS，尽管在某些方面可能存在限制。

### 流行度：
    Docker 和容器技术 在云计算、微服务和 DevOps 环境中大量使用。容器技术可以更好地支持快速迭代开发、轻量级部署和资源优化。
    然而，在某些需要更高安全性和隔离性的场景中，VM 仍然具有一定的优势。
#### DevOps
    是一种软件开发和交付方法，它强调开发（Dev）和运维（Ops）团队之间的紧密协作、快速迭代和持续改进。
在实践中，DevOps 通常涉及到以下技术和工具：

    版本控制：如 Git，用于管理代码和协作开发。
    持续集成/持续部署：如 Jenkins、GitLab CI/CD 和 Travis CI，用于自动化构建、测试和部署代码。
    配置管理：如 Ansible、Puppet 和 Chef，用于自动化服务器和应用程序配置。
    容器化和虚拟化：如 Docker 和 Kubernetes，用于轻量级部署和资源管理。
    监控和日志：如 Prometheus、Grafana 和 ELK Stack（Elasticsearch, Logstash, Kibana），用于收集、分析和可视化系统指标和日志。
    

## K8s

容器随时创建，随时删除，IP不固定，所以交互要走主机名， 每次容器启动，自动对应容器主机名和容器IP；
如何维护iptables？ 
如何保证高可用负载均衡的实施？ 本质还是操作镜像，维护容器
        for instance： nginx 的 upstream {container 1; container2; container3; container4}
        如何保证一直存活？ 如何保证统一升级，统一回滚？


### dockerfile
是个脚本，写部署逻辑（部署jdk， 安装python，sed修改配置，导入源码等一些列action）， 构建容器用的。

docker的生命周期 2  大要素： 1. 镜像构建； 2. 基于镜像，运行容器；


DevOps 部署流水线，环境一致性： 

    1. githhub 提供源码；
    2. docker hub / registry(私有) 存的是 dockerfile 构建的镜像；
    3. Jenkins ：
        1. 从github 下载源码；
        2. 从docker hub 下载镜像；

    4. 部署 test env;
    5. 部署 生产 env；

dockerfile 和 镜像  和 container的关系？


镜像不是虚拟机模板， 容器进行，容器实例




### iptables
防火墙作用， 数据包转发，sourceIP:port ---> Destination IP:port


### 容器化技术
容器， 镜像，仓库， 网络， 容器间的桥网络网段

容器编排

网络： 跨主机通信
存储： 跨主机共享数据


### pod
管理一组容器，是一个逻辑单元，比如一整套的服务需要三个容器，分别提供微服务：python, nginx, mysql.
这三个容器可以一起给一个pod 管理。

可能是要共享：
1. 存储（volume or local）;
2. 网络；
3. 互相依赖；



### docker-compose： 单机容器编排

在一台主机上，要run 4个容器，执行4次 `docker run -v -p 镜像 --restart 容器名 --link`
<mark>可以用yaml 文件来写： ---> 网络资源，存储资源，镜像名，镜像资源，容器名，容器运行参数，容器资源</mark>
1. 如何部署；
2. 用什么参数；
3. 什么卷；
4. 什么网络；


### k8s 的部分组件介绍
> 是基于yaml文件的 大而全的 容器管理平台

> master node 根据user写的yaml， 对容器的运行描述，创建具体容器，到node 工作节点上去干活；

网络关系
数据共享关系
配置文件加密
<mark>容器内的负载均衡反向代理</mark>

####  deployment
保证弹性扩容，应用发布，升级降级， 保证container数量， 确认要4个container，当其中2个挂掉，会自动再启动2台补全；


