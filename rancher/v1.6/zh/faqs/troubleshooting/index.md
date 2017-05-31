---
title: Troubleshooting FAQs about Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/v1.6/zh/faqs/
  - /rancher/faqs/
  - /rancher/faqs/troubleshooting/
---

## 故障排除常见问题

------

请阅读有关[Rancher服务器]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//faqs/server)和[Rancher代理/主机的]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//faqs/Agents)更多详细常见问题。

本节假设您能够成功启动Rancher服务器并添加主机。

### 服务/容器

#### 为什么只能编辑容器的名称？

Docker容器在创建后是不可变的，你能编辑的东西只有我们存储的东西，而不是Docker容器的一部分。这包括重新启动，如果停止并重新启动它，它仍然是同一个容器。您将需要删除并重新创建一个容器来更改任何其他内容。

您可以克隆，它将预先填充使用来自现有容器的所有设置在 **Add Container**页面上。如果您忘记了一件事，您可以克隆容器，更改它，然后删除旧容器。

#### 在Rancher中，链接的容器/服务是如何工作的?

在Docker中，链接的容器（使用`docker run` `--link`）显示在`/etc/hosts`与其链接的容器中。在Rancher，我们不编辑`/etc/hosts`。相反，我们运行[内部DNS服务器]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cattle/internal-dns-service)，使链接可以跨主机访问。DNS服务器将使用正确的IP进行响应。

#### 不能从UI执行shell或查看容器的日志。Rancher如何访问容器的shell /日志？

由于代理商可能对公共互联网开放，因此对容器的shell（或日志等）的代理的请求不会被自动信任。来自Rancher Server的请求包括一个JWT（JSON Web Tokcn），并且该JWT由服务器签名，并且可由代理验证其实际来自服务器。其中的一部分包括到期时间，距离发布时间为5分钟。如果它被拦截，这可以防止令牌长时间被使用。如果不使用SSL，这一点尤其重要。

如果运行docker logs -f rancher-agent，并且日志显示有关过期令牌的消息，那么请检查Rancher Server主机和Rancher Agent主机的日期/时间是否同步。

#### 我在哪里可以看到我的服务日志？

在服务详细信息中，我们在名为**Log**的选项卡中提供服务日志。在**日志**选项卡中，它列出与服务相关的所有事件，包括时间戳和API中发生的事件的描述。这些日志会被保留24小时才能被删除。

### 跨主机通信

如果不同主机上的容器无法ping通，那么可能会出现一些常见的情况。

#### 如何检查跨主机通信是否正常？

在**堆栈** - > **基础架构堆栈中**，检查`healthcheck`堆栈的状态。如果堆栈处于活动状态，则交叉主机通信正在运行。

要手动测试，您可以执行任何容器并ping另一个容器的内部IP（即10.42.xx）。基础架构堆栈的容器可能会隐藏在主机页面上。要查看它们，请选中右上角的“显示系统”复选框。

#### 用户界面中主机的IP地址是否正确？

有时，主机的IP将意外地获取为容器桥接的IP而不是实际的主机IP（这些通常`172.17.42.1`或开始`172.17.x.x` ）。如果是这种情况，您需要通过在`docker run`命令中显式设置环境变量`CATTLE_Agent_IP`来重新注册具有正确IP的主机。

```
$ sudo docker run -d -e CATTLE_AGENT_IP=<HOST_IP> --privileged \
    -v /var/run/docker.sock:/var/run/docker.sock \
    rancher/agent:v0.8.2 http://SERVER_IP:8080/v1/scripts/xxxx
```

#### 运行Ubuntu，容器无法相互通信。

如果您已`UFW`启用，您可以禁用`UFW`或更改`/etc/default/ufw`为：

```
DEFAULT_FORWARD_POLICY="ACCEPT"

```

#### Rancher使用的子网已经在我的网络中使用，并禁止受管网络。如何更改子网？

要更改容器网络使用的子网，您需要确保要使用的[网络]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//rancher-services/networking/#subnets)基础结构服务`default_network`在`rancher-compose.yml`文件中具有正确的[子网](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/faqs/troubleshooting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking/#subnets)。

要更改Rancher的IPsec或VXLAN网络驱动程序，您将需要具有更新的基础架构服务的[环境模板]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//environmcnts/#what-is-an-environmcnt-template)。创建新环境模板或编辑现有环境模板时，可以通过单击**编辑配置**来**编辑**网络基础设施服务的**配置**。在编辑屏幕中，您可以输入不同的子网，然后单击**配置**。任何使用更新后的环境模板环境将使用新的子网。编辑现有环境模板将不会更新现有环境中的基础架构服务。

> **注意：**以前通过API更新子网的方法将不再适用，因为Rancher已转移到基础架构服务。

### DNS

### 如何查看我的DNS设置是否正确？

如果您想查看Rancher DNS设置的配置，请访问**Stacks** - > **Infrastructure**。查找`network-services`堆栈并选择`metadata`服务。在`metadata`服务中，exec进入任何指定的容器`network-services-metadata-dns-X`。您可以使用UI并在容器上选择**Execute Shell**。

```
$ cat /etc/rancher-dns/answers.json
```

#### CentOS

##### 为什么我的容器无法连接到网络？

如果您在主机上运行容器（例如docker run -it ubuntu），并且容器不能与互联网或主机外的任何东西通信，那么您可能会遇到网络问题。

CentOS将默认设置/proc/sys/net/ipv4/ip_forward为0，这将禁止Docker容器所有的网络连接。Docker需要将这个值设置为1，但是如果您在CentOS上重新启动网络服务，则其值又会变为0 。

### 负载平衡器

#### 为什么我的负载均衡器卡在`Initializing`>？

负载平衡器自动对其启用[健康检查]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//cattle/health-checks)。如果负载平衡器卡在`initializing`状态，则很可能[主机]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//faqs/troubleshooting/index.md#cross-host-communication)之间的[跨主机通信不起作用]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//faqs/troubleshooting/index.md#cross-host-communication)。

#### 我如何看到我的负载均衡器的配置？

如果要查看负载平衡器的配置，则需要进入特定的负载均衡器容器并查找配置文件。您可以使用UI并在容器上选择**Execute Shell**。

```
$ cat /etc/haproxy/haproxy.cfg
```

该文件将提供负载平衡器的所有配置详细信息。

#### 在哪里可以找到HAProxy的日志？

HAProxy的日志可以在负载平衡器容器内找到。`docker logs`的负载均衡器容器将仅提供与负载平衡器相关的服务的详细信息，但不提供实际的HAProxy日志记录。

```
$ cat /var/log/haproxy

```

### HA

#### Rancher Compose Executor 和Go-Machine-Service不断重新启动。

在HA集中，如果您正在使用代理服务器，Rancher Compose Executor 和 Go-Machine-Service不断重新启动，请确保使用正确的代理协议。

### 认证

#### 打开[访问控制]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//configuration/access-control)，不能再访问Rancher。如何重置Rancher来停用访问控制？

如果您的身份验证出现问题（例如您的GitHub身份验证已损坏），则可能会将其锁定在Rancher中。要重新获得对Rancher的访问权限，您需要关闭数据库中的Access Control。为此，您需要访问运行Rancher Server的计算机。

```
$ docker exec -it < rancher_server_container_ID > mysql
```

> **注：**在`<rancher_server_container_ID>`将具有Rancher 数据库容器。如果您[升级]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//upgrading)并创建了一个Rancher数据容器，则需要使用Rancher数据容器的ID而不是Rancher服务器容器。

访问cattle 数据库。

```
mysql> use cattle;
```

查看`setting`表。

```
mysql> select * from setting;  
```

更新`api.security.cnabled`到`false`并清除该`api.auth.provider.configured`值。此更改将关闭访问控制，任何人都可以使用UI / API访问Rancher服务器。

```
mysql> update setting set value="false" where name="api.security.enabled";
mysql> update setting set value="" where name="api.auth.provider.configured";
```

确认已在`setting`表中进行更改。

```
mysql> select * from setting;  
```

可能需要约1分钟才能在用户界面中关闭身份验证，但您可以刷新网页并访问Rancher，并关闭访问控制。