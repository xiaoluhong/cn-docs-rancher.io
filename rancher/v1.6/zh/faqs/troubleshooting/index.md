---
title: Troubleshooting FAQs about Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
redirect_from:
  - /rancher/v1.6/cn/faqs/
  - /rancher/faqs/
  - /rancher/faqs/troubleshooting/
---

## 故障排除常见问题

------

请阅读有关[Rancher服务器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/faqs/server)和[Rancher代理/主机的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/faqs/agcnts)更多详细常见问题。

本节假设您能够成功启动Rancher服务器并添加主机。

### 服务/集装箱

#### 为什么只能编辑容器的名称？

Docker容器在创建后是不可变的（不可变的）。唯一可以编辑的东西是我们存储的，不是Docker容器的一部分。这包括重新启动，如果停止并启动它，它仍然是同一个容器。您将需要删除并重新创建一个容器来更改任何其他内容。

您可以**克隆**，这将预先填充**添加容器**屏幕与现有容器的所有设置。如果您忘记了一件事情，您可以克隆容器，更改容器，然后删除旧的容器。

#### 连接容器/服务如何在Rancher工作？

在Docker中，链接的容器（使用`--link`in `docker run`）显示在`/etc/hosts`与其链接的容器中。在牧场主，我们不编辑`/etc/hosts`。相反，我们运行[内部DNS服务器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/internal-dns-service)，使链接可以跨主机访问。DNS服务器将使用正确的IP进行响应。

#### 帮帮我！我不能从UI执行shell或查看容器的日志。Rancher如何访问容器的shell /日志？

由于代理商可能对公共互联网开放，因此对容器的shell（或日志等）的代理的请求不会被自动信任。来自Rancher Server的请求包括一个JWT（JSON Web Tokcn），并且该JWT由服务器签名，并且可由代理验证其实际来自服务器。其中的一部分包括到期时间，距离发布时间为5分钟。这可以防止令牌长时间被使用，如果它被拦截，这是不重要的，如果不使用SSL。

如果运行docker日志-f rancher-agcnt，并且日志显示有关过期令牌的消息，那么请检查Rancher Server主机和Rancher Agcnt主机的日期/时间是否同步。

#### 我在哪里可以看到我的服务日志？

在服务详细信息中，我们在名为**Log**的选项卡中提供服务日志。在**日志**选项卡中，它列出与服务相关的所有事件，包括时间戳和API中发生的事件的描述。这些日志会被保留24小时才能被删除。

### 跨主机通信

如果不同主机上的容器无法ping通，则有一些常见的情况可能是问题。

#### 如何检查跨主机通信是否正常？

在**堆栈** - > **基础架构堆栈中**，检查`healthcheck`堆栈的状态。如果堆栈处于活动状态，则交叉主机通信正在运行。

要手动测试，您可以执行任何容器并ping另一个容器的内部IP（即10.42.xx）。基础架构堆栈的容器可能会隐藏在主机页面上。要查看它们，请选中右上角的“显示系统”复选框。

#### 用户界面中主机的IP地址是否正确？

经常地，主机的IP将意外地接收码头桥IP而不是实际的IP。这些通常`172.17.42.1`或开始`172.17.x.x`。如果是这种情况，您需要通过`CATTLE_AGcnT_IP`在`docker run`命令中显式设置环境变量来重新注册具有正确IP的主机。

```
$ sudo docker run -d -e CATTLE_AGcnT_IP = < HOST_IP > --privileged \
    -v /var/run/docker.sock:/var/run/docker.sock \
    牧师/代理：v0.8.2 http：// SERVER_IP：8080 / v1 / scripts / xxxx
```

#### 运行Ubuntu，容器无法相互通信。

如果您已`UFW`启用，您可以禁用`UFW`或更改`/etc/default/ufw`为：

```
DEFAULT_FORWARD_POLICY="ACCEPT"

```

#### Rancher使用的子网已经在我的网络中使用，并禁止受管网络。如何更改子网？

要更改用于组网联网的子网，您需要确保要使用的[网络](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking/#subnets)基础结构服务`default_network`在`rancher-compose.yml`文件中具有正确的[子网](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking/#subnets)。

要更改Rancher的IPsec或VXLAN网络驱动程序，您将需要具有更新的基础架构服务的[环境模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#what-is-an-cnvironmcnt-template)。创建新环境模板或编辑现有环境模板时，可以通过单击**编辑配置**来**编辑**网络基础设施服务的**配置**。在编辑屏幕中，您可以输入不同的子网，然后单击**配置**。任何**新的**使用更新后的环境模板环境将使用新的子网。编辑现有环境模板将不会更新现有环境中的基础架构服务。

> **注意：**以前通过API更新子网的方法将不再适用，因为Rancher已转移到基础架构服务。

### DNS

### 如何查看我的DNS设置是否正确？

如果您想查看Rancher DNS设置的配置，请访问**Stacks** - > **Infrastructure**。查找`network-services`堆栈并选择`metadata`服务。在`metadata`服务中，exec进入任何指定的容器`network-services-metadata-dns-X`。您可以使用UI并在容器上选择**Execute Shell**。

```
$ cat /etc/rancher-dns/answers.json
```

#### CcntOS的

##### 为什么我的容器无法连接到网络？

如果您在主机（即`docker run -it ubuntu`）上运行容器，并且容器不能与互联网或主机外的任何东西通话，那么您可能会遇到网络问题。

CcntOS将默认设置`/proc/sys/net/ipv4/ip_forward`为`0`，这将基本上是所有网络的Docker。Docker设置此值，`1`但如果您`service restart networking`在CcntOS上运行，则将其设置为`0`。

### 负载平衡器

#### 为什么我的负载均衡器卡在`Initializing`>？

负载平衡器自动对其启用[健康检查](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/health-checks)。如果负载平衡器卡在`initializing`状态，则很可能[主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/index.md#cross-host-communication)之间的[跨主机通信不起作用](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/index.md#cross-host-communication)。

#### 我如何看到我的负载均衡器的配置？

如果要查看负载平衡器的配置，则需要执行特定的负载均衡器容器并查找配置文件。您可以使用UI并在容器上选择**Execute Shell**。

```
$ cat /etc/haproxy/haproxy.cfg
```

该文件将提供负载平衡器的所有配置详细信息。

#### 在哪里可以找到HAProxy的日志？

HAProxy的日志可以在负载平衡器容器内找到。`docker logs`的负载均衡器容器将仅提供与负载平衡器相关的服务的详细信息，但不提供实际的HAProxy日志记录。

```
$ cat /var/log/haproxy

```

### 哈

#### 牧师撰写执行者和Go-Machine-Service不断重新启动。

在HA集中，如果您正在使用代理服务器后，如果生产者配合执行器和go-machine-service不断重新启动，请确保正在使用代理协议。

### 认证

#### 帮帮我！我打开[访问控制](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)，不能再访问Rancher。如何重置牧场手来停用访问控制？

如果您的身份验证出现问题（例如您的GitHub身份验证已损坏），则可能会将其锁定在Rancher中。要重新获得对Rancher的访问权限，您需要关闭数据库中的Access Control。为此，您需要访问运行Rancher Server的计算机。

```
$ docker exec -it < rancher_server_container_ID > mysql
```

> **注：**在`<rancher_server_container_ID>`将具有牧场主数据库容器。如果您[升级](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/upgrading)并创建了一个Rancher数据容器，则需要使用Rancher数据容器的ID而不是Rancher服务器容器。

访问牛数据库。

```
的MySQL >使用牛;
```

查看`setting`表。

```
的MySQL >  选择 *从设置;  
```

更新`api.security.cnabled`到`false`并清除该`api.auth.provider.configured`值。此更改将关闭访问控制，任何人都可以使用UI / API访问Rancher服务器。

```
mysql > update setting set value = “ false ”其中name = “ api.security.cnabled ” ; 
mysql > update setting set value = “ ”其中name = “ api.auth.provider.configured ” ;
```

确认已在`setting`表中进行更改。

```
的MySQL >  选择 *从设置;  
```

可能需要约1分钟才能在用户界面中关闭身份验证，但您可以刷新网页并访问Rancher，并关闭访问控制。