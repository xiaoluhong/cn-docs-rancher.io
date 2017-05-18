---
title: Adding DigitalOcean Hosts
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 添加DigitalOcean主机

------

Rancher支持使用[DigitalOcean](https://www.digitalocean.com/)主机`docker machine`。

### 寻找DigitalOcean证书

为了推出DigitalOcean主机，您需要DigitalOcean提供的**个人访问令牌**。登录到您的DigitalOcean帐户。

1. 转到[Apps和API页面](https://cloud.digitalocean.com/settings/applications)。
2. 在**个人访问令牌中**，单击**生成新令牌**按钮。命名您的令牌（例如牧场主），然后单击**生成令牌**。
3. 从用户界面复制您的**访问令牌**，并将其保存在安全的地方。这是您唯一可以看到访问令牌的时间。下次您进入页面时，令牌将不再显示，您将无法检索。

### 推出DigitalOcean主机

现在我们已经保存了**访问令牌**，我们已经准备好启动我们的DigitalOcean主机了。在基础**结构 - >主机**选项卡下，单击**添加主机**。选择**DigitalOcean**图标。

1. 使用滑块选择要启动的主机数量。
2. 提供一个**名称**，如果需要，**说明**主机。
3. 提供您为DigitalOcean帐户创建的**访问令牌**。
4. 选择要启动的**图像**。无论`docker machine`对DigitalOcean的支持也是由Rancher支持的。
5. 选择图像的**大小**。
6. 选择**地区**要在推出。我们已经提供了可以使用元数据推出了可用的区域。某些区域可能不包括在内，因为API不支持它。
7. （可选）如果要启用任何高级选项（即备份，IPv6，专用网络），请选择要包括的选项。
8. （可选）向主机添加**标签**，以帮助组织主机并[安排服务/负载均衡器，](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/digitalocean/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)或[使用主机IP以外的IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/digitalocean/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)对[外部DNS记录进行编程](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/digitalocean/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)。
9. （可选）在**高级选项中**，`docker-machine create`使用[Docker引擎选项](https://docs.docker.com/machine/reference/create/#specifying-configuration-options-for-the-created-docker-engine)自定义命令。
10. 完成后，单击**创建**。

一旦您点击了创建，Rancher将创建DigitalOcean小滴，并在液滴中启动*牧场主代理*容器。在几分钟之内，主机将被激活并可用于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/digitalocean/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)。