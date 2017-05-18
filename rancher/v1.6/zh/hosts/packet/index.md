---
title: Adding Packet Hosts
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 添加分组主机

------

Rancher支持配置[数据包](https://www.packet.net/)主机`docker machine`。

### 查找分组凭证

为了启动数据包主机，您需要一个**API密钥**。登录到您的数据包帐户。

1. 导航到[api-key页面](https://app.packet.net/portal#/api-keys)。如果您尚未创建一个api密钥，则需要添加一个密钥。
2. 在新的api密钥屏幕中，您将介绍一个描述（例如牧场主），然后单击**生成**。
3. 新创建的**令牌**将可见，供您在Rancher中复制和使用。

### 启动数据包主机

现在我们已经发现了我们的**令牌**，我们已经准备好推出我们的Packet主机了。在基础**结构 - >主机**选项卡下，单击**添加主机**。选择**分组**图标。

1. 使用滑块选择要启动的主机数量。
2. 提供一个**名称**，如果需要，**说明**主机。
3. 提供您从Packet帐户创建的**API密钥**。
4. 提供**项目**要启动主机。此项目位于您的数据包帐户中。
5. 选择**图像**。`docker machine`Rancher还支持Packet的任何支持。
6. 选择图像的**大小**。
7. 选择**地区**要启动英寸
8. （可选）向主机添加**标签**，以帮助组织主机并[安排服务/负载均衡器，](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/packet/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)或[使用主机IP以外的IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/packet/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)对[外部DNS记录进行编程](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/packet/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)。
9. （可选）`docker-machine create`使用[Docker引擎选项](https://docs.docker.com/machine/reference/create/#specifying-configuration-options-for-the-created-docker-engine)自定义命令。
10. 完成后，单击**创建**。

一旦您点击了创建，Rancher将创建数据包并启动“大屠杀者*代理”*容器。在一两分钟内，主机将处于活动状态并可用于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/packet/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)。