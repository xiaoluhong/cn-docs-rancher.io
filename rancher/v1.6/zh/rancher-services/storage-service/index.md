---
title: Persistcnt Storage Service in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 存储服务

------

Rancher提供不同的存储服务，能够将卷暴露于容器。

### 设置存储服务

设置环境[模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#what-is-an-cnvironmcnt-template)时，可以选择要在环境中使用的存储服务。

或者，如果您已经设置了一个环境，则可以从[目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)中选择并启动存储服务。

> **注意：**某些存储服务可能与某些容器编排类型（即Kubernetes）不兼容。环境模板将根据业务流程限制哪些模板兼容，但所有选项都可以从目录中获得。

### 查看存储驱动程序

存储服务启动后，已创建存储驱动程序，并可在**基础设施** - > **存储**中查看。您将可以看到您环境中可用的所有存储驱动程序。存储驱动程序的名称来自堆栈的名称。

在每个存储驱动程序中，将列出运行存储服务的主机。通常，这是环境中的所有主机。还列出了存储驱动程序中的卷及其状态列表。对于每个卷，您可以看到卷的名称（即主机上的卷的名称）和每个卷的安装。对于每个安装，容器名称和容器中的目录路径。

### 卷范围

通过我们的存储服务，我们可以在不同的级别进行范围的量化。目前，只有[Rancher Compose](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/index.md#using-storage-drivers-with-rancher-compose)支持创建不同类型的卷。UI仅创建环境范围的卷。

#### 堆栈范围

使用堆栈范围的卷，引用堆栈中相同卷的服务将共享相同的卷。堆栈外的服务不能使用相同的卷。

在Rancher中，堆栈范围的卷以堆栈名称作为前缀命名，以指示卷的范围为哪个堆栈，后缀为随机数，以确保不会重复。引用卷时，仍然使用原始卷名。例如，如果创建一个名为卷`foo`中`stackA`的UI，并在主机上会，卷名`stackA_foo_<randomNumber>`，而是使用同一个卷在你的服务，你会使用的名称`foo`。

#### 环境范围

使用环境作用域卷，在环境中引用相同卷的服务将共享相同的卷。不同堆栈中的服务可以共享相同的卷。目前，环境范围的卷只能通过UI创建。

### 在UI中使用存储驱动程序

存储服务启动后`active`， [服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)可以开始使用共享存储。在创建服务时，在“ **卷** ”选项卡中，提供**卷**和**卷驱动程序**。

该**卷**将与Docker的语法相同`<volume_name>:</path/in/container>`。Docker卷默认以读写模式进行装载，但您可以通过`:ro`在卷的末尾加入将其设置为只读。

该**卷驱动程序**将存储驱动器，它是叠层的名称的名称。

如果`<volume_name>`存储驱动程序中已经存在，则如果卷满足卷范围要求，则将使用相同的卷。

#### 创建新卷

可以在2个部分中创建卷：

1. 创建服务后，如果存储驱动程序中不存在**“卷** ”选项卡中的**卷**，则会创建一个环境作用域卷。如果卷驱动程序中已经存在卷，则不会创建新的卷。

> **注意：**此选项不适用于Rancher EBS，因为在使用该卷之前必须在卷上定义特定的驱动程序选项。

1. 在**基础设施** - > **存储中**，单击**添加卷**。如果需要，请提供卷和驱动程序选项的名称。此卷将`inactive`直到服务开始使用。

### 使用存储驱动程序与Rancher Compose

存储基础架构服务启动后，您可以开始创建卷。在下面的例子中，我们使用了**Rancher NFS**存储服务。

卷可以被指定为该`volumes`键下的Docker Compose文件的一部分。每个卷可以与同一文件中的一个或多个服务相关联。此功能仅在使用v2格式的Compose文件时可用。

```
版本：' 2 '
服务：
   foo：
     image：busybox 
    stdin_opcn：true 
    volumes：
    - bar：/ var / lib / storage 
volumes：
   bar：
     driver：rancher-nfs
```

#### 堆栈范围

默认情况下，所有卷都在堆栈范围内创建。在Compose文件或堆栈中引用相同卷的服务将共享相同的卷。

从同一个Compose文件启动新的堆栈时，将创建一个新的卷。当堆栈被删除时，其相应的卷被删除。

在上面的例子中，volume `bar`有堆栈范围。

#### 环境范围

要跨堆叠使用卷，您需要使用环境范围的卷。在这种情况下，在使用卷启动服务和堆栈之前，必须在Rancher中创建卷。要使用环境范围的卷，您`external`可以向卷添加选项。

```
版本：' 2 '
服务：
   foo：
     image：busybox 
    stdin_opcn：true 
    volumes：
    - bar：/ var / lib / storage 
volumes：
   bar：
     driver：rancher-nfs 
    external：true
```

如果`bar`在启动此堆栈时在环境级别找不到名称的卷，则会抛出错误。环境范围的卷只能从UI中删除。