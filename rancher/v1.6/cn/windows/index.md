---
title: Windows in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## Windows（实验）

------

要在Rancher中部署Windows，您首先需要创建一个具有容器编排设置为**Windows**的[环境模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/windows/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#what-is-an-cnvironmcnt-template)的新[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/windows/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)。****

目前，Rancher只支持在特定主机上创建容器。目前不支持在UI中显示的牛的其他功能（例如，服务发现，健康检查，元数据，DNS，负载平衡器）。

> **注意：**有一个默认的Windows环境模板可用。如果您尝试使用Windows创建自己的环境模板，则需要禁用所有其他基础设施服务，因为它们目前与Windows不兼容。

### 创建Windows环境

在环境下拉菜单中，单击**管理环境**。要创建新环境，请单击**添加环境**，提供**名称**，**说明**（可选），然后选择具有Windows作为业务流程的环境模板。如果[访问控制](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/windows/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)开启，您可以[添加成员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/windows/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#editing-members)并选择其[成员角色](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/windows/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#membership-roles)。任何添加到会员列表的人都可以访问您的环境。

在创建Windows环境之后，您可以通过在左上角的环境下拉列表中选择环境名称，或者在特定环境的下拉列表中选择“ **切换到此环境”**来导航**到环境**。

> **注意：**由于Rancher添加了对多个容器编排框架的支持，Rancher目前不支持在已经具有服务运行的环境之间进行切换的能力。

### 添加Windows主机

为了将主机添加到Windows中，您需要准备运行[Windows Server 2016并安装Docker的](https://msdn.microsoft.com/cn-us/virtualization/windowscontainers/about/index)主机。

在“ **基础设施** ”选项卡中，您将获得一个自定义命令来启动“牧场主代理”服务。按照说明在Windows中启动Rancher代理服务。

在主机上，代理二进制文件将被下载到一个被调用的文件夹中`C:/Program Files/rancher`，代理日志将被发现`C:/ProgramData/rancher/agcnt.log`。

### 删除Windows主机

作为向Rancher添加主机的一部分，Rancher代理在主机上安装并注册为服务。为了重新使用主机，您必须删除现有服务。在PowerShell中，运行以下命令。服务被删除后，您可以在Windows环境中重新使用主机。

```
＆ ' C：\ Program Files文件\牧场主\ agcnt.exe ' -unregister服务
```

### Windows中的网络

默认情况下，我们支持NAT和透明[网络](https://docs.microsoft.com/cn-us/virtualization/windowscontainers/manage-containers/container-networking)。

目前，默认的**Windows**环境模板支持一个名为的透明网络`transparcnt`，它由运行创建`docker network create -d transparcnt transparcnt`。

如果要创建具有不同名称的透明网络，则需要使用**Windows**创建一个新的环境模板作为容器编排。选择**Windows后**，可以单击“ **编辑配置”**，更改透明网络的名称。默认名称为`transparcnt`。创建更新的环境模板后，您可以创建一个新环境，以支持新命名的透明网络。UI将继续`transparcnt`使用默认名称，因此您需要更新命令`docker network create -d transparcnt <NEW_NAME_IN_TEMPLATE`。