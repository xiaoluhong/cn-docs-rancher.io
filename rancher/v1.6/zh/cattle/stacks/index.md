---
title: Stacks in Cattle Environments
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 堆

------

栈是一组服务。栈可用于将一起实现应用程序的服务组合在一起。

### 添加栈

在“ **栈”**页面中，单击“ **添加栈”**。您将需要提供一个**名称**，然后单击**创建**。

您将立即将其带入栈，并可以开始[添加服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/stacks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)，[添加负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/stacks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)，[添加服务别名](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/stacks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-service-alias)或[添加外部服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/stacks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-external-services)。

> **注意：**在开始任何服务之前，您需要在Rancher中启动至少一个主机。有关向Rancher添加主机的更多信息，请参阅[文档](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/stacks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)。

Rancher还提供导入`compose`文件的选项。该`docker-compose.yml`和`rancher-compose.yml`文件可以直接导入到栈的创建页面。文件可以上传到Rancher或直接粘贴到文本框中。单击“ **创建** ”按钮后，栈将与相关联的服务组成。来自`docker-compose.yml`意志的服务只会创建，但不会启动。你必须单独启动他们。

### 在栈中查看服务

从栈页面，您可以轻松地监视你所有的栈[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/stacks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)。从每个栈，您可以通过单击下拉菜单旁边的克拉展开栈以显示各个服务。

这将展开以显示栈内的任何服务以及作为服务一部分的所有容器。您可以单击任何单个容器或服务以转到详细页面。

### 栈配置

当创建服务，牧场主同时创建`docker-compose.yml`和`rancher-compose.yml`文件的筹码。在`docker-compose`YAML文件可能牧场主的以外来启动同组使用服务的`docker-compose`命令。有关更多信息，请参阅docker [-compose](https://docs.docker.com/compose/)。

该`rancher-compose.yml`文件用于管理Rancher用于启动服务的附加信息。docker-compose文件中不支持这些字段。

使用这些文件，您还可以使用“牧场主撰写”来启动将包含在“牧场主”中的服务。有关更多信息，请参阅牧场主[撰写](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/stacks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose)。

#### 查看配置

在栈下拉列表中，您可以选择**查看配置**或单击**文件图标**。

#### 导出配置

以下是导出配置文件的选项。

选项1：下载包含一个zip文件`docker-compose.yml`，并`rancher-compose.yml`选择**导出配置**栈中的下拉菜单。

选项2：通过单击要复制的文件名旁边的图标将文件复制到剪贴板。您可以复制`docker-compose.yml`文件或`rancher-compose.yml`文件。

### 图形视图

您可以在另一个视图中查看栈，其中显示了所有服务/平衡器是如何相互关联的。如果它们链接在一起，则服务名称之间有连接。

单击**图形图标**显示此视图。

#### 编辑服务

所有的Rancher服务都是以不同的方式创建的，但在创建后，它们在下拉菜单中都有相同的选项。服务和负载平衡器的所有选项都是一样的。

#### 缩放

对于服务和负载平衡器，您可以通过单击**+缩放**链接快速增加服务中的容器数量。此链接位于服务中的附加容器中。

> **注意：**对于负载平衡器，如果扩展到高于具有可用公共端口的主机数量的数量，则平衡器将停留在**更新活动**状态。如果需要任何类型的更改，您将需要启动新服务。如果卡住了，解决方法是**停止**平衡器并将缩放比例更改回可用主机数量。

您还可以通过在服务的下拉菜单中选择**“编辑”**选项来增加或减少服务中的容器数量。将鼠标悬停在该服务上时，下拉菜单可见。移动**缩放**滑块以更改服务中的数字容器。

#### 编辑

编辑服务的选择有限，因为Docker容器在创建后是不可变的（不可更改的）。您可以编辑的唯一的工件是Rancher存储的不是Docker容器的一部分的东西。这包括重新启动; 如果停止并启动它，它仍然是同一个容器。您将需要删除并重新创建服务以更改其他任何内容。更改服务的简单方法是**克隆**服务。

要查看可以更改的内容，请在服务的下拉菜单中选择**编辑**。所有服务的名称，描述和比例可以更改。如果您在设置完毕后忘记链接您的服务，您将能够通过此选项将服务链接到我们的任何服务。例如，服务，负载平衡器，服务别名和外部服务。

对于服务，大多数选项不具备动态更改的功能。Docker容器在创建后是不可变的（不可变的）。要克服这个限制，你可以**克隆**服务。这将设置启动具有相同参数的服务。您可以在创建新服务或容器之前进行更改。

#### 克隆

您可以克隆任何服务，这将保存所有配置。任何链接到原始服务将不被克隆。您需要编辑链接到原始服务的服务，并添加新克隆的服务，以便这些服务现在将链接到新服务。

例：

AppA已连接到AppB。如果您复制AppB并创建AppC，则AppA将不会链接到AppC。链接的唯一方法是编辑AppA链接到AppC。

#### 停止

您可以一次停止一个栈中的单个服务或所有服务。如果要停止单独的服务，请从服务下拉菜单中选择**停止**。如果您决定停止栈中的所有服务，您可以从栈下拉菜单中选择**停止服务**。

#### 删除

您可以删除单个服务/平衡器或删除整个栈。当您为单独的服务/平衡器选择“ **删除”**时，它将在从主机中删除之前停止容器。可能会有一点延迟，因为Rancher在从UI中删除之前清理容器。