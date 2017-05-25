---
title: Infrastructure Services in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/latest/zh/rancher-services/
---

## 基础设施服务

------

当启动Rancher时，每个[环境]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environments)都基于[环境模板]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environments/#what-is-an-environment-template)，在环境模板中，您可以选择在创建环境时要启动哪些基础架构服务。这些基础架构服务包括协调类型，[外部DNS]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cattle/external-dns-service)，[网络]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services/networking)，[存储]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services/storage-service)和框架服务（即[内部dns]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services/dns-service)， [元数据]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services/metadata-service)和[健康检查]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cattle/health-checks)）。

这些基础设施服务基于`infra-templates`我们的[Rancher目录](https://github.com/rancher/rancher-catalog)和[社区目录](https://github.com/rancher/community-catalog)的文件夹中的[模板](https://github.com/rancher/community-catalog)。默认情况下，Rancher目录和社区目录均已启用，它们提供了可在环境模板中使用的基础架构服务的基本列表。

创建新的环境模板时，自动启用需要具有工作环境的默认基础设施服务。