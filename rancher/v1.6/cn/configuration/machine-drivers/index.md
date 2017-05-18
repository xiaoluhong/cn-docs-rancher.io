---
title: Machine Drivers in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 机器驱动程序

------

[Docker机器](https://docs.docker.com/machine/)驱动程序可以添加到Rancher中，以便可以[将主机添加](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/machine-drivers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/other)到Rancher。只有[管理员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/machine-drivers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#admin)可以更改哪些机器驱动程序可见，哪些可以位于 **管理** - > **机器驱动程序中**。

该**活动的**机器驱动程序是显示在唯一的选择**基础设施** - > **添加主机**页面。默认情况下，Rancher提供了许多机器驱动程序，在我们的UI 中只有一个子集是**Active**。

### 添加机器驱动程序

您可以通过单击**添加机器驱动程序**轻松添加自己的机器驱动**程序**。

1. 提供**下载URL**。这个URL是机器驱动程序二进制64位Linux。
2. （可选）提供**自定义UI URL**来加载驱动程序的**自定义**添加主机屏幕。该[UI的驱动程序库的skel](https://github.com/rancher/ui-driver-skel)对如何设置它的详细信息。
3. （可选）提供**校验和**以验证下载的驱动程序与预期的校验和匹配。
4. 完成后，单击**创建**。

点击创建后，Rancher会添加额外的驱动程序，并在添加[其他主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/machine-drivers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/other)或自定义映像（如果提供）的**驱动程序**字段中显示此选项。