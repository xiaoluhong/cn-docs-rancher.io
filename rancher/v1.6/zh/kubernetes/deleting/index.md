---
title: Deleting Kubernetes
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 删除Kubernetes

------

如果您选择从您的环境中删除**Kubernetes**编排，并希望继续使用您的环境和主机，则需要清理主机。

### 清理荚

在删除**Kubernetes** - > **Infrastructure Stacks**中的Kubernetes堆栈之前，需要删除pod。您可以使用kubernetes删除所有的节点并等待。

```
$ kubectl delete node --all

```

### 清理持久性数据

删除Kubernetes基础架构堆栈后，持久数据仍然保留在主机上。

#### 清理主机

对于任何运行**etcd**服务的主机，有几个项目需要通过执行每个主机进行清理：

- 命名卷`etcd`：通过运行删除命名卷`docker volume rm etcd`。
- 默认情况下，[备份](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/deleting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/backups)已启用并存储在`/var/etcd/backups`：通过运行删除备份`rm -r /var/etcd/backups/*`。

#### 清理持久性数据的原因

- 当主机被重新添加到Kubernetes需要运行的不同环境中时。
- 当Kubernetes系统栈被删除并重新启动通过同一组主机上的目录启动Kubernetes。

> **注意：**如果要清理持久性数据，则在删除Kubernetes基础架构堆栈之前，需要先删除该[pod](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/deleting/index.md#cleaning-up-pods)。否则，pod将保留在主机上。

#### 不清理的后果

如果未执行清除，则保存的`etcd`卷的数据将被新的Kubernetes系统堆栈使用。可能会导致以下几个问题：

- 您可能不想重新创建存储在etcd数据中的所有预先存在的Kubernetes应用程序，但是当Kubernetes API服务连接到映射到保存的etcd数据的etcd服务时，它将会发生。
- **重要提示：**每个新的Kubernetes API服务都会从Rancher获取新的证书。这使得以前创建的所有Kubernetes秘密存储在etcd卷中，已过时。这意味着需要与Kubernetes API服务通讯的任何Kubernetes应用程序（即[仪表板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/deleting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/addons/#dashboard)，[掌舵机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/deleting/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/addons/#helm)，堆栈程序和任何需要此类通信的用户应用程序）都将被破坏。解决这种缺乏沟通的唯一方法是使用旧的秘密重新创建用户pod。