---
title: Webhooks in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: en
---

##  Webhooks

---

在Rancher中，您可以创建接收器钩子，它提供了一个可用于触发Rancher内部操作的URL。例如，接收机钩可以与外部监视系统集成以增加或减少服务的容器。在**API** - > **Webhooks中**，您可以查看和创建新的接收器钩子。

### 添加接收器钩

要创建接收器钩子，请导航到**API** - > **Webhooks**。点击**添加接收器**。

- 为接收者提供一个**名称**，这将允许您轻松识别它。
- 选择您要创建的接收者**种类**。
- 根据接收机的类型确定接收机的动作。

单击**创建**。创建后，URL将在新添加的接收器钩子旁边提供。

### 使用接收器钩

要使用触发器网址，您需要对`POST`特定的URL进行操作。`POST`网址不需要身份验证或身体。

### 接收器钩的种类

- [缩小服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/webhook-service/index.md#scaling-a-service)
- [缩放主机数量](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/webhook-service/index.md#scaling-hosts)
- [基于DockerHub标记更新升级服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/webhook-service/index.md#upgrading-a-service-based-on-docker-hub-webhooks)

#### 缩放服务

要扩展服务，您必须配置您的webhook：

- 扩展/缩小服务（即在服务中添加或删除容器）
- 从环境中的服务列表中选择
- 一次放大/缩小多少个容器
- 服务的容器的最小/最大数量

##### 使用接收机钩子自动缩放服务的示例

通过使用接收器挂钩来扩展服务，您可以通过与外部服务集成来实现自动缩放。在我们的示例中，我们将使用Prometheus来监控服务和Alertmanager到`POST`URL。

##### 安装普罗米修斯

普罗米修斯通过[牧场目录提供](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/webhook-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)，可以在**目录**下找到。选择**Prometheus**并启动目录条目。在普罗米修斯堆栈中，找到被调用的服务`prometheus`，它在端口上暴露出来`9000`。执行到容器并去`/etc/prom-conf`。prometheus配置文件`prometheus.yml`将出现在那里。为了添加警报，为警报创建单独的文件，并提供该文件的路径`prometheus.yml`。例如，如果您创建的警报文件被调用`rules.conf`，则将其添加到`prometheus.yml`最后添加以下两行：

```
rule_files:
  - rules.conf

```

该文件`rules.conf`可以有多个警报，以下是警报的示例

###### 示例警报 `/etc/prom-conf/rules.conf`

```
ALERT CpuUsageSpike 
IF率（container_cpu_user_seconds_total {container_label_io_rancher_container_name = “演示-testTarget-1”} [30秒]）* 100> 70个
标签{ 
  严重性= “关键的”，
  动作= “向上” 
} 
注解{ 
  “需要额外的容器”摘要=，
  描述=“CPU使用率在70％以上” 
}
```

添加警报后，重新启动服务。

##### 添加Alertmanager

为了调用接收器钩子，Alertmanager将需要启动。您可以将其添加到普罗米修斯堆栈。点击普罗米修斯堆栈中的**添加服务**。使用`prom/alertmanager`添加服务。确保`9093:9093`在添加服务时映射端口。服务启动后，exec进入容器来更新`etc/alertmanager/config.yml`。在该文件中，添加webhook的URL，以便`POST`在触发警报时向URL 发送请求。使用URL信息更新文件后，重新启动服务。

###### 例 `etc/alertmanager/config.yml`

```
路线：
   repeat_interval：5h 
  路线：
  - 匹配：
       action：up 
    receiver：“ webhook-receiver-up ” 
  - match：
       action：down 
    receiver：“ webhook-receiver-down ”
 receiver：
- 名称：“ webhook-receiver-up ”
   webhook_configs：
  - url：<WEBHOOK_URL> 
    send_resolved：true 
- name：“ webhook-receiver-down ”
   webhook_configs：
  - url：<WEBHOOK_URL> 
    send_resolved：true
```

##### 自动缩放

在Prometheus和Alertmanager已使用警报和挂钩进行更新后，请确保重新启动服务，以便更新和激活配置。对于已添加警报的服务，服务将根据创建的接收器挂钩自动按比例放大或缩小。

#### 扩展主机

牧师可以通过克隆通过牧场主（即Docker机器）创建的现有主机来扩展主机。这意味着使用[自定义命令](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/webhook-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)添加的主机无法缩放。

使用[主机上的标签](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/webhook-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)，主机可以在缩放组中组合在一起。我们建议在主机上使用唯一的标签来帮助区分缩放组。任何具有相同标签的主机，无论主机如何添加到Rancher中，都将被计为缩放组的一部分。当创建webhook时，标签不需要在任何主机上，但是当使用webhook时，必须至少有一个主机具有标签，以便webhook将有一个主机克隆以便扩展。放大主机时，Rancher将使用缩放组中最长的可克隆主机。

要扩展主机，您必须配置您的webhook：

- 放大/缩小主机（即添加或删除主机）
- 添加主机选择器标签。该标签是将主机分组到缩放组的主机上的标签。
- 选择一次添加或删除的主机数量。
- 选择主机缩放组允许的主机的最小和最大数量。如果添加主机，缩放组中的主机数不能超过最大数量。如果删除主机，缩放组中的主机数不能低于最小数量。
- 如果要创建一个webhook来缩小主机，您可以选择如何删除主机的顺序的优先级（即，按最早或最新的方式开始删除）。

##### 关于扩展主机的注意事项

- **主机标签：**标签用于将主机区分为不同的缩放组。由于这些是由用户添加的，所以需要仔细选择，添加和编辑标签。在主机上添加的任何标签将自动将主机添加到缩放组中，并且如果主机是可克隆的，则可以将其用于克隆以添加更多主机。在主机上删除的任何标签将自动从缩放组中移除主机，并且不会从webhook服务中克隆或删除主机。
- **自定义主机：**任何类型的主机都可以添加到缩放组中，因为您只需要向主机添加标签即可。Rancher将无法使用这些主机来克隆和创建更多的主机。
- **克隆主机：**由于扩展是克隆主机，所有配置，包括资源分配，Docker引擎等将在任何新的主机上重复。牧师将永远使用最古老的主机进行克隆。
- **处于错误状态的**主机：在`Error`缩放组的计数中不会考虑任何状态的主机。
- **删除主机的订单：**当从牧场主删除主机，在缩放组中的任何主机将首先删除这些状态（主机`Inactive`，`Deactivating`，`Reconnecting`或者`Disconnected`开始删除之前）`Active`主机。

#### 基于Docker Hub Webhook升级服务

使用Docker Hub的webhooks，您可以添加Rancher的接收器钩子，以便在按下特定图像时，`POST`将发送一个将Rancher触发接收器挂钩。使用这种webhook的组合，您可以进行自动化，以便`image:tag`在Docker Hub中推送特定内容时，任何使用匹配版本的服务将自动升级。要选择要升级的服务组，您将使用一个选择器标签，该标签将接收包含匹配标签的任何服务。在创建服务时，应将标签添加到服务中。如果标签不存在，您将需要升级Rancher中的服务以添加webhook的标签。

为了升级服务，您必须配置您的webhook：

- 选择要升级的标签
- 选择标签以查找要升级的服务
- 确定一次要升级的容器数量（即批量大小）
- 确定在升级期间启动下一个容器之间的秒数（即批间隔）
- 选择新的容器是否应在旧容器停止之前启动

在创建接收机挂钩后，您需要在Docker Hub webhook中使用**触发器URL**。当Docker Hub触发其挂钩时，由Rancher接收器钩子拾取的服务将被升级。默认情况下，Rancher接收器挂钩需要Docker Hub webhook提供的特定信息。要使用Rancher的接收器钩子与另一个webhook，`POST`将需要包含这些字段：

```json
{
    "push_data": {
        "tag": <pushedTag>
    },
    "repository": {
        "repo_name": <image>
    }
}
```
