---
title: Labels
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 标签

------

Rancher在服务/容器和主机上使用标签来帮助管理Rancher的不同功能。

### 牧场主撰写服务标签

标签用于帮助Rancher启动服务并利用Rancher的功能。这个标签索引用于帮助用户使用Rancher Compose创建服务。应该使用这些标签的UI等价物，而不是向服务添加标签。

| 键                                        | 值                                        | 描述                                       |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| `io.rancher.sidekicks`                   | 服务名称                                     | 用于定义哪些服务是[配对](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services/#sidekick-services) |
| `io.rancher.loadbalancer.target.SERVICE_NAME` | `REQUEST_HOST:SOURCE_PORT/REQUEST_PATH=TARGET_PORT` | 用于确定[L7负载平衡](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers/#advanced-load-balancing-l7) |
| `io.rancher.container.dns`               | `true`                                   | 服务能够使用基于Rancher DNS的服务发现来解决其他服务，并由其他服务解决。如果网络设置为需要DNS，则需要此标签`host`。 |
| `io.rancher.container.hostname_override` | `container_name`                         | 用于将容器的主机名设置为容器的名称（例如StackName_ServiceName_CreateIndex） |
| `io.rancher.container.start_once`        | `true`                                   | 用于运行容器一次，并在服务保持状态时保持停止`active`状态         |
| `io.rancher.container.pull_image`        | `always`                                 | 用于在部署容器之前始终拉一张新图像。                       |
| `io.rancher.container.requested_ip`      | 来自10.42.0.0/16地址空间的IP                    | 允许您选择容器的特定IP。注意：如果IP在主机上不可用，则容器将以随机IP开始。 |
| `io.rancher.container.dns.priority`      | `service_last`                           | 在服务域之前使用主机的DNS搜索路径。这将`*.rancher.internal`在主机搜索后进行搜索`/etc/resolv.conf`。 |
| `io.rancher.service.selector.container`  | [*选择器标签*值](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/labels/#selector-labels) | 用于服务，以便可以选择独立的容器来加入服务DNS。注意：作为独立容器，任何服务操作都不会影响独立容器（即停用/删除/编辑服务，健康检查等）。 |
| `io.rancher.service.selector.link`       | [*选择器标签*值](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/labels/#selector-labels) | 用于服务以允许服务根据服务标签链接到服务。示例：Service1有一个标签`io.rancher.service.selector.link: foo=bar`。任何添加到Rancher的具有`foo=bar`标签的服务将自动链接到Service1。 |
| `io.rancher.scheduler.global`            | `true`                                   | 用于设置[全局服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling/#global-service) |
| `io.rancher.scheduler.affinity:host_label` | 主机标签的主要价值对                               | 用于根据[主机标签](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling/#finding-hosts-with-host-labels)在主机上安排容器 |
| `io.rancher.scheduler.affinity:container_label` | 任何容器标签的关键值对                              | 用于根据[容器标签或服务名称](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling/#finding-hosts-with-container-labels)在主机上安排容器 |
| `io.rancher.scheduler.affinity:container` | 集装箱名称                                    | 用于根据[容器名称](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling/#finding-hosts-with-container-names)在主机上安排容器 |
| `io.rancher.lb_service.target`           | [*目标服务标签*值](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/index.md#target-service-labels) | 用于配置负载平衡器，以便流量将路由到与负载平衡器位于同一主机上的容器。      |

> **注意：**对于前缀的标签`io.rancher.scheduler.affinity`，根据您想要匹配的方式（即相等或不相等，硬或软规则）有轻微的变化。更多细节可以在[这里](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling/#table-of-scheduling-labels)找到。

#### 选择器标签

使用*选择器标签*（即`io.rancher.service.selector.link`，`io.rancher.service.selector.container`），Rancher可以通过标签识别服务/容器，并将它们自动链接到服务。*选择器标签*在两种情况下进行评估。情景1是将*选择标签*添加到服务中。在方案1中，对所有现有标签进行评估，以查看它们是否与*选择器标签*相匹配。场景2是服务已经有*选择器标签的时候*。在方案2中，检查添加到Rancher的任何新服务/容器，查看是否有资格链接。甲*选择器标签*可以由多种需求，这是逗号分隔。如果有多重要求，****

```
# One of the container labels must have a key equal to `foo1` and value equal to `bar1`
foo1 = bar1
# One of the container labels must have a key equal to `foo2` with a value not equal to `bar2`
foo2 != bar2
# One of the container labels must have a key equal to `foo3`, value of the label does not matter
foo3
# One of the container labels must have a key equal to `foo4` and value equal to `bar1` OR `bar2`
foo4 in (bar1, bar2)
# One of the container labels must have a key equal to `foo5` and value other than `bar3` OR `bar4`
foo5 notin (bar3, bar4)

```

> **注意：**如果有一个包含逗号的标签，则选择器将无法与标签相匹配，因为*选择器标签*可以在任何没有关联值的键上匹配。示例：`io.rancher.service.selector.link: foo=bar1,bar2`将转换为具有一个标签的任何服务的标签必须是键等于`foo`和等于`bar1` **AND与**其他键相同的另一个标签`bar2`。它不会拿起一个具有等于`foo`和等于的键的标签的服务`bar1,bar2`。

#### 逗号分隔列表的示例

```
service1:
  labels:
    # Selector label added to pick up other services
    io.rancher.service.selector.link: hello != world, hello1 in (world1, world2), foo = bar

```

在此示例中，将链接的服务`service1`需要满足以下所有条件：

- 密钥等于`hello`和值不等于的标签`world`
- 密钥等于的标签`hello1`可以具有等于`world1`或等于的值`world2`
- 密钥等于`foo`和等于的标签`bar`

以下示例，部署时`service2`会自动链接到`service1`。

```
service2:
   labels:
      hello: test
      hello1: world2
      foo: bar

```

### 服务上的系统标签

除了Rancher Compose可以使用的标签之外，Rancher在启动服务时还会创建一系列系统标签。

| 键                                        | 描述              |
| ---------------------------------------- | --------------- |
| `io.rancher.stack.name`/`io.rancher.project.name` | 根据堆栈名称创建        |
| `io.rancher.stack_service.name`/`io.rancher.project_service.name` | 根据服务名称创建        |
| `io.rancher.service.deployment.unit`     | 根据部署的配对服务创建     |
| `io.rancher.service.launch.config`       | 根据配对服务创建。       |
| `io.rancher.service.requested.host.id`   | 根据该服务安排在哪个主机上创建 |

### 主机标签

可以在主机注册期间将[主机标签](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)添加到主机，稍后通过**编辑**主机添加[主机标签](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)。

| 键                                 | 值                                        | 描述                                       |
| --------------------------------- | ---------------------------------------- | ---------------------------------------- |
| `io.rancher.host.external_dns_ip` | IP用于[外部DNS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service)，egabcd | 用于外部DNS服务，并要求[使用主机IP以外的IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)对DNS记录[进行](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)编程 |

### 自动应用主机标签

Rancher自动创建与主机的linux内核版本和Docker Engine版本相关的主机标签。这些标签可用于[调度](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/labels/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)。

| 键                                      | 值                        | 描述                  |
| -------------------------------------- | ------------------------ | ------------------- |
| `io.rancher.host.linux_kernel_version` | 主机上的Linux内核版本（例如，`3.19`） | 在主机上运行的Linux内核的版本   |
| `io.rancher.host.docker_version`       | 主机上的Docker版本（例如`1.10.3`） | Docker Engine版本在主机上 |
| `io.rancher.host.provider`             | 云提供商信息                   | 云提供商名称（目前仅适用于AWS）   |
| `io.rancher.host.region`               | 云提供商区域                   | 云供应商区域（目前仅适用于AWS）   |
| `io.rancher.host.zone`                 | 云提供商区域                   | 云提供商区域（目前仅适用于AWS）   |

### 本机Docker标签

`io.rancher.container.network`| `true`| 将此标签添加到`docker run`命令中以将Rancher网络添加到容器

#### 目标服务标签

负载平衡器可以配置为将流量优先于位于负载均衡器的同一主机上的目标服务的容器。根据标签的值，负载平衡器将被配置为将流量仅引导到那些容器，或者将流量优先于这些容器。默认情况下，负载平衡器以循环算法将流量路由到目标服务的所有容器。

| 键                              | 值              | 描述                                       |
| ------------------------------ | -------------- | ---------------------------------------- |
| `io.rancher.lb_service.target` | `only-local`   | 只能直接流量到负载平衡器容器在同一主机上的容器。如果同一主机上没有目标服务的容器，则不会将流量路由到该服务。 |
| `io.rancher.lb_service.target` | `prefer-local` | 将流量与负载均衡器容器在同一主机上的容器进行优先级排序。如果在同一主机上没有目标服务的容器，则流量将被路由到驻留在其他主机上的服务的其他容器。 |

