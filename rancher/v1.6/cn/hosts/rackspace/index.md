---
title: Adding Rackspace Hosts
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 添加Rackspace主机

------

Rancher支持配置[Rackspace](http://www.rackspace.com/)主机`docker machine`。

### 查找机顶盒证书

为了启动Rackspace主机，您需要由Rackspace提供的**API密钥**。登录到您的Rackspace帐户。

1. 导航到帐户设置。
2. 在“登录详细信息”部分中，有一个**API密钥**。单击显示以显示API密钥。复制在Rancher中使用的密钥。

### 启动Rackspace主机

现在我们已经找到了我们的**API密钥**，我们已经准备好推出我们的Rackspace主机了。在基础**结构 - >主机**选项卡下，单击**添加主机**。选择**Rackspace**图标。

1. 使用滑块选择要启动的主机数量。
2. 提供一个**名称**，如果需要，**说明**主机。
3. 提供您登录到您的Rackspace帐户的**用户名**。
4. 提供我们发现与您的用户名相关联的**API密钥**。
5. 挑选**地区**要启动您的主机英寸
6. 挑选主机的**味道**。
7. （可选）向主机添加**标签**，以帮助组织主机并[安排服务/负载均衡器，](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/rackspace/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)或[使用主机IP以外的IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/rackspace/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)对[外部DNS记录进行编程](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/rackspace/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)。
8. （可选）`docker-machine create`使用[Docker引擎选项](https://docs.docker.com/machine/refercnce/create/#specifying-configuration-options-for-the-created-docker-cngine)自定义命令。
9. 完成后，单击**创建**。

一旦您点击了创建，Rancher将创建Rackspace服务器并启动服务器中的Rancher *-agcnt*容器。在几分钟之内，主机将被激活并可用于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/rackspace/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)。