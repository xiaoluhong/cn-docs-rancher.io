---
title: Containers
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 容器

------

### 添加容器

通常，我们建议人们使用[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/containers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-ui/applications/stacks/adding-services)添加容器，因为它为用户提供了一些更多的灵活性，但有时我们明白您可能需要旋转一个容器。

在“ **基础结构** - > **容器”**页面中，单击“ **添加容器”**。`docker run`Rancher中还支持任何支持创建容器的选项。

1. 提供**名称**，如果需要，**容器**的说明。

2. 提供**图像**使用。您可以使用[DockerHub](https://hub.docker.com/)上的任何图像以及已添加到Rancher的任何[注册表](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/containers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/registries)。图像名称的语法将匹配任何`docker run`命令。

   图像名称的语法。默认情况下，我们从docker注册表中提取。如果没有指定标签，我们将拉出最新的标签。

   `[registry-name]/[namespace]/[imagename]:[version]`

   ​

3. 如果需要，设置端口映射，这证明了通过主机IP访问容器上暴露的端口的能力。在“ **端口映射”**部分中，定义将用于与容器进行通信的公共端口。您还定义用于连接到容器的暴露端口的专用端口。

   **随机端口映射**

   如果您想利用Rancher的随机端口映射，公共端口将为空，您将需要定义私有端口。专用端口通常是容器上的暴露端口之一。

   > **注意：**当Rancher暴露端口时，Rancher将`docker ps`管理iptable规则以使端口完全动态，不会出现。

4. 在各种选项卡中，Docker中可用的所有选项均可用于Rancher。默认情况下，我们设置了`-i -t`。

   如果您选择从“ **基础设施”** - >“ **容器”**页面添加容器，则“ Rancher”会自动为您选择一个主机。否则，如果您选择了一个主机来添加容器，主机将被安装在“ **安全/主机** ”选项卡中。

   还可以向容器添加标签以及应用调度规则。有关标签和计划的更多细节可以在[这里](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/containers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-ui/scheduling)阅读。

5. 完成填写您的容器选项后，单击**创建**。

## 编辑容器

------

从容器的下拉列表中，您可以选择不同的操作来在容器上执行。在主机或服务器上查看容器时，可以通过将鼠标悬停在容器名称上来查找下拉图标。在“ **基础设施”** - >“ **容器** ”中，仅在主机上创建的容器才可以看到下拉图标。通过服务创建的任何容器都不会显示其下拉图标。

您可以随时点击容器名称，这将带您进入容器详细信息页面。在该页面上，下拉菜单位于容器状态旁边的右上角。

当您从下拉菜单中选择**“编辑”**时，您将只能更改容器的名称和说明。Docker容器在创建后是不可变的（不可变的）。唯一可以编辑的东西是我们存储的，不是Docker容器的一部分。这包括重新启动，如果停止并启动它，它仍然是同一个容器。您将需要删除并重新创建一个容器来更改任何其他内容。

> **注意：**当Rancher暴露端口时，Rancher将`docker ps`管理iptable规则以使端口完全动态，不会出现。

您可以**克隆**，这将预先填充**添加容器**屏幕与现有容器的所有设置。如果您忘记了一件事情，您可以克隆容器，更改容器，然后删除旧的容器。

### 改变集装箱国家

当容器处于**运行**状态时，可以**停止**容器。这将停止主机上的容器，但不会将其移除。容器处于“ *停止”*状态后，可以选择**“开始”**以使容器再次开始运行。另一个选项是**重新启动**容器，该容器将在一个步骤中停止并启动容器。

您可以**删除**容器并将其从主机中删除。

### 执行Shell

当您选择**执行Shell**，它将带您进入容器shell提示符。

### 查看日志

查看容器日志发生的情况总是有用的。单击**查看日志**提供相当于`docker logs <CONTAINER_ID>`在主机上。