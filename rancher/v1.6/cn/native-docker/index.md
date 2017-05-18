---
title: Using Native Docker CLI with Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 使用Native Docker CLI与Rancher

------

Rancher与本机Docker CLI集成，可以与其他DevOps和Docker工具一起使用。在高层次上，这意味着如果您启动，停止或销毁Rancher外的容器，Rancher会检测到这些更改并进行相应更新。

### Docker事件监控

通过监控所有主机上的Docker事件，实时更新Rancher。因此，当容器在Rancher之外启动，停止或销毁时（例如`docker stop sad_einstein`直接在主机上执行），Rancher会检测到该更改并相应地更新其状态。

> **注意：**目前的一个限制是我们等到容器启动（未创建）才能将其导入Rancher。运行`docker create ubuntu`不会导致容器出现在Rancher UI中，而是运行`docker start ubuntu`或`docker run ubuntu`将要运行。

您可以通过`docker evcnts`在主机的命令行上执行来观察Rancher正在监视的Docker事件流。

### 加入本地将集装箱启动到Rancher网络

您可以在Rancher外部启动集装箱，还可以将其加入Rancher管理网络。这意味着这些容器可以参与跨主机联网。要启用此功能，请在创建容器时添加`io.rancher.container.network`带有值的标签`true`。以下是一个例子：

```
$ docker run -l io.rancher.container.network = true -itd ubuntu bash
```

要了解有关Rancher管理网络和跨主机网络的更多信息，请阅读[Rancher中的网络](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/native-docker/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)。

### 导入现有容器

主机注册时，Rancher还支持导入现有的容器。当您使用UI 中的[自定义命令](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/native-docker/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)注册主机时，主机上的任何容器将被检测并导入到Rancher。

### 定期同步状态

除了实时监控Docker事件之外，Rancher会定期与主机同步状态。每五秒钟，主机将所有容器报告给Rancher，以确保Rancher中的预期状态与主机上的实际状态相匹配。这可以防止网络中断或服务器重新启动，导致Rancher错过Docker事件。当以这种方式同步状态时，主机上的容器状态将永远是真实的根源。所以，例如，如果Rancher认为容器正在运行，但实际上在主机上停止运行，那么Rancher会将容器的状态更新为停止。它不会尝试重新启动容器。