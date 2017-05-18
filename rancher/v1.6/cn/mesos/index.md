---
title: Mesos in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## Mesos
---

要在Rancher中部署Mesos，您首先需要创建一个具有容器编排设置为**Mesos**的[环境模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/mesos/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#what-is-an-cnvironmcnt-template)的新[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/mesos/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)。****

### 创建Mesos环境

在环境下拉菜单中，单击**管理环境**。要创建新环境，请单击**添加环境**，提供**名称**，**说明**（可选），并选择具有Mesos作为业务流程的环境模板。如果[访问控制](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/mesos/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)开启，您可以[添加成员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/mesos/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#editing-members)并选择其[成员角色](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/mesos/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#membership-roles)。任何添加到会员列表的人都可以访问您的环境。

创建Mesos环境之后，您可以通过在左上角的环境下拉列表中选择环境名称，或者在特定环境的下拉列表中选择“ **切换到此环境”**来导航**到环境**。

> **注意：**由于Rancher添加了对多个容器编排框架的支持，Rancher目前不支持在已经具有服务运行的环境之间进行切换的能力。

### 启动Mesos

在创建了Mesos环境之后，除非您至少添加一个主机到您的环境，否则[基础架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/mesos/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)将不会启动。该**Mesos**服务将需要至少吃要添加3台主机。[添加主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/mesos/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)的过程与所有容器编排类型的步骤相同。一旦添加了第一个主机，Rancher将自动开始部署基础设施服务，包括Mesos服务（即mesos-master，mesos-slave和zookeeper）。您可以通过访问**Mesos**选项卡来查看部署进度。

> **注意：**只有牧场主或环境所有者的管理员才能查看基础[架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/mesos/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)。

### 使用Mesos

安装完成后，您可以通过以下方式开始创建或管理您自己的Mesos应用程序：

#### Mesos UI

您可以通过点击**Mesos UI**来管理Mesos 。一个新的网页将打开一个不同的UI来管理Mesos。在此UI中创建的任何框架也将反映在Rancher中。

#### 牧场主目录

Rancher支持托管Mesos框架目录的功能。要使用框架，请单击“ **Mesos”** - > **“启动框架** ”按钮或单击“ **目录** ”选项卡。选择要启动的框架，然后单击“ **查看详细信息”**。查看并编辑堆栈名称，堆栈描述和配置选项，然后单击**启动**。

如果要将自己的模板添加到Mesos，可以将它们添加到您自己的私有[Rancher目录中](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/mesos/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)，并将模板放在一个`mesos-templates`文件夹中。