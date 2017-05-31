---
title: Kubernetes - Resilicncy Planes
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## Kubernetes - Resilicncy Planes

---

对于生产部署，最佳实践是每个平面都运行在专用物理或虚拟主机上。为了开发，可以使用多租户来简化管理并降低成本。

#### Data Plane

This plane由一个或多个**etcd**容器组成。Etcd是一个分布式可靠的键值存储，存储所有Kubernetes状态。该平面可以被称为有状态，意味着包括平面的软件维护应用状态。

#### Orchestration Plane

This plane 由无状态的部件组成，它为我们的Kubernetes的分布提供动力。

#### Compute Plane

This plane is comprised of the Kubernetes [pods](https://kubernetes.io/docs/user-guide/pods/).

### 规划

在安装之前，请考虑您的具体用例很重要。Rancher提供两种不同的部署类型。

如果您正在寻找一种快速启动Kubernetes来开始测试Kubernetes的方法，我们建议您使用[Overlapping Planes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//kubernetes/resilicncy-planes/index.md#overlapping-planes)启动[Kubernetes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/resilicncy-planes/index.md#overlapping-planes)。默认情况下，这是默认的Kubernetes模板中的设置。

对于生产环境，Rancher建议使用[Separated Planes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/resilicncy-planes/index.md#separated-planes)运行Kubernetes 。

### 安装

#### Overlapping Planes

默认情况下，Kubernetes被配置为在重叠平面上部署。所有飞机重叠，所有服务都可以在单个主机上运行。服务随机安排。添加至少三个主机以使数据平面（即etcd）具有弹性。

1. 创建Kubernetes环境。
2. 添加至少1个CPU，2GB RAM的1个或多个主机。资源需求因工作负载而异。

#### Separated Planes

此部署允许用户通过专用于每个平面类型的特定主机来分离平面。它提供数据平面弹性和计算平面性能保证。添加主机之前，需要[配置Kubernetes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/#configuring-kubernetes)。配置Kubernetes时，选择`required`“ **平面隔离”**选项。

> **注意：**如果您将Kubernetes从重叠平面升级到分离的飞机，请[阅读有关升级的更多信息]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//kubernetes/upgrading)以正确处理更改。

##### 添加主机标签主机

添加到Kubernetes环境中的所有主机都必须贴上标签，以便Rancher可以根据飞机的类型安排服务。此部署类型至少需要五个主机。

1. **数据平面：**添加3个或更多主机1 CPU，> = 1.5GB RAM，> = 20GB磁盘。当添加主机，[标注这些主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/resilicncy-planes/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)使用`etcd=true`。
2. **编排平面：**添加2个或更多主机，其中> = 1 CPU和> = 2GB RAM。当添加主机，[标注这些主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/resilicncy-planes/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)使用`orchestration=true`。您可以离开1个主机，但是如果发生主机故障，k8s API将不可用，直到添加一个新的业务流程主机。
3. **计算平面：**添加1个或多个主机。当添加主机，[标注这些主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/resilicncy-planes/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)使用`compute=true`。

> **注意：**可以将主机标签添加到现有主机以将其添加到平面中，但是我们不支持通过更改标签将主机从一种平面类型更改为另一种平面类型。为了更改平面类型，您可以删除旧平面标签，并删除主机上的所有现有服务，然后再添加新的平面标签或更新标签，并根据新的平面标签[升级Kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/resilicncy-planes/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/upgrading)以重新平衡容器。