---
title: Overview of Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/latest/zh/
  - /
  - /rancher/zh/
  - /rancher/latest/
  - /rancher/latest/zh/
---

## Rancher概述

------

ranhcer是一个开放源码的软件平台，使企业能够在生产中运行容器。使用Rancher，组织不再需要使用不同的开源技术从头开始构建容器服务平台。Rancher提供管理生产中的容器所需的整个软件堆栈。

rancher软件由四个主要组成部分组成：

### 基础设施协调

Rancher以Linux [主机]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts)的形式从任何公共或私有云接收原始计算资源。每个Linux主机都可以是虚拟机或物理机。Rancher不会期望每个主机的CPU，内存，本地磁盘存储和网络连接都有更多的空间。从Rancher的角度来看，来自云提供商的VM实例和托管的裸机服务器是没有区别的。

Rancher实现了专门为集装箱应用服务的便携式基础[架构服务]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services)层。rancher基础架构服务包括[网络]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//rancher-services/networking)，[存储]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services/storage-service)，[负载平衡器]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/load-balancer)，[DNS]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/dns-service)和安全性。Rancher基础架构服务通常作为容器部署，因此，相同的Rancher基础架构服务可以在来自云端的任何Linux主机上运行。

### 集装箱协调和调度

许多用户选择使用容器编排和调度框架运行容器化应用程序。Rancher包含了所有受欢迎的容器编排和调度框架，包括[Docker Swarm]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/swarm)，[Kubernetes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes)和[Mesos]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/mesos)。同一用户可以创建多个Swarm或Kubernetes群集。然后，他们可以使用本机Swarm或Kubernetes工具来管理其应用程序。

除了Swarm，Kubernetes和Mesos，Rancher还支持自己的容器编排和调度框架，称为“Cattle”。Cattle最初设计为Docker Swarm的扩展。随着Docker Swarm的不断发展，Cattle和Swarm开始分歧。因此，rancher将支持Cattle和Swarm ，作为未来的单独框架。由Rancher自己广泛使用Cattle来协调基础设施服务，以及建立，管理和升级Swarm，Kubernetes和Mesos群集。

### 应用目录

Rancher用户可以从应用程序[目录]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/catalog)中部署整个多容器集群应用程序，只需单击一个按钮即可。用户可以管理部署的应用程序，并在新版本的应用程序可用时执行全自动升级。Rancher维护着由Rancher社区贡献的流行应用程序的公共目录。rancher用户可以[创建私有目录]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/catalog/private-catalog/)。

### 企业级控制

Rancher支持灵活的用户验证插件，并与Active Directory，LDAP和GitHub 进行预先建立的用户身份[验证集成]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/configuration/access-control)。rancher支持基于角色的访问控制（RBAC）在水平[环境中]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environments)，允许用户和组共享或拒绝访问，例如，开发和生产环境。

下图显示了Rancher的主要组件和功能。

<img src="{{site.baseurl}}/img/rancher/rancher_overview_2.png" width="800" alt="Rancher Overview">
