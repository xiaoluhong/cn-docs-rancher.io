---
title: Rancher Catalog
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/catalog/
  - /rancher/latest/zh/catalog/
---

## 目录

------

Rancher提供了一个应用程序模板的目录，可以轻松部署这些复杂的堆栈。通过访问“ **目录** ”选项卡，可以查看[启用的目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/catalog/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/settings/#catalog)中可用的所有模板。该**图书馆**目录包含模板[牧场主认证的产品目录](https://github.com/rancher/rancher-catalog)和**社区**目录包含模板 [社区目录](https://github.com/rancher/community-catalog)。*牧师*只会保持对图书馆认证模板的支持。

Rancher 的管理员可以在Rancher中添加或删除目录。Rancher中的目录可以在**Admin** - > **Settings中**找到。添加目录与添加目录名称，URL和分支名称一样简单。该URL需要一个`git clone` 可以处理的URL 。分支名称必须是目录URL中的分支。它将`master`默认使用。每当您添加目录条目时，它将立即在您的目录中可用。

如果您在代理服务器后面运行Rancher服务器，则需要[使用某些环境变量启动Rancher，](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/catalog/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#http-proxy)以便Rancher目录在Rancher中运行。

### 目录中的基础设施服务

可在[环境模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/catalog/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/#what-is-an-environment-template)中启用的[基础架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/catalog/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)来自Rancher中启用的任何目录的文件夹。`infra-templates`

这些服务也可从“ **目录** ”选项卡中获得，即使可能无法使用所选的编排类型，您也可以查看所有基础架构服务。建议在环境模板创建期间选择基础架构服务，而不是直接从目录中启动它们。

### 启动模板

搜索所需的模板或使用过滤器进行分类或目录。找到您的模板后，点击**启动**。填写模板所需的信息。

1. 默认情况下，选择最新**版本**的模板，但如果需要，您可以选择较旧的版本。
2. 选择一个**堆栈**如果需要，名称和**描述**堆栈。
3. 填写**配置选项**，这是特定于所选模板的问题。
4. 单击“ **创建”**，根据模板创建堆栈。您可以在创建堆栈之前通过展开**“预览”**来查看用于生成堆栈的文件`docker-compose.yml`和`rancher-compose.yml`文件。****

单击“ **创建”后**，立即创建堆栈，但没有任何服务已启动。从堆栈下拉菜单中单击**启动服务**以启动堆栈的所有服务。

### 升级模板

关于Rancher的好处是，如果较新版本的模板已经上传到目录，我们会通知您有一个较新版本可升级到。当您单击**升级可用**时，您可以选择要升级到哪个版本。在升级之前，请始终查看有关版本的信息，以便您了解所有潜在的风险。选择版本后，在点击**保存**之前，需要对**配置选项**进行检查。****

所有服务升级后，堆栈和服务将处于**升级**状态。如果您对升级感到满意，最后一步是通过单击堆栈下拉菜单中的**完成升级**来确认升级。**注意：完成升级后，您将无法恢复到旧版本。**

#### 回滚

如果升级过程中出现问题，并且需要恢复到以前的版本，则需要在堆栈下拉菜单中选择“ **回滚** ”。