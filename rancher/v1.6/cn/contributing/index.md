---
title: Contributing to Rancher
layout: rancher-default-v1.6
version: v1.6
lang: en
redirect_from:
  - /rancher/latest/en/contributing/
---

## 为牧场主贡献

------

### 发展

在我们的GitHub [存储库中](https://github.com/rancher/rancher)，我们的wiki包含开始使用Rancher的所有步骤。

从我们的第一个[牛头](https://github.com/rancher/rancher/wiki/Cowpoke-1:-Getting-Started-with-Rancher)开始吧！

### 库

所有的仓库位于我们的主要GitHub [页面中](https://github.com/rancher)。有很多仓库用于Rancher，但是我们将提供Rancher中使用的一些主要的介绍。

[Rancher Repo](https://github.com/rancher/rancher)：这个回购是将所有其他回卷整合在一起的主要回购。

[牛](https://github.com/rancher/cattle)回报：这个回购是开发牧场主的功能的地方。

[UI Repo](https://github.com/rancher/ui)：这个repo是Rancher开发的所有UI。

[牧场主目录回购](https://github.com/rancher/rancher-catalog)：此回购了大部分的[基础设施服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/contributing/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)为模板[牧场主目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/contributing/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)中`infra-templates`的文件夹，这是在使用[环境中](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/contributing/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)的一部分[环境模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/contributing/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/#what-is-an-environment-template)。我们欢迎社区对Rancher目录的贡献，以获得额外的[负载均衡器供应商](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/contributing/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/%7D%7D)。

[社区目录回收](https://github.com/rancher/community-catalog)：此回购具有所有社区提供的牧场主[目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/contributing/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)模板。我们欢迎社区对Rancher模板的贡献。

[Rancher CLI Repo](https://github.com/rancher/cli)：这个repo是[Rancher CLI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/contributing/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli) CLI所基于的。

[Rancher Compose Repo](https://github.com/rancher/rancher-compose)：这个repo是[Rancher Compose](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/contributing/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose) CLI所基于的。它与docker / libcompose同步。

### 错误

如果您发现任何错误或有任何问题，请通过提出[问题](https://github.com/rancher/rancher/issues/new)与我们联系。虽然我们有很多与Rancher有关的存储库，但我们希望在[Rancher repo](https://github.com/rancher/rancher)中的大部分bug都可以让我们不要错过！

如果您对我们的[文档](https://github.com/rancher/rancher.github.io)有任何更新，请将任何PR转到我们的[文档中，](https://github.com/rancher/rancher.github.io)或单击**编辑此页面**直接进入该页面。 