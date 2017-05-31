---
title: Health Checks in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 健康检查

------

在牛环境中，Rancher通过运行`healthcheck`基础架构服务实现健康监控系统，该服务`healthcheck`在其主机上启动集装箱，以协调容器和服务的分布式健康检查。这些容器内部使用HAProxy来验证应用程序的运行状况。当在单个容器或服务上启用健康检查时，每个容器将由最多三个`healthcheck`容器在不同的主机上运行进行监控。如果至少有一个HAProxy实例报告“通过”健康检查，并且当所有HAProxy实例报告“不健康”健康检查时，该容器被认为是不健康的，则该容器被认为是健康的。

> **注意：**此模型的唯一例外是当您的环境包含单个主机时，在这种情况下，运行状况检查将由同一主机执行。

Rancher处理网络分区，比基于客户端的健康检查更有效。通过使用HAProxy执行运行状况检查，Rancher使用户可以在应用程序和负载平衡器之间指定相同的运行状况检查策略。

> **注意：**运行状况检查仅适用于正在使用受管网络的服务。如果选择任何其他网络选项，则**不会**被监视。

### 组态

使用以下选项配置运行状况检查：

**检查类型**：有两种类型的检查 - *TCP连接打开*（仅验证端口打开）和*HTTP响应2xx / 3xx*（执行HTTP请求并确保收到良好的响应）。

**HTTP请求**：如果检查类型为*HTTP Responds 2xx / 3xx*，则必须指定要查询的URL路径。您可以选择请求方法（`GET`，，`POST`etc）以及HTTP版本（`HTTP/1.0`，`HTTP/1.1`）。

**端口**：执行检查的端口。

**初始化超时：**我们退出初始化之前的毫秒数。

**检查间隔**：检查之间的毫秒数。

**检查超时**：没有响应**超时**的检查之前的毫秒数。

**健康阈值**：在（当前标记的不健康）容器之前成功检查响应的数量再次被认为是健康的。

**不健康的门槛**：在（当前标记健康的）容器之前的失败检查响应的数量被认为是不健康的。

**当不健康时**：当容器被认为是不健康时，有3种选择。`Take no action`意味着容器将保持不健康状态。`Re-create`意味着Rancher会破坏不健康的容器并为服务创建一个新的容器。 `Re-create, only when at least X container is healthy`意味着如果有`X`多个容器健康，则不健康的容器将被破坏并重新创建。

### 在UI中添加健康检查

对于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/health-checks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)或[负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/health-checks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)，可以通过导航到**健康检查**选项卡来添加**健康检查**。您可以检查服务的TCP连接或HTTP响应，并更改运行状况检查配置的默认值。

### 将健康检查添加到Rancher Compose中的服务

使用[Rancher Compose](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/health-checks/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose)，可以添加健康检查`rancher-compose.yml`。

在我们的示例中，如果容器发现不健康，我们会显示三种不同策略的健康检查配置。

```
版本：' 2 '
服务：
   服务1：
     刻度：1 
    health_check：
       ＃哪个端口来执行对检查
      端口：80 
      ＃对于TCP，request_line需要是''或未示出
      ＃ TCP实例：
      ＃ request_line： '' 
      request_line：GET / healthcheck HTTP / 1.0 
      ＃间隔以毫秒为单位测量
      间隔：2000 
      initializing_timeout：60000 
      unhealthy_threshold：3 
      ＃策略适合做什么，当不健康
      ＃在该服务中，也不会发生作用时，容器被发现不健康的
      策略：无
      healthy_threshold：2 
      ＃响应超时以毫秒为单位
      RESPONSE_TIMEOUT：2000 
  客服2：
     规模：1 
    health_check：
       ＃哪个端口对
      端口进行检查：80 
      ＃间隔以毫秒为单位测量
      间隔：2000 
      initializing_timeout：60000 
      unhealthy_threshold：3 
      ＃在不健康的情况下做什么的策略
      ＃在这项服务中，Rancher将重新创建任何不健康的容器
      策略：重新创建
      health_threshold：2 
      ＃响应超时以毫秒为
      单位response_timeout：2000 
  service3：
     scale：1 
    health_check：
       ＃哪个端口对
      端口进行检查：80 
      ＃间隔以毫秒为单位测量
      间隔：2000 
      initializing_timeout：60000 
      unhealthy_threshold：3 
      ＃在不健康的情况下做什么的策略
      ＃在这项服务中，只有至少有1个容器
      ＃是健康的
      策略，Rancher才会重建任何健康的容器：recreateOnQuorum 
      recreate_on_quorum_strategy_config：quorum：1 
      healthy_threshold：2 
      ＃响应超时以毫秒为单位测量
      response_timeout：20003 ＃策略在不健康的情况下做什么＃在这项服务中，只有至少有1个容器＃是健康的策略，Rancher才会重建任何健康的容器：recreateOnQuorum recreate_on_quorum_strategy_config：quorum：1 healthy_threshold：2 ＃响应超时以毫秒为单位response_timeout：20003 ＃策略在不健康的情况下做什么＃在这项服务中，只有至少有1个容器＃是健康的策略，Rancher才会重建任何健康的容器：recreateOnQuorum recreate_on_quorum_strategy_config：quorum：1 healthy_threshold：2 ＃响应超时以毫秒为单位response_timeout：2000只有至少有1个容器＃是健康的策略， Rancher才会重建任何健康的容器：recreateOnQuorum recreate_on_quorum_strategy_config：quorum：1 healthy_threshold：2 ＃响应超时以毫秒为单位response_timeout：2000只有至少有1个容器＃是健康的策略， Rancher才会重建任何健康的容器：recreateOnQuorum recreate_on_quorum_strategy_config：
         quorum：1 healthy_threshold：2 ＃响应超时以毫秒为单位response_timeout：20002 ＃响应超时以毫秒为单位测量response_timeout：20002 ＃响应超时以毫秒为单位测量response_timeout：2000
```

### 故障情况

| 脚本                                  | 响应                                       |
| ----------------------------------- | ---------------------------------------- |
| 被监测的容器停止响应健康检查。                     | 正在监视容器的所有活动的HAProxy实例将检测到故障，并将容器标记为“不健康”。如果容器是服务的一部分，则Rancher通过其服务HA功能将服务恢复到其预定义的缩放比例。 |
| 运行健康检查的容器的主机失去了网络连接或该主机上的代理。        | 当网络连接丢失到主机时，与代理服务器的连接丢失。由于代理不可访问，主机被标记为“重新连接”。目前，所有这些都知道的是，Rancher服务器无法连接到该主机的主机代理。对Rancher的健康检查是针对容器本身而不是主机完成的; 因此，容器将无法访问其所有活动的HAProxy实例。如果容器是服务的一部分，则Rancher通过其服务HA功能将服务恢复到其预定义的缩放比例。 |
| 运行启用了健康检查的容器的主机完全失败。                | 当主机遇到完全故障（如断电）时，与代理服务器的连接将从Rancher服务器中丢失。由于代理不可访问，主机被标记为“重新连接”。目前，所有这些都知道的是，Rancher服务器无法连接到该主机的主机代理。对Rancher的健康检查是针对容器本身而不是主机完成的; 因此，容器将被其所有活动的HAProxy实例无法访问。如果容器是服务的一部分，则Rancher通过其服务HA功能将服务恢复到其预定义的缩放比例。 |
| 主机的代理失败，但是主机保持在线，容器正在运行，并且正在进行健康检查。 | 在这种情况下，如之前的情况，与代理服务器的连接丢失。由于代理不可访问，主机被标记为“重新连接”。目前，所有这些都知道的是，Rancher服务器无法连接到该主机的主机代理。对Rancher的健康检查是针对容器本身而不是主机完成的; 因此，容器将无法访问其所有活动的HAProxy实例。如果容器是服务的一部分，则Rancher通过其服务HA功能将服务恢复到其预定义的缩放比例。 |

根据健康检查的结果，容器处于绿色或红色状态。如果实施该服务的所有容器处于绿色状态，或者如果所有容器都处于红色状态，则服务处于红色（或“向下”）状态，处于绿色（或“向上”）状态。如果Rancher检测到至少一个容器处于红色状态或者将其返回到绿色状态，则服务处于黄色（或“退化”）状态。

检测故障所用的时间是通过“间隔”值进行控制的，该值是通过撰写或UI创建健康检查时定义的。

> **注意：**故障恢复操作仅在容器变为*绿色*后执行。也就是说，如果服务的启动时间很长，则容器将不会立即重新启动，因为服务需要超过2000ms才能启动。健康检查首先需要在采取任何其他行动之前将容器变绿。