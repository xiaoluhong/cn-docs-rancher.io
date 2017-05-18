---
title: Upgading Kubernetes
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 升级Kubernetes

------

如果您正在升级现有的Kubernetes设置以[要求平面隔离](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/resiliency-planes)，请阅读有关迁移过程的更多信息。

迁移过程分两个阶段进行。

1. 确保您拥有足够的主机以及相应的标签。
2. 升级Kubernetes基础设施堆栈。

### 带标签的主机

确认您的环境具有足够的主机与飞机的标签。您可以添加新的主机或使用现有的主机。

1. **计算平面主机：** *对于运行kubelet和代理容器的所有现有主机*，请添加标签`compute=true`。这些是运行时已经向Kubernetes注册的节点`kubectl get node`。此步骤**至关重要**，因为在此升级过程中，如果没有标签，Kubernetes pod将在主机上成为孤儿。如果您有运行kubelet和代理容器的主机，则可以按照[从计算平面](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/upgrading/index.md#removing-hosts-with-pods-from-the-compute-plane)中[删除它们](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/upgrading/index.md#removing-hosts-with-pods-from-the-compute-plane)的步骤。您还可以添加更多的主机和[标记这些主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)使用`compute=true`。
2. **数据平面主机：**拥有3个或更多主机，1个CPU，> = 1.5GB RAM，> = 20GB磁盘。**如果您现有的Kubernetes设置与etcd服务正在运行，请确保etcd=true标签位于这些主机上。**
3. **编排平面主机：**添加2个或更多主机，其中> = 1 CPU和> = 2GB RAM。当添加主机，[标注这些主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)使用`orchestration=true`。你可以摆脱1个主机，但你牺牲高可用性。在主机发生故障的情况下，一些K8s功能，如API，在发生故障时重新安排的端口等，直到新的主机被配置才会发生。

### 升级Kubernetes

1. 在**Kubernetes** - > **基础设施堆栈中**，找到Kubernetes基础架构堆栈。单击**升级可用**或**最新日期** 按钮。
2. 确认Kubernetes的模板版本是您要使用的。
3. 选择`required`的**平面隔离**。
4. 选择[云提供商](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/providers)，[备份](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/backups)，[附加组件](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/addons)的配置选项。
5. 单击**升级**以启动更新的Kubernetes基础架构堆栈。
6. Kubernetes堆栈完成升级所有服务并处于`upgraded`状态后，单击“ **升级：完成升级”**。

#### 从计算平面删除具有pod的主机

**警告：**如果您打算删除从计算平面上的任何主机，裸荚不是复制控制器的一部分或类似的将*不*被重新安排。这是正常的行为。

计算平面中的主机正在运行kubelet和代理容器。

1. 如果主机有`compute=true`标签，请从主机中删除标签。这将阻止在删除这些容器之后，将kubelet和代理容器重新调度到主机上。
2. 使用`kubectl`通过远程CLI或外壳，运行`kubectl delete node <HOST>`。您可以`<HOST>`通过运行从Rancher UI或kubectl 找到主机名（ie ）`kubectl get node`。请等待所有的pods被删除，然后再转到下一个可选步骤。
3. **可选：**如果要从主机中删除kubelet和代理，可以对kubelet服务和代理服务执行升级，而无需更改任何设置。在运行此升级之前，请确保在主机上删除所有pod。