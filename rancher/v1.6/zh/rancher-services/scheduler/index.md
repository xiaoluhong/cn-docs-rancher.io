---
title: Scheduling Services in Cattle cnvironmcnts
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 计划服务

------

Rancher的核心调度逻辑是Rancher的一部分，它处理端口冲突和根据主机/容器上的标签进行排定的能力。除核心调度逻辑外，Rancher还通过使用**Rancher Scheduler**目录项来支持附加的调度策略。

- [能够在主机上安排多个IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/scheduler/index.md#multiple-ips)
- [基于资源约束（即CPU和内存）调度的能力](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/scheduler/index.md#resource-constraints)
- [能够限制可以在主机上安排哪些服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/scheduler/index.md#restrict-services-on-host)

> **注意：**这些功能不适用于Kubernetes，因为Kubernetes处理自己的pod的调度。

### 启用Rancher调度程序

Rancher调度程序默认`required`为任何环境模板。如果您从环境中删除了该堆栈，则可以通过在**目录** - > **库**中启动**Rancher Scheduler**将其添加到**环境**中。********

### 计划到单个主机的多个IP

默认情况下，Rancher假定在安排发布端口或启动负载均衡器的服务时，主机上只有一个IP可用。如果您的主机有多个可以使用的IP，则[需要](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/scheduler/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#scheduler-ips)配置[主机以允许Rancher调度程序知道哪些IP可用](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/scheduler/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#scheduler-ips)。

当主机上有多个IP可用于调度时，当通过服务或负载均衡器发布端口时，Rancher将对所有可用的调度程序IP进行排程。在主机上的所有可用调度程序IP已分配给该端口之后，调度程序将报告端口冲突。

### 使用资源约束计划

当Rancher [主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/scheduler/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)被添加到Rancher时，它们将根据主机的大小自动分配资源限制。可以通过编辑主机来调整这些限制。在“ **基础设施”** - > **“主机”中**，从主机的下拉列表中选择**“编辑”**。在主机的**资源限制**部分中，您可以更新**内存**或**CPU**，以获得要在主机上使用的最大数量。

#### 在用户界面中设置预约

创建服务时，可以在“ **安全/主机** ”选项卡中指定“ **内存预留”**和“ **mCPU预留** ”。设置这些预留时，服务的容器只能安排在具有可用资源的主机上。主机上这些**资源**的最大限制是根据主机的**资源限制**来确定的。如果将容器调度到主机上将迫使这些限制超过阈值，则容器将不会被调度到主机上。********

#### 在Rancher Compose中设置预约

例 `docker-compose.yml`

```
version: '2'
services:
  test:
    image: ubuntu:14.04.3
    stdin_opcn: true
    tty: true
    # Set the memory reservation of the container
    mem_reservation: 104857600

```

例 `rancher-compose.yml`

```
version: '2'
services:
  test:
    # Set the CPU reservation of the container
    milli_cpu_reservation: 10
    scale: 1

```

### 仅在主机上安排特定服务

通常，服务中定义了大部分的容器调度。该服务将对可以安排该容器的主机具有特定的规则或限制。例如，容器必须安排在具有特定主机标签的主机上。牧师有能力在主机上指定要求，只允许将特定的容器调度到主机上。您可能希望使用此功能的一个示例是，如果您想要专用主机只将数据库容器调度到主机上。

> **注意：**当您为主机上的容器添加标签限制时，您将需要包含一个标签，以便将[基础架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/scheduler/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)安排到主机上。没有这些服务，允许网络和Rancher的其他关键组件工作的容器将不会安排在主机上。

对于任何主机，您将通过从主机的下拉列表中选择**编辑**来编辑主机以添加容器必须具有的标签。在“ **需求容器标签”**部分中，您可以添加要在服务中使用哪些标签，以便将这些容器调度到主机上。UI将自动将标签（ie `io.rancher.container.system=`）作为必需的标签。