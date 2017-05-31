---
title: Adding Azure Hosts
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 添加Azure主机

------

Rancher支持使用[Microsoft Azure](https://azure.microsoft.com/)主机进行配置`docker machine`。

### 启动Azure主机

1. 使用滑块选择要启动的主机数量。

2. 提供一个**名称**，如果需要，**说明**主机。

3. 从您的Azure帐户提供**帐户访问**信息。这包括**用户名**，**密码**，**订阅ID**和**订阅证书**。

   > **注意：**如果您粘贴在订阅证书中，它必须是base64字符串。

4. 选择要启动的**图像**。`docker machine`Rancher也支持Azure的任何支持。

5. 选择图像的**大小**。

6. 更新**SSH端口**，**Docker端口**和**Docker Swarm Master端口，**如果它们与默认端口不同。

7. 添加**发布设置文件**。

8. 选择您的Azure资源属于哪个**区域**。

9. （可选）向主机添加**标签**，以帮助组织主机并[安排服务/负载均衡器，](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/azure/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)或[使用主机IP以外的IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/azure/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)对[外部DNS记录进行编程](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/azure/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)。

10. （可选）在**高级选项中**，`docker-machine create`使用[Docker引擎选项](https://docs.docker.com/machine/refercnce/create/#specifying-configuration-options-for-the-created-docker-cngine)自定义命令。

11. 完成后，单击**创建**。

一旦您点击了创建，Rancher将创建Azure虚拟机，并在液滴中启动Rancher *-agcnt*容器。在几分钟之内，主机将处于活动状态并开始[添加服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/azure/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)。