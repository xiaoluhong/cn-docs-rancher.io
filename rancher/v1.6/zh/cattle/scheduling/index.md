---
title: Scheduling Services in Cattle Environments
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 计划服务

------

在Rancher中，您可以根据严格和软的亲和力/反关联性规则在特定主机上安排服务。这些规则可以比较主机上的标签或主机上容器上的标签，以确定容器应该安排在哪个主机上。

默认情况下，Rancher将检测主机上的端口冲突，如果端口不可用，则不会将需要端口的容器计划到主机上。

该核心调度逻辑内置于Rancher，但Rancher还支持位于我们的[外部调度程序](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/scheduler)中的[额外的调度能力](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/scheduler)，这是我们[基础设施服务的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)一部分。其他调度能力包括：

- [能够在主机上安排多个IP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/scheduler/#multiple-ips)
- [基于资源约束（即CPU和内存）调度的能力](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/scheduler/#resource-constraints)
- [能够限制可以在主机上安排哪些服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/scheduler/#restrict-services-on-host)

### 标签和计划规则

通过[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)或通过[负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)启动容器[时](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)，我们提供了创建容器标签的选项，并且可以安排要容纳该容器的主机。对于本节的剩余部分，我们将使用术语服务，但这些标签也适用于负载平衡器（即特定类型的服务）。

调度规则提供了如何让Rancher选择要使用哪个主机的灵活性。在Rancher中，我们使用标签帮助定义调度规则。您可以根据需要在容器上创建任意数量的标签。使用多个调度规则，您可以完全控制要在其上创建容器的主机。您可以要求容器在具有特定主机标签，容器标签或名称或特定服务的主机上启动。这些调度规则可以帮助您创建用于托管关系的容器的黑名单和白名单。

### 在UI中添加标签

对于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)，可以在**标签**选项卡中添加**标签**。对于负载平衡器，可以在“ **标签** ”选项卡中找到**标签**。

通过向[负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)添加标签，每个负载平衡器容器将接收该标签，该标签是一个关键值对。在Rancher中，我们使用这些容器标签来帮助定义调度规则。您可以根据需要在负载平衡器上创建任意数量的标签。默认情况下，Rancher已经在每个容器上添加了系统相关的标签。

### 在UI中调度选项

对于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)和[负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)，可以在“ **计划** ”选项卡中找到标签。

对于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)，有两个选项用于确定启动容器的位置。

#### 选项1：运行特定主机上的*所有*容器

通过选择此选项，容器/服务将在特定主机上启动。如果您的主机掉线，则容器也将下降。如果从容器页面创建容器，即使端口冲突，容器也将启动。如果创建一个比例大于1且服务端口冲突的服务，则您的服务可能会停留在*激活*状态，直到您编辑服务的比例值。

#### 选项2：自动选择主机匹配的调度规则

通过选择此选项，您可以灵活地选择调度规则。遵循所有规则的任何主机都是可以启动容器的主机。您可以通过点击**+**按钮添加规则。

对于[负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)，由于端口冲突，只有选项2可用。您只能添加调度规则。单击**计划**选项卡。您可以通过单击**添加计划规则**按钮**添加任意数量的调度规则**。

对于每个规则，您选择一个**条件**规则的。有四种不同的条件，它们定义了遵守规则的严格程度。该**字段**确定要应用规则的字段。该**键**和**值**是要对要检查的字段中的值。如果您启动了一个服务或负载平衡器，Rancher将根据每个主机的负载将容器的分发扩展到适用的主机上。根据所选择的条件将确定适用的主机是什么。

> **注意：**对于[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services) / [负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)，如果您选择了**在每个主机上始终运行此容器的一个实例以**进行缩放，那么只有主机标签将显示为可能的字段。

#### 条件

- **必须**或**不能**：牧场主只会使用匹配或不匹配字段和值的主机。如果Rancher找不到符合这些条件的所有规则的主机，您的服务可能会停留在*激活*状态。该服务将不断尝试找到容器的主机。要修复此状态，您可以编辑服务的比例值或添加/编辑将满足所有这些规则的其他主机。
- **应该**或**不应该**：Rancher将尝试使用匹配字段和值的主机。在没有匹配所有规则的主机的情况下，Rancher将逐个删除软约束（应该/不应该规则），直到主机满足剩余约束。

#### 字段

- **主机标签**：选择要用于容器/服务的主机时，Rancher将检查主机上的标签，看它们是否与提供的键/值对匹配。由于每个主机都可以有一个或多个标签，所以Rancher将比较主机上的所有标签的键/值对。向主机添加主机时，可以向主机添加标签。您还可以使用主机下拉菜单中的**“编辑”**选项来**编辑**主机上的标签。活动主机上的标签列表可从关键字段的下拉列表中找到。
- **带标签的容器**：选择此字段时，Rancher将查找已经具有与键/值对匹配的标签的容器的主机。由于每个容器都可以有一个或多个标签，所以Rancher会将键/值对与主机中每个容器上的所有标签进行比较。容器标签位于容器的“ **标签** ”选项卡中。在容器启动后，您将无法编辑容器标签。为了创建具有相同设置的新容器，您可以**克隆**容器/服务，并在启动之前添加标签。运行容器上的用户标签列表可从关键字段的下拉列表中找到。
- **服务名称**：Rancher将检查主机是否具有从其上运行的指定服务的容器。如果在稍后的时间，该服务名称更改或不活动/已删除，该规则将不再有效。如果您选择此字段，则该值将需要格式为`stack_name/service_name`。运行服务的列表可从值字段的下拉列表中获得。
- **容器的名称为**：Rancher将检查主机是否具有运行特定名称的容器。如果稍后时间，容器有名称更改或不活动/已删除，该规则将不再有效。运行容器的列表可从值字段的下拉列表中获得。

### 在Rancher Compose中添加标签

Rancher根据文件中`labels`定义的方法确定如何安排服务的容器`docker-compose.yml`。所有带有调度的标签都将用于`docker-compose.yml`文件中。Rancher通过3个主要组件定义了调度规则：条件，字段和值。条件决定了牧场主遵守规则的严格程度。字段是要与哪些项目进行比较。价值观是您在字段上定义的。在介绍一些例子之前，我们会广泛讨论这些组件。

#### 计划条件

当我们编写我们的调度规则时，我们对每个规则都有条件，这说明了Rancher如何使用规则。亲和条件是当我们试图找到一个符合我们的价值的字段时。反兴奋剂条件是当我们试图找到一个不符合我们价值的字段时。

为了区分亲和力和抗亲和力，我们添加`_ne`到标签名称以指示标签**不**匹配字段和值。

规则也有硬而软的条件。

一个艰难的条件相当于说**必须**或**绝对不会**。Rancher只会使用匹配或不匹配字段和值的主机。如果Rancher找不到符合这些条件的所有规则的主机，您的服务可能会停留在*激活*状态。该服务将不断尝试找到容器的主机。要修复此状态，您可以编辑服务的比例值或添加/编辑将满足所有这些规则的其他主机。

软的条件相当于说**应该**或**不应该**。Rancher将尝试使用与该字段和值相匹配的主机。在没有匹配所有规则的主机的情况下，Rancher将逐个删除软约束（应该/不应该规则），直到主机满足剩余的约束。

为了区分*必须*和*应该的*条件，我们添加`_soft`到我们的标签名称以指示标签**应该**尝试匹配字段和值。

#### 字段

Rancher能够比较与主机标签，容器标签，容器名称或服务名称的值。标签前缀是Rancher用来定义哪个字段将被评估的用法。

| 领域         | 标签前缀                                     |
| ---------- | ---------------------------------------- |
| 主机标签       | `io.rancher.scheduler.affinity:host_label` |
| 集装箱标签/服务名称 | `io.rancher.scheduler.affinity:container_label` |
| 集装箱名称      | `io.rancher.scheduler.affinity:container` |

请注意，服务名称中没有特定的前缀。当Rancher创建一个服务时，会将系统标签添加到服务的所有容器中，以指示堆栈和服务名称。

要创建我们的标签的关键字，我们从字段前缀（例如`io.rancher.scheduler.affinity:host_label`）开始，并根据我们正在寻找的条件，我们附加我们想要的条件类型。例如，如果我们希望容器在不能等于（即`_ne`）到主机标签值的主机上启动，则标签键将是`io.rancher.scheduler.affinity:host_label_ne`。

#### 值

您可以使用这些值来定义要检查的字段。如果您有两个值要针对相同的条件和字段进行比较，那么您只需要使用一个标签作为标签的名称。对于标签的值，您需要使用逗号分隔的列表。如果有多个具有相同键（例如`io.rancher.scheduler.affinity:host_label_ne`）的标签，Rancher将使用与标签键一起使用的最后一个值覆盖任何先前的值。

```
标签：
   io.rancher.scheduler.affinity：host_label：key1 = value1，key2 = value2
```

#### 全球服务

将服务置于全局服务中相当于在UI中的**每个主机上**选择“ **始终运行此容器的一个实例”**。这意味着将在环境中的任何主机上启动一个[容器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)。如果将新主机添加到环境中，并且主机满足全球服务的主机要求，则服务将自动启动。

目前，我们只支持使用硬盘条件的主机标签字段的全球服务。这意味着只有`host_labels`在调度时才会遵守与之相关的标签，它**必须**或**不能**等于值。任何其他标签类型都将被忽略。

##### 例 `docker-compose.yml`

```
版本：“ 2 ”
服务：
   WordPress的：
     标签：
       ＃制作WordPress的全球服务
      io.rancher.scheduler.global：“真” 
      ＃使WordPress的只有一个键1 =值标签的主机上运行的容器
      io.rancher.scheduler.affinity： host_label：key1 = value1 
      ＃使wordpress仅在没有key2 = value2标签的主机上运行
      io.rancher.scheduler.affinity：host_label_ne：key2 = value2 
    image：wordpress 
    links：
    - db：mysql 
    stdin_open：true
```

#### 使用主机标签查找主机

将主机添加到Rancher时，可以添加[主机标签](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/scheduling/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)。在安排服务时，您可以利用这些标签创建规则来选择要部署服务的主机。

##### 使用主机标签的示例

```
标签：
   ＃主机必须有标签`key1 = value1` 
  io.rancher.scheduler.affinity：host_label：key1 = value1 
  ＃主机不能有标签`key2 = value2` 
  io.rancher.scheduler.affinity：host_label_ne：key2 = value2 
  ＃主机应该有标签`key3 = value3` 
  io.rancher.scheduler.affinity：host_label_soft：key3 = value3 
  ＃主机应该没有标签`key4 = value4` 
  io.rancher.scheduler.affinity：host_label_soft_ne：key4 = value4
```

##### 自动应用主机标签

Rancher自动创建与主机的linux内核版本和Docker Engine版本相关的主机标签。

| 键                                      | 值                               | 描述                  |
| -------------------------------------- | ------------------------------- | ------------------- |
| `io.rancher.host.linux_kernel_version` | 主机上的Linux内核版本（例如，`3.19`）        | 在主机上运行的Linux内核的版本   |
| `io.rancher.host.docker_version`       | 主机上的Docker Engine版本（例如`1.10.3`） | Docker Engine版本在主机上 |
| `io.rancher.host.provider`             | 云提供商信息                          | 云提供商名称（目前仅适用于AWS）   |
| `io.rancher.host.region`               | 云提供商区域                          | 云供应商区域（目前仅适用于AWS）   |
| `io.rancher.host.zone`                 | 云提供商区域                          | 云提供商区域（目前仅适用于AWS）   |

```
标签：
 ＃主机必须运行Docker版本1.10.3 
io.rancher.scheduler.affinity：host_label：io.rancher.host.docker_version = 1.10.3 
＃主机必须不运行Docker 1.6版
io.rancher.scheduler.affinity： host_label_ne：io.rancher.host.docker_version = 1.6
```

> **注意：** Rancher不支持在具有`>=`特定版本的主机上调度容器的概念。您可以使用主机调度规则来创建特定的白名单和黑名单，以确定您的服务是否需要特定版本的Docker Engine。

#### 用容器标签查找主机

向Rancher添加容器或服务时，可以添加容器标签。这些标签可以用于您希望规则进行比较的字段。提醒：如果将全局服务设置为true，则无法使用。

> **注意：**如果容器标签有多个值，则Rancher将查看主机上所有容器上的所有标签，以检查容器标签。多个值不需要在主机上的同一容器上。

##### 使用容器标签的示例

```
标签：
   ＃主机必须有一个容器，标签为`key1 = value1` 
  io.rancher.scheduler.affinity：container_label：key1 = value1 
  ＃主机不能有一个标签为`key2 = value2` 
  io.rancher.scheduler的容器。亲和力：container_label_ne：key2 = value2 
  ＃主机应该有一个容器，标签为`key3 = value3` 
  io.rancher.scheduler.affinity：container_label_soft：key3 = value3 
  ＃主机应该没有一个标签为`key4 = value4 
  io 的容器。 rancher.scheduler.affinity：container_label_soft_ne：key4 = value4
```

##### 服务名称

当Rancher Compose启动服务的容器时，它也会自动创建多个容器标签。因为检查一个特定的容器标签正在寻找一个`key=value`，我们可以使用这些系统标签作为我们规则的关键。以下是当Rancher启动服务时在容器上创建的系统标签：

| 标签                            | 值                                 |
| ----------------------------- | --------------------------------- |
| io.rancher.stack.name         | `$${stack_name}`                  |
| io.rancher.stack_service.name | `$${stack_name}/$${service_name}` |

> **注意：**使用时`io.rancher.stack_service.name`，该值必须为格式`stack name/service name`。

宏`$${stack_name}`并且`$${service_name}`还可以在使用中的`docker-compose.yml`文件中的任何其他`label`并在服务启动时会进行评估。

##### 使用服务名称的示例

```
标签：
   ＃主机必须有一个服务名称为“value1”的容器
  io.rancher.scheduler.affinity：container_label：io.rancher.stack_service.name = stackname / servicename
```

#### 查找具有容器名称的主机

向Rancher添加容器时，可以给每个容器一个名称。您可以使用此名称作为要与规则进行比较的字段。提醒：如果将全局服务设置为true，则无法使用。

##### 使用容器名称的示例

```
标签：
   ＃主机必须有一个名称为“ 
  value1 ”的容器io.rancher.scheduler.affinity：container：value1 
  ＃主机不能有一个名称为“ 
  value2 ”的容器io.rancher.scheduler.affinity：container_ne：value2 
  ＃主机应该有一个名为`value3`的容器
  io.rancher.scheduler.affinity：container_soft：value3 
  ＃主机应该没有一个名为`value4 
  io.rancher.scheduler.affinity：container_soft_ne：value4的容器
```

### 例子

#### 示例1：

典型的调度策略可能是尝试在不同的可用主机之间传播服务的容器。实现这一点的一个方法是使用反相关性规则来实现：

```
标签：
   io.rancher.scheduler.affinity：container_label_ne：io.rancher.stack_service.name = $$ {stack_name} / $$ {service_name}
```

由于这是一个很强的反相关性规则，如果比例大于可用主机数量，我们可能遇到问题。在这种情况下，我们可能需要使用软反相关性规则，以便调度程序仍然允许将容器部署到已经具有该容器运行的主机。基本上，这是一个软规则，所以如果没有更好的选择存在，它可以被忽略。

```
标签：
   io.rancher.scheduler.affinity：container_label_soft_ne：io.rancher.stack_service.name = $$ {stack_name} / $$ {service_name}
```

#### 示例2：

另一个例子可能是将所有容器部署在同一主机上，而不考虑哪个主机。在这种情况下，可以使用对其自身的软亲和力。

```
标签：
   io.rancher.scheduler.affinity：container_label_soft：io.rancher.stack_service.name = $$ {stack_name} / $$ {service_name}
```

如果选择了自己的硬约束规则，则第一个容器的部署将失败，因为目前没有该服务正在运行的主机。

### 计划标签表

| 标签                                       | 值                               | 描述                                       |
| ---------------------------------------- | ------------------------------- | ---------------------------------------- |
| io.rancher.scheduler.global              | 真正                              | 将此服务指定为全局服务                              |
| io.rancher.scheduler.affinity：host_label | key1 = value1，key2 = value2等... | 容器**必须**被部署到与标签主机`key1=value1`和`key2=value2` |
| io.rancher.scheduler.affinity：host_label_soft | 键1 =值，键2 =值                     | 容器**应**被部署到与标签主机`key1=value1`和`key2=value2` |
| io.rancher.scheduler.affinity：host_label_ne | 键1 =值，键2 =值                     | 容器**必须不**被部署到一个主机与标签`key1=value1`或`key2=value2` |
| io.rancher.scheduler.affinity：host_label_soft_ne | 键1 =值，键2 =值                     | 容器**不应该**被部署到一个主机与标签`key1=value1`或`key2=value2` |
| io.rancher.scheduler.affinity：container_label | 键1 =值，键2 =值                     | 容器**必须**部署到与标签的容器运行在主机`key1=value1`和`key2=value2`。注意：这些标签不必在同一容器上。可以在同一主机内的不同容器上。 |
| io.rancher.scheduler.affinity：container_label_soft | 键1 =值，键2 =值                     | 容器**应**被部署到与标签的容器运行在主机`key1=value1`和`key2=value2` |
| io.rancher.scheduler.affinity：container_label_ne | 键1 =值，键2 =值                     | 容器**必须不**被部署到具有与标签的容器运行在主机`key1=value1`或`key2=value2` |
| io.rancher.scheduler.affinity：container_label_soft_ne | 键1 =值，键2 =值                     | 容器**不应该**被部署到具有与标签的容器运行在主机`key1=value1`或`key2=value2` |
| io.rancher.scheduler.affinity：容器         | container_name1，container_name2 | 容器**必须**部署到具有名称`container_name1`并`container_name2`运行的容器的主机上 |
| io.rancher.scheduler.affinity：container_soft | container_name1，container_name2 | 容器**应**被部署到与名称容器主机`container_name1`和`container_name2`运行 |
| io.rancher.scheduler.affinity：container_ne | container_name1，container_name2 | 容器**不能**部署到具有名称`container_name1`或`container_name2`正在运行的容器的主机上 |
| io.rancher.scheduler.affinity：container_soft_ne | container_name1，container_name2 | 容器**不应**部署到具有名称`container_name1`或`container_name2`正在运行的容器的主机上 |

