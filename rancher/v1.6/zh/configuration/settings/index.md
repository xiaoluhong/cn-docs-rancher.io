---
title: Settings in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 设置

------

在Rancher 的**管理** - > **设置**页面中，我们允许为产品的不同区域定制Rancher。

### 主机注册

在启动任何主机之前，将要求您完成主机注册。此注册设置您的Rancher服务器将如何连接到您的主机。如果您已经设置了[访问控制](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/settings/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)，则不会提示您设置主机注册，因为Rancher假定您的URL将被访问。

该设置确定您的主机将用于连接到Rancher API的基本URL。默认情况下，Rancher会选择用于访问UI的基本URL。如果您选择更改地址，请确保指定应用于连接到Rancher API的端口。如果您使用SSL配置Rancher，请确保将协议更改为`https`。此注册设置确定[添加自定义主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/settings/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)的命令。

如果Rancher打开[访问控制](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/settings/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)功能，只有**管理员**才能更改主机注册。默认情况下，第一个**管理员**是配置访问控制时第一个使用Rancher验证的用户。如果访问控制仍未配置，则该站点的任何用户都可以更新主机注册。可以在**管理** - > **主机注册**选项卡中更新此选项。

### 目录

默认情况下，[目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/settings/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)已启用三个目录：

- [Rancher Infrastructure](https://github.com/rancher/infra-catalog)包含所有基础架构服务的模板。
- [Rancher认证图书馆](https://github.com/rancher/rancher-catalog)包含Rancher认证应用程序的模板。
- [社区贡献](https://github.com/rancher/community-catalog)包含由社区贡献的应用程序的模板。

由于“设置”页面仅适用于[管理员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/settings/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/accounts/#admin)，因此只有管理员才能向“专家”添加专用目录。添加目录与添加目录名称和git URL一样简单。git URL的正确格式可以在[这里](https://git-scm.com/docs/git-clone#_git_urls_a_id_urls_a)找到。每当您添加目录，它将立即在目录中可用。

如果要创建自己的私有目录来添加，则git仓库必须以[特定的格式设置](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/settings/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog/private-catalog)。

### 统计

默认情况下，Rancher要求您选择收集有关Rancher配置的匿名统计信息。这些数据使我们能够更好地了解我们的用户群，以及如何使Rancher更好。阅读更多关于[Rancher遥测技术的信息](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/settings/%7B%7Bsite.baseurl%7D%7D/rancher/telemetry)。