---
title: Adding Other Hosts
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 从其他云提供商添加主机

------

Rancher支持使用其他云提供商进行配置`docker-machine`。其他云提供商有一个通用的UI，它提供了所有的选项`docker-machine`，我们只需要必要的参数。

请查看您选择的驱动程序的Docker Machine默认值，以确认您可以使用默认值。

### 添加其他驱动程序

如果要将其他驱动程序添加到Rancher实例中，则[管理员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/other/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/accounts/#admin)需要将其添加到我们的[机器驱动程序](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/other/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/machine-drivers)页面中。

### 启动其他主机

在“ **添加主机”**页面中选择“ **其他”**图标。****

1. 使用滑块选择要启动的主机数量。
2. 提供一个**名称**，如果需要，**说明**主机。
3. 选择您要使用的**驱动程序**类型。
4. 根据您的**驱动程序**，**驱动程序选项**部分将根据可用的**选项**进行填充`docker-machine`。
5. （可选）向主机添加**标签**，以帮助组织主机并[安排服务/负载均衡器，](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/other/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)或[使用主机IP以外的IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/other/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)对[外部DNS记录进行编程](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/other/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)。
6. （可选）`docker-machine create`使用[Docker引擎选项](https://docs.docker.com/machine/refercnce/create/#specifying-configuration-options-for-the-created-docker-cngine)自定义命令。
7. 完成后，单击**创建**。

一旦你点击创建，牧场主会在您选择的启动虚拟机**驱动程序**使用`docker-machine`并启动*牧场主代理*在VM容器。在几分钟之内，主机将被激活并可用于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/other/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)。