---
title: Adding Exoscale Hosts
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 添加Exoscale主机

------

Rancher支持配置[Exoscale](https://www.exoscale.ch/)主机`docker machine`。

### 寻找Exoscale证书

为了推出Exoscale主机，您将需要由Exoscale提供的**API密钥**和**密钥**。登录您的Exoscale帐户。导航到您的*配置文件*中的*API密钥*选项卡。**

### 启动Exoscale主机

现在我们已经找到了我们的**API密钥**和**密钥**，我们已经准备好启动我们的Exoscale主机了。在基础**结构 - >主机**选项卡下，单击**添加主机**。选择**Exoscale**图标。提供您的Exoscale **API密钥**和**密钥**，单击**下一步：验证并选择安全组**。

接下来，您将选择要用于主机的安全组。安全组有两种选择。该**标准的**选项将创建或使用现有的`rancher-machine`安全组。如果Rancher创建`rancher-machine`安全组，它将打开所有必需的端口，以允许Rancher成功工作。`docker machine`将自动打开端口`2376`，这是Docker守护进程端口。

在**自定义**选项中，您可以选择一个现有的安全组，但是您需要确保特定端口打开，以使Rancher正常工作。

### 牧场工作所需的端口：

- 从牧场服务器到TCP端口22（SSH安装和配置Docker）和TCP端口2376（访问Docker守护程序）
- 如果您正在使用IPsec [网络驱动程序](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/exoscale/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)，从UDP端口`500`和/或UDP端口的所有其他主机`4500`
- 如果使用VXLAN [网络驱动程序](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/exoscale/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)，则从UDP端口上的所有其他主机使用`4789`

> **注意：**如果重新使用`rancher-machine`安全组，安全组中的任何缺少的端口将不会被重新打开。如果主机无法正常启动，则需要检查Exoscale中的安全组。

选择安全选项后，单击**下一步：设置实例选项**。

最后，您只需填写主机的最终详细信息。

1. 使用滑块选择要启动的主机数量。
2. 提供一个**名称**，如果需要，**说明**主机。
3. 选择**实例配置文件**。
4. 选择**根大小**，这是Exoscale磁盘大小。
5. （可选）向主机添加**标签**，以帮助组织主机并[安排服务/负载均衡器，](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/exoscale/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)或[使用主机IP以外的IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/exoscale/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)对[外部DNS记录进行编程](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/exoscale/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)。
6. （可选）`docker-machine create`使用[Docker引擎选项](https://docs.docker.com/machine/refercnce/create/#specifying-configuration-options-for-the-created-docker-cngine)自定义命令。
7. 完成后，单击**创建**。

> **注意：**目前，并不是所有的选项`docker machine`都在用户界面中暴露于Exoscale。我们使用默认的Exoscale端点（ie `https://api.exoscale.ch/compute`），image（ie `ubuntu-14.04`）和可用性区（ie `ch-gva-2`）。

一旦你点击了创建，Rancher将创建Exoscale实例，并在实例中启动rancher *-agcnt*容器。在几分钟之内，主机将被激活并可用于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/exoscale/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)。