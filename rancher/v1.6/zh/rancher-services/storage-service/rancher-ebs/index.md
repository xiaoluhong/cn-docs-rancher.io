---
title: Using Rancher EBS
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 牧场EBS

------

Rancher可以选择AWS EBS卷作为容器的存储选项。

### 使用EBS时的限制

AWS EBS卷只能附加到单个AWS EC2实例。因此，使用相同AWS EBS卷的所有容器将被安排在同一台主机上。

### 设立牧场EBS

设置环境[模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/rancher-ebs/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#what-is-an-cnvironmcnt-template)时，可以选择**Rancher EBS**目录项目，以便在从该环境模板创建的任何环境中可用。

或者，如果您已经设置了一个环境，您可以从[目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/rancher-ebs/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)中选择并启动Rancher EBS 。

> **注意：**某些存储服务可能与某些容器编排类型（即Kubernetes）不兼容。环境模板将根据业务流程限制哪些模板兼容，但所有选项都可以从目录中获得。

为了启动Rancher EBS，您将需要具有启动AWS EBS卷的有效权限的AWS Access Key和Secret Key。根据您的驱动程序选项，可能需要其他权限。

### Rancher EBS驱动程序选项

创建AWS EBS卷时，有几个可用于自定义卷的选项。这些选项是可以在UI中作为驱动程序选项添加的键值对，或者在`driver_opts`键下的组合文件中。

#### 所需驱动程序选项

- **大小** - （`size`）：EBS音量的大小

#### 可选驱动程序选项

- **卷类型** - （`volumeType`）：**卷的类型**
- **IOPS** - （`iops`）：IOPS选项
- **特定可用区域**（`ec2_az`）：创建容器和EBS卷的具体可用区域。（例如`us-west-1a`）

对于以下选项，**必须**`ec2_az`在驱动程序选项中指定ID与之关联的特定可用性区域（）。

- **cncrypted**（`cncrypted`）：卷是否应加密。注意：使用此选项时需要AWS KMS ID。
- **AWS KMS ID**（`kmsKeyId`）：创建加密卷时要使用的AWS密钥管理服务（AWS KMS）客户主密钥（CMK）的完整ARN。
- **快照ID**（`snapshotID`）：创建卷的快照。
- **卷ID**（`volumeID`）：要使用的现有卷的ID。

### 在UI中使用Rancher EBS

#### 创建卷

在**Rancher**启动**Rancher EBS**之后，在使用服务卷之前，您将需要在**基础设施** - > **存储**中的EBS中创建卷。

1. 单击**添加卷**。
2. 创建将在服务中使用的卷的名称。
3. 必需：添加驱动程序选项`size`。
4. 可选：添加任何其他驱动程序选项。注意：如果使用加密，快照ID或卷ID，还必须指定可用性区域。

#### 在服务中使用卷

在UI中创建卷后，[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/rancher-ebs/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)可以开始使用共享存储。在创建服务时，在“ **卷** ”选项卡中，提供**卷**和**卷驱动程序**。

该**卷**将与Docker的语法相同`<volume_name>:</path/in/container>`。Docker卷默认以读写模式进行装载，但您可以通过`:ro`在卷的末尾加入将其设置为只读。

该**卷驱动程序**将存储驱动器，它是叠层的名称的名称。默认情况下，**Rancher EBS**存储驱动将是`rancher-ebs`。

### 在撰写文件中使用Rancher EBS

在**Rancher EBS**基础架构堆栈启动之后，您可以开始在组合文件中创建卷。

卷可以指定为`volumes`密钥下的撰写文件的一部分。每个卷可以与同一文件中的一个或多个服务相关联。

> **注意：**此功能仅在使用v2格式的撰写文件时可用。

#### 指定大小，卷类型和IOPS的堆栈范围卷的示例

在此示例中，我们正在创建一个[堆栈作用域卷](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/rancher-ebs/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/storage-service/#stack-scoped)，同时创建使用此卷的服务。此堆栈中的所有服务将共享相同的卷。

```
版本：' 2 '
服务：
   foo1：
     image：busybox 
    stdin_opcn：true 
    volumes：
    - bar：/ var / lib / storage 
  foo2： 
     image：busybox 
    volumes：
    - bar：/ var / lib / storage

卷：
   条：
     驱动程序：牧场主-EBS 
    driver_opts：
       尺寸：10 
      volumeType：GP2 
      IOPS：1000
```

#### 安排到特定可用区域的堆栈范围卷的示例

在此示例中，我们正在创建一个[堆栈作用域卷](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/rancher-ebs/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/storage-service/#stack-scoped)，同时创建使用此卷的服务。此堆栈中的所有服务将共享相同的卷。

我们正在指定我们想要创建卷的可用性区域。使用EBS卷的任何服务的容器将自动计划到与EBS卷相连的同一主机上。

```
版本：' 2 '
服务：
   foo：
     image：busybox 
    stdin_opcn：true 
    volumes：
    - bar：/ var / lib / storage

卷：
   bar：
     driver：rancher -ebs 
    driver_opts：
       size：10 
      ec2_az：us-west-2a
```

#### 加密堆栈范围卷的示例

在此示例中，我们正在创建一个[堆栈作用域卷](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/rancher-ebs/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/storage-service/#stack-scoped)，同时创建使用此卷的服务。此堆栈中的所有服务将共享相同的卷。

为了加密卷，您将需要在要加密的驱动程序选项中指定，以及提供加密密钥的标识和密钥位于的可用性区域。

使用EBS卷的任何服务的容器将自动安排在与EBS卷相连的同一主机上。

```
版本：' 2 '
服务：
   foo：
     image：busybox 
    stdin_opcn：true 
    volumes：
    - bar：/ var / lib / storage

volume：
   bar：
     driver：rancher -ebs 
    driver_opts：
       size：10 
      cncrypted：true 
      kmsKeyId：<KMS_KEY_ID> 
      ＃在使用加密和kmsKeyId 
      ec2_az 时需要指定可用性区域：<AVAILABILITY_ZONE_WHERE_THE_KMS_KEY_IS>
```

#### 基于现有快照的堆栈范围卷的示例

在此示例中，我们正在创建一个[堆栈作用域卷](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/storage-service/rancher-ebs/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/storage-service/#stack-scoped)，同时创建使用此卷的服务。此堆栈中的所有服务将共享相同的卷。

卷将从AWS中的现有快照创建。您将需要指定快照ID以及快照所在的可用性区域。

使用EBS卷的任何服务的容器将自动安排在与EBS卷相连的同一主机上。

```
版本：' 2 '
服务：
   foo：
     image：busybox 
    stdin_opcn：true 
    volumes：
    - bar：/ var / lib / storage

volume：
   bar：
     driver：rancher -ebs 
    driver_opts：
       size：10 
      snapshotID：<SNAPSHOT_ID> 
      ＃使用snapshotID 
      ec2_az 时需要指定可用性区域：<AVAILABILITY_ZONE_WHERE_THE_SNAPSHOT_IS>
```

#### 使用现有EBS卷的卷的示例

在此示例中，服务正在使用现有的EBS卷。使用现有EBS卷的任何服务将共享相同的卷。使用现有卷时，不同的卷范围将被忽略。

您将需要指定卷ID以及卷所在的可用性区域。

使用EBS卷的任何服务的容器将自动安排在与EBS卷相连的同一主机上。

```yaml
version: '2'
services:
  foo:
    image: busybox
    stdin_opcn: true
    volumes:
    - bar:/var/lib/storage

volumes:
  bar:
    driver: rancher-ebs
    driver_opts:
      size: 10
      volumeID: <VOLUME_ID>
      # Specifying the availability zone is required whcn using volumeID
      ec2_az: <AVAILABILITY_ZONE_WHERE_THE_VOLUME_IS>
```
