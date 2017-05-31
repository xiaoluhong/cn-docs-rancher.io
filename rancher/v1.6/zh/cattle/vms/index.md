---
title: Virtual Machines
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 虚拟机入门

------

默认情况下，禁用在Rancher中启动虚拟机（VM）。要启用启动VM，任何[管理员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/vms/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#admin)都可以在**管理** - > **设置** - > **虚拟机**部分中启用Rancher中的**虚拟机**。选择**启用**，然后单击**保存**。

### 主机启动VM的要求

- Ubuntu的

- RancherOS - 需要`userland-proxy`启用以下cloud-config

  ```
  #cloud-config
  rancher:
    docker:
      extra_args: [--userland-proxy=true]                                                                                                   

  ```

### 添加虚拟机

在**基础架构** - > **虚拟机中**，单击**添加虚拟机**并设置虚拟机。

对于图像名称选项，目前Rancher仅支持`rancher/vm-ubuntu`。用户名/密码是`ubuntu/ubuntu`。

单击**创建**。

容器启动后，单击**打开控制台**，单击容器的下拉列表以登录到虚拟机。