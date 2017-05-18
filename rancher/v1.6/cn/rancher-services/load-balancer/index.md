---
title: Load Balancers in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 负载平衡器

------

Rancher提供在Rancher内使用不同负载平衡器驱动程序的能力。负载平衡器可以通过向目标服务添加规则来将网络和应用程序流量分配到单个容器。任何目标服务将使其所有底层容器自动注册为Rancher的负载平衡器目标。。

默认情况下，Rancher使用HAProxy提供了一个托管负载平衡器，可以手动缩放到多个主机。我们计划添加额外的负载平衡器提供程序，所有负载平衡器的选项将是相同的，不管负载平衡器提供程序。

对于牛的环境，请了解有关[UI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/load-balancer/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers/#load-balancer-options-in-the-UI)和[Rancher Compose](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/load-balancer/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers/#load-balancer-options-in-rancher-compose)的负载均衡器的选项的更多信息，并使用[UI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/load-balancer/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers/#adding-a-load-balancer-in-the-ui)和[Rancher Compose](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/load-balancer/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers/#adding-a-load-balancer-with-rancher-compose)显示示例。

对于Kubernetes环境，请了解有关如何根据[云提供商](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/load-balancer/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/providers)启动外部负载均衡器服务的更多信息，或者[在Kubernetes环境中](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/load-balancer/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/ingress)使用[Rancher负载平衡器进行入口支持](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/load-balancer/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/ingress)。