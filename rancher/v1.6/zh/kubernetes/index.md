---
title: Kubernetes in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## Kubernetes
---

用Rancher部署Kubernetes，您首先需要创建一个新的[环境，]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environmcnts)有一个[环境的模板]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environmcnts/#what-is-an-cnvironmcnt-template)与容器编排设置为**Kubernetes**。

### 配置Kubernetes

可以在创建或编辑环境模板时配置Kubernetes。如果您从**cattle**环境开始，您可以从**目录** - > **库**中配置和启动**Kubernetes**。如果选择目录路由，则可以跳过步骤1。********

1. 在[环境模板]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environmcnts/#what-is-an-cnvironmcnt-template)中**编辑****Kubernetes ****配置**，单击[模板]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environmcnts/#what-is-an-cnvironmcnt-template)的“ **编排”**部分中的**“编辑配置** ”。您可以在创建新的环境模板或编辑现有环境模板时编辑配置。
2. 确认Kubernetes的模板版本是您要使用的。
3. 选择[平面隔离]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//kubernetes/resilicncy-planes)，[云提供商]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//kubernetes/providers)，[备份]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/backups)，[附加组件]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/addons)的配置选项。
4. 点击**配置**保存点击**启动**的环境模板的配置选项，从目录启动Kubernetes。

> **注意：**我们建议您使用正确的配置启动您的Kubernetes环境，如果要更改现有Kubernetes设置的配置，可以单击“ **最新日期** ”按钮将Kubernetes设置升级到新的。

### 创建Kubernetes环境

在环境下拉菜单中，单击**管理环境**。要创建一个新的环境，点击**添加环境**，提供一个**名称**，**描述**（可选），并选择具有环境模板**Kubernetes**的配置为你想要的编排。如果[访问控制]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//configuration/access-control)开启，您可以[添加成员]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environmcnts/#editing-members)并选择其[成员角色]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environmcnts/#membership-roles)。任何添加到会员列表的人都可以访问您的环境。

创建了Kubernetes环境后，您可以通过在左上角的环境下拉菜单中选择环境名称，或者在特定环境的下拉列表中选择“ **切换到此环境”**来导航**到环境**。

> **注意：**由于Rancher添加了对多个容器编排框架的支持，Rancher目前不支持在已经具有服务运行的环境之间进行切换的能力。

### 开始Kubernetes

在创建了Kubernetes环境之后，基础[架构服务]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//rancher-services)将不会启动，除非您至少添加一个主机到您的环境中。[添加主机]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//hosts)的过程与所有容器编排类型的步骤相同。一旦添加了第一个主机，Rancher将自动启动基础架构服务的部署，包括Kubernetes服务（即主服务器，kubelet等，代理服务器等）。您可以通过访问**Kubernetes** - > **Infrastructure Stacks**选项卡来查看部署进度。

#### Kubernetes的主机要求

- 对于[重叠平面]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//kubernetes/resilicncy-planes/#overlapping-planes)设置：至少1个CPU，2GB RAM。资源需求因工作负载而异。
- 对于[分离的平面]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//kubernetes/resilicncy-planes/#separated-planes)设置：此部署类型至少需要五个主机。
- 数据平面：添加3个或更多主机1 CPU，> = 1.5GB RAM，> = 20GB磁盘。当添加主机，[标注这些主机]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts/#host-labels)使用`etcd=true`。
- 编排平面：添加1个或多个主机，其中> = 1 CPU和> = 2GB RAM。当添加主机，[标注这些主机]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts/#host-labels)使用`orchestration=true`。你可以摆脱1个主机，但你牺牲高可用性。在主机发生故障的情况下，一些K8s功能，如API，在发生故障时重新安排的端口等，直到新的主机被配置才会发生。
- 计算平面：添加1个或多个主机。当添加主机，[标注这些主机]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts/#host-labels)使用`compute=true`。

> **注意：**只有牧场主或环境所有者的管理员才能查看基础[架构服务]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//rancher-services)。

当向Kubernetes添加主机时，使用主机名作为Kubernetes节点的唯一标识符`kubectl get nodes`。

### 使用Kubernetes

安装完成后，您可以通过以下方式开始创建或管理自己的Kubernetes应用程序：

#### 牧场主目录

Rancher支持托管Kubernetes模板目录的功能。要使用模板，请单击“ **目录** ”选项卡。选择要启动的模板，然后单击**查看详细信息**。查看并编辑堆栈名称，堆栈描述和配置选项，然后单击**启动**。

如果要将自己的模板添加到Kubernetes，可以将它们添加到[Rancher目录中]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//catalog)，并将模板放在一个`kubernetes-templates`文件夹中。

#### kubectl

要配置您自己的kubectl与新创建的Kubernetes群集通话，请访问**Kubernetes** - > **kubectl**。单击**生成配置**生成必要的`kube/config_file`，您可以下载并添加到本地目录。

在配置文件中，它为您的本地机器提供必要的信息，以便您启动的任何`kubectl`内容将显示在Rancher中。

#### kubectl通过壳牌

Rancher提供了一个方便的shell访问管理的kubectl实例，可用于管理Kubernetes集群和应用程序。

### 添加私人注册表到Kubernetes

私人注册表可以通过在Kubernetes环境中添加[私有仓库]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environmcnts/registries)与Kubernetes服务一起使用。