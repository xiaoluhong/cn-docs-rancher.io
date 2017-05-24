---
title: Overview of Rancher
layout: rancher-default-v1.6
version: v1.6
lang: en
redirect_from:
  - /rancher/latest/en/
  - /
  - /rancher/
  - /rancher/latest/
  - /rancher/latest/en/
---

## Rancher概述

------

牧场主是一个开放源码的软件平台，使企业能够在生产中运行容器。使用Rancher，组织不再需要使用不同的开源技术从头开始构建容器服务平台。Rancher提供管理生产中的容器所需的整个软件堆栈。

牧场主软件由四个主要组成部分组成：

### 基础设施协调

Rancher以Linux [主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)的形式从任何公共或私有云接收原始计算资源。每个Linux主机都可以是虚拟机或物理机。Rancher不会期望每个主机的CPU，内存，本地磁盘存储和网络连接都有更多的空间。从Rancher的角度来看，来自云提供商的VM实例和在colo工厂托管的裸机服务器是不可区分的。

Rancher实现了专门为集装箱应用供电的便携式基础[架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)层。牧场主基础架构服务包括[网络](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)，[存储](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/storage-service)，[负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/load-balancer)，[DNS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/dns-service)和安全性。Rancher基础架构服务通常作为容器部署，因此，相同的Rancher基础架构服务可以在任何来自任何云端的任何Linux主机上运行。

### 集装箱协调和调度

许多用户选择使用容器编排和调度框架运行容器化应用程序。Rancher包括今天发布的所有受欢迎的容器编排和调度框架，包括[Docker Swarm](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/swarm)，[Kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes)和[Mesos](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/mesos)。同一用户可以创建多个Swarm或Kubernetes群集。然后，他们可以使用本机Swarm或Kubernetes工具来管理其应用程序。

除了Swarm，Kubernetes和Mesos，Rancher还支持自己的容器编排和调度框架，称为“牛”。牛最初设计为Docker Swarm的扩展。随着Docker Swarm的不断发展，牛和Swarm开始分歧。因此，牧场主将支持牛和群，作为未来的单独框架。由Rancher自己广泛使用牛来协调基础设施服务，以及建立，管理和升级Swarm，Kubernetes和Mesos群集。

### 应用目录

Rancher用户可以从应用程序[目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)中部署整个多容器集群应用程序，只需单击一个按钮即可。用户可以管理部署的应用程序，并在新版本的应用程序可用时执行全自动升级。Rancher维护着由Rancher社区贡献的流行应用程序的公共目录。牧场主用户可以[创建自己的私人目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog/private-catalog)。

### 企业级控制

Rancher支持灵活的用户验证插件，并与Active Directory，LDAP和GitHub 进行预先建立的用户身份[验证集成](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)。牧场主支持基于角色的访问控制（RBAC）在水平[环境中](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)，允许用户和组共享或拒绝访问，例如，开发和生产环境。

下图显示了Rancher的主要组件和功能。

<img src="{{site.baseurl}}/img/rancher/rancher_overview_2.png" width="800" alt="Rancher Overview">
