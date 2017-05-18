---
title: Kubernetes - Disaster Recovery
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## Kubernetes -  灾难恢复

---

Rancher 在3个不同的主机上运行最多3个etcd的**实例**。如果大多数运行**etcd**的主机失败，请按照下列步骤操作：

1. 在

   Kubernetes

    - > 

   基础设施堆栈中

   ，扩展

   Kubernetes

   堆栈。点击

   etcd

   服务。找到

   ```
   kubernetes-etcd
   ```

   处于

   ```
   running
   ```

   状态的容器。通过使用

   Execute Shell执行

   到容器。在shell中运行

   ```
   etcdctl cluster-health
   ```

   。

   - 如果最后一条输出行读取`cluster is healthy`，那么没有灾难，立即停止。
   - 如果最后一条输出行读取`cluster is unhealthy`，请记下此`kubernetes-etcd`容器。这是你唯一的幸存者。所有其他容器都可以更换，因为您将使用此容器进行放大。

2. 删除`Disconnected`状态下的主机。确认这些主机都没有运行您的唯一幸存者。

3. 执行你的唯一幸存者的容器。在shell中运行`disaster`。容器将自动重新启动，etcd将自动恢复为单节点集群。系统功能将恢复。

4. 添加更多主机，直到您至少拥有以前运行的主机数量为止。我们建议运行至少3台主机，如果您使用[分离的飞机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/disaster-recovery/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/resiliency-planes/#separated-planes)，不要忘记将标签添加`etcd=true`到主机。Etcd将随着主机的添加而**缩减**，并开始运行**etcd**服务。在大多数情况下，一切都会自动愈合。如果新的/死的容器`initializing`在三分钟后被卡住，执行到这些容器并运行`delete`。**在任何情况下，不要delete在您的幸存者身上执行命令。**