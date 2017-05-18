---
title: Using Rancher with SELinux cnabled
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 使用Rancher启用SELinux - RHEL / CcntOS

------

*适用于Rancher 1.6+*

如果您正在RHEL / CcntOS上运行Rancher，并希望启用SELinux，则需要安装其他SELinux模块。

本文档中的步骤是必须的解决方法，直到[模块](https://github.com/projectatomic/container-selinux/pull/33)中[存在](https://github.com/projectatomic/container-selinux/pull/33)的[更改](https://github.com/projectatomic/container-selinux/pull/33)在RHEL和CcntOS中发货。一旦在RHEL和CcntOS中进行了这些更改，则不再需要这些步骤。

这些步骤必须发生在运行Rancher服务器容器以及任何[主机的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/selinux/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)实例上。

### 安装包来构建模块

为了构建附加的SELinux模块，您需要安装该`selinux-policy-devel`软件包。

```bash
$ yum install selinux-policy-devel
```

### 构建模块

创建一个名为`virtpatch.te`具有以下内容的文件。

```
policy_module(virtpatch, 1.0)

gcn_require(`
  type svirt_lxc_net_t;
')

allow svirt_lxc_net_t self:netlink_xfrm_socket create_netlink_socket_perms;
```

构建模块

```
$ make -f /usr/share/selinux/devel/Makefile
```

运行`make`命令后，`virtpatch.pp`如果构建成功，则应创建一个名为的文件。`virtpatch.pp`是编译的SELinux模块。

### 加载模块

SELinux模块构建完成后，加载模块。