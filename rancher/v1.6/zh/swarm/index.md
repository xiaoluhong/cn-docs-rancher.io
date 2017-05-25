---
title: Swarm in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## Swarm
---

要在Rancher中部署Swarm，您首先需要创建一个具有容器编排设置为**Swarm**的[环境模板]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environments/#what-is-an-environment-template)的新[环境]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environments)。****

### 要求

- Docker 1.13或更高版本
- 端口`2377`和`2378`必须是主机之间开放

### 创建一个Swarm环境

在环境下拉菜单中，单击**管理环境**。要创建新环境，请单击**添加环境**，提供**名称**，**描述**（可选），并选择一个具有Swarm作为业务流程的环境模板。如果[访问控制]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/configuration/access-control)开启，您可以[添加成员]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environments/#editing-members)并选择其[成员角色]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environments/#membership-roles)。任何添加到会员列表的人都可以访问您的环境。

创建Swarm环境后，您可以通过在左上角的环境下拉菜单中选择环境名称，或者在特定环境的下拉列表中选择“ **切换到此环境”**来导航**到环境**。

> **注意：**由于Rancher添加了对多个容器编排框架的支持，Rancher目前不支持在已经具有服务运行的环境之间进行切换的能力。

### 开始Swarm

在创建了一个Swarm环境之后，基础[架构服务]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services)将不会启动，除非您至少添加一个主机到您的环境中。该**Swarm**服务将需要至少3台主机。[添加主机]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts)的过程与所有容器编排类型的步骤相同。一旦添加了第一个主机，Rancher将自动开始部署基础架构服务，包括Swarm组件（即Swarm组和Swarm组代理）。您可以通过访问**Swarm**选项卡来查看部署进度。

> **注意：**请勿尝试`docker swarm`手动对docker守护程序运行命令。这可能与Rancher对Swarm集群的管理有冲突。

> **注意：**只有rancher或环境所有者的管理员才能查看基础[架构服务]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services)。

#### CLI通过Shell

Rancher提供了一个方便的shell访问实例，可以用于执行`docker`或`docker-compose`命令。