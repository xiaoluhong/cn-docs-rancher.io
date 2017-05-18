---
title: DNS Service in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## DNS服务

------

Rancher通过使用自己的轻量级DNS服务器和高可用性的控制平面来为分布式DNS服务提供基础设施服务。每个健康容器在链接到另一个服务或添加到服务别名时自动添加到DNS服务。当通过服务名称查询时，DNS服务返回实施该服务的健康容器的随机IP地址列表。

- 默认情况下，同一堆栈中的所有服务都将添加到DNS服务中，而不需要明确的服务链接，可以在**服务**中的“ **服务链接”**下设置。
- 您可以通过服务名称解析相同堆栈中的容器。
- 如果您需要服务的自定义DNS名称，这与您的服务名称不同，您将需要设置一个链接以获取自定义DNS名称。
- 如果使用服务别名，则仍然需要链接。
- 要使服务可分解为不同的堆栈，您可以使用`<service_name>.<stack_name>`并且不需要显式链接，可以在**服务**中的“ **服务链接”**下设置。

因为Rancher的覆盖网络为每个容器提供了一个不同的IP地址，所以您不需要处理端口映射，并且不需要处理像重复的服务在不同端口上侦听的情况。因此，简单的DNS服务足以处理服务发现。

了解有关[牛环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/dns-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/internal-dns-service)的内部DNS服务的更多信息。