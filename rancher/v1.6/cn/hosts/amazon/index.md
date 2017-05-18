---
title: Adding Amazon EC2 Hosts
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 添加Amazon EC2主机

------

Rancher支持配置[Amazon EC2](http://aws.amazon.com/ec2/)主机`docker machine`。

### 查找AWS证书

在AWS上启动主机之前，您需要找到您的AWS帐户凭据以及您的安全组信息。该**帐户访问**信息可以使用亚马逊的找到[文件](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSGettingStartedGuide/AWSCredcntials.html)找到正确的钥匙。创建**访问密钥**和**秘密密钥**时，请确保将其保存在某处，除非您创建新的密钥对，否则它将不可用。

### 启动Amazon EC2主机

在基础结构 - >主机选项卡下，单击**添加主机**。选择**Amazon EC2**图标。选择您所需的**区域**。提供您的AWS **Access密钥**和**密钥**，单击**下一步：验证并选择一个网络**。Rancher将使用您的凭据来确定AWS中可用于启动实例的内容。

您需要选择可用性区域来创建实例。根据您选择的区域，将显示可用的VPC ID和子网ID。选择**VPC ID**或**子网ID**，然后单击**下一步：选择安全组**。

接下来，您将选择要用于主机的安全组。安全组有两种选择。该**标准的**选项将创建或使用现有的`rancher-machine`安全组。如果Rancher创建`rancher-machine`安全组，它将打开所有必需的端口，以允许Rancher成功工作。`docker machine`将自动打开端口`2376`，这是Docker守护进程端口。

在**自定义**选项中，您可以选择一个现有的安全组，但是您需要确保特定端口打开，以使Rancher正常工作。

### 牧场工作所需的端口：

- 从`22`牧场服务器到TCP端口（SSH安装和配置Docker）
- 如果您正在使用IPsec [网络驱动程序](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/amazon/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)，从UDP端口`500`和/或UDP端口的所有其他主机`4500`
- 如果使用VXLAN [网络驱动程序](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/amazon/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)，则从UDP端口上的所有其他主机使用`4789`

> **注意：**如果重新使用`rancher-machine`安全组，安全组中的任何缺少的端口将不会被重新打开。如果主机无法正常启动，则需要检查AWS中的安全组。

选择安全选项后，单击**下一步：设置实例选项**。

最后，您只需填写主机的最终详细信息。

1. 使用滑块选择要启动的主机数量。
2. 提供一个**名称**，如果需要，**说明**主机。
3. 选择要启动的**实例类型**。
4. 选择图像的**根大小**。默认值`docker machine`是16GB，这是我们在Rancher中默认的。
5. （可选）对于**AMI**，`docker machine`在特定区域中默认使用Ubuntu 16.04 LTS映像。您还可以选择自己的AMI。如果您输入自己的AMI，请确保在该地区可用！
6. （可选）提供要用作实例配置文件的**IAM配置**文件。
7. （可选）向主机添加**标签**，以帮助组织主机并[安排服务/负载均衡器，](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/amazon/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)或[使用主机IP以外的IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/amazon/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)对[外部DNS记录进行编程](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/amazon/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)。
8. （可选）在**高级选项中**，`docker-machine create`使用[Docker引擎选项](https://docs.docker.com/machine/refercnce/create/#specifying-configuration-options-for-the-created-docker-cngine)自定义命令。
9. 完成后，单击**创建**。

Rancher将创建EC2实例，并在该实例中启动Rancher *-agcnt*容器。在几分钟之内，主机将被激活并可用于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/amazon/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)。