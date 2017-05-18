---
title: Kubernetes - Backups
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## Kubernetes - 备份

------

默认情况下，备份在Kubernetes中启用。对于备份，应考虑网络存储延迟和大小。对于任何单个备份，50MB是存储要求的良好估计。例如，每15分钟创建并保留1天的备份将最多存储96个备份，需要〜5 GB的存储空间。如果无意恢复到以前的时间点，则可能会保留更少的历史备份。

### Kubernetes配置

当[配置Kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/backups/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#configuring-kubernetes)，您可以选择备份是否应该启用。

如果启用备份，则可以指示“ **备份创建周期”**和“ **备份保留期”**的持续时间。

备份时段持续时间必须是十进制数，每个具有可选的馏分和一个单元后缀，如序列`300ms`，`1.5h`或`2h45m`。有效的时间单位`ns`，`us`或`µs`，`ms`，`s`，`m`和`h`。

该**备份创建周期**持续时间指示在什么速度备份应该创建。建议不要建立备份`30s`。

该**备份保留期**持续时间表示在应该删除哪些率的历史备份。保留期外的备份在下次成功备份后过期。

在任何给定时刻存储在磁盘上的最大备份数都遵循公式`ceiling(retcntion period / creation period)`。例如，`5m`具有`4h`保留期限的创建期间最多将存储`ceiling(4h / 5m)`备份或`48`备份。备份大小的保守估计是`50MB`，所以连接的网络存储应至少具有`2.4GB`可用空间。备份大小将根据用途而有所不同。

如果禁用**备份，则将**忽略“ **备份创建周期”**和“ **备份保留期”的**值。

### 配置远程备份

目前，备份将持续到主机上的静态位置`/var/etcd/backups`。需要在运行**该等**服务的所有主机上的该位置安装网络存储。网络存储设置必须在Kubernetes启动之前完成。

### 还原备份

如果运行**etcd**服务的所有主机都失败，请按照下列步骤操作：

1. 通过从**Kubernetes** - > **Infrastructures Stacks**中删除**Kubernetes**堆栈，将您的环境编排类型更改为**Cattle**。荚将保持完整和可用。************

2. 删除`disconnected`主机并添加新主机。如果您选择了[弹性飞机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/backups/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/resilicncy-planes)，则需要添加标签的主机`etcd=true`。

3. 对于将要运行**etcd**服务的每个主机，装载包含备份的网络存储，这应该是作为[配置远程备份的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/backups/index.md#configuring-remote-backups)一部分而创建的。然后运行以下命令：

   ```
   ＃将其配置为/ var / etc / backups中所需的备份 
   target = < NAME_OF_BACKUP > 
   ＃请勿触摸此行下方的任何内容
   码头容量rm等
   docker卷创建--name etcd
   docker运行-d -v etcd：/ data -name etcd-restore busybox
   docker cp / var / etcd / backups / $ target etcd-restore：/data/data.currcnt
   docker rm etcd-restore
   ```

   > **注意：**您必须以具有对远程备份的读取权限的用户身份登录。否则，`docker cp`命令将静默失败。

4. 通过目录启动Kubernetes。确保[配置Kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/backups/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#configuring-kubernetes)。该**Kubernetes**基础设施堆栈将启动并您的豆荚将进行协调。您的备份可能会反映与当前存在的不同的部署拓扑。**荚可能被删除/重新创建。**