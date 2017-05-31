---
title: Hosts in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 主机入门

------

主机是Rancher中最基本的资源单元，并且表示为虚拟或物理的任何Linux服务器，具有以下最低要求：

- 任何流行的Linux发行[版]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts/index.md#supported-docker-versions)都具有受支持的Docker版本。RancherOS，Ubuntu，RHEL / CcntOS 7进行了更严格的测试。
  - 对于RHEL / CcntOS，[Docker](https://docs.docker.com/cngine/refercnce/commandline/dockerd/#/storage-driver-options)不推荐使用默认存储驱动程序，即使用环回的devicemapper 。请参考Docker文档，了解如何更改。
  - 对于RHEL / CcntOS，如果要启用SELinux，则需要[安装其他SELinux模块]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//installing-rancher/selinux)。
  - 对于RHEL / CcntOS，请使用内核版本`3.10.0-514.2.2.el7.x86_64`或更高版本。使用7.3版或更高版本时包括。
- 1GB RAM
- 推荐CPU / AES-NI
- 能够通过预先配置的端口通过http或https与Rancher服务器进行通信。默认值为8080。
- 能够在同一环境下路由到任何其他主机，以利用Rancher对Docker容器的跨主机网络。

Rancher还支持Docker Machine，并允许您通过任何支持的驱动程序添加主机。

从基础 **设施** - > **主机**选项卡，单击**添加主机**。

### 支持的Docker版本

| 版                   | 支持的？  | Kubernetes支持？ | 安装脚本                                     |
| ------------------- | ----- | ------------- | ---------------------------------------- |
| `1.9.x` 早些时候        | 没有    |               |                                          |
| `1.10.0` - `1.10.2` | 没有    |               |                                          |
| `1.10.3` （和更高）      | **是** | **是**         | `curl [https://releases.rancher.com/install-docker/1.10.sh](https://releases.rancher.com/install-docker/1.10.sh) |
| `1.11.x`            | 没有    |               | `curl [https://releases.rancher.com/install-docker/1.11.sh](https://releases.rancher.com/install-docker/1.11.sh) |
| `1.12.0` - `1.12.2` | 没有    |               |                                          |
| `1.12.3` （和更高）      | **是** | **是**         | `curl [https://releases.rancher.com/install-docker/1.12.sh](https://releases.rancher.com/install-docker/1.12.sh) |
| `1.13.x`            | **是** | 没有            | `curl [https://releases.rancher.com/install-docker/1.13.sh](https://releases.rancher.com/install-docker/1.13.sh) |
| `17.03.x-ee`        | **是** | 没有            | N / A                                    |
| `17.03.x-ce`        | **是** | 没有            | `curl [https://releases.rancher.com/install-docker/17.03.sh](https://releases.rancher.com/install-docker/17.03.sh) |
| `17.04.x-ce`        | 没有    |               | `curl [https://releases.rancher.com/install-docker/17.04.sh](https://releases.rancher.com/install-docker/17.04.sh) |
| `17.05.x-ce`        | 没有    | 没有            |                                          |

### 安装特定Docker版本

该标准`curl https://get.docker.com | sh`始终安装当时可用的最新Docker版本，并且可能不受您安装的Rancher版本的支持。相反，我们建议您使用上述脚本来安装特定版本。这些是标准脚本的稍微修改版本，并将安装引导到特定的Docker引擎版本。也可以使用精确的补丁版本`1.<x>.<y>.sh`，例如`1.12.6.sh`。

> **注意：**如果要从UI启动主机，可以选择要在主机上安装的Docker版本。在**高级选项**部分，有一个**Docker安装URL**。

### 主机如何工作？

当主机上启动Rancher代理程序容器时，主机将连接到Rancher服务器。注册令牌（它是**添加主机** - > **自定义**屏幕中的长URL）由Rancher代理程序第一次用于连接到服务器。连接后，它会在Rancher服务器中生成代理帐户和API密钥对。然后，密钥对用于所有后续通信，使用与其他类型的帐户相同的认证和授权逻辑，如环境API密钥。

设计是因为它在外部运行并且潜在的敌对（服务器）硬件，因此该代理是不受信任的。代理帐户只能访问API中需要的资源，对事件的回复被检查事件是否实际发送到该代理程序等。代理程序验证主机的方向不是相反的方向，所以您还可以设置TLS，证书将被验证。

注册令牌是每个[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)。它在服务器上生成，存储在数据库中，并作为使用API密钥对进行代理注册的一部分发送到主机。连接是主机和AES加密之间的点对点，这是大多数现代CPU加速的。

### 添加主机

您首次添加主机时，可能需要设置[主机注册URL](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/settings/#host-registration)。此设置确定您的主机将连接到Rancher API的DNS名称或IP地址以及端口。默认情况下，我们选择了管理服务器的IP和端口`8080`。如果您选择更改地址，请确保指定应用于连接到Rancher API的端口。随时可以更新[主机注册](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/settings/#host-registration)。设置主机注册后，单击**保存**。

我们支持直接从云提供商添加主机或添加已经配置的主机。对于云提供商，我们提供使用`docker-machine`和支持任何支持的图像`docker-machine`。

选择要添加的主机类型：

- [添加自定义主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)
- [添加Amazon EC2主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/amazon)
- [添加Azure主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/azure)
- [添加DigitalOcean主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/digitalocean)
- [添加Exoscale主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/exoscale)
- [添加分组主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/packet)
- [添加Rackspace主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/rackspace)
- [从其他云提供商添加主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/other)

当主机被添加到Rancher时，在主机上启动一个牧场主代理容器。Rancher会自动提取正确的图像版本标签`rancher/agcnt`并运行所需的版本。代理版本被特别标记给每个Rancher服务器版本。

### 主机标签

使用每个主机，您可以添加标签来帮助您组织主机。标签在启动牧师/代理容器时作为环境变量添加。UI中的主机标签将是一个键/值对，密钥必须是唯一的标识符。如果您添加了两个不同值的键，我们将把最后输入的值用作键/值对。

通过向主机添加标签，您可以在[计划服务/负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)上使用这些标签，并创建允许您的[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)运行的主机的白名单或黑名单。

如果您打算使用[外部DNS服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service)，并且需要[使用主机IP以外的IP来对DNS记录进行编程](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/external-dns-service/#using-a-specific-ip-for-external-dns)，则需要`io.rancher.host.external_dns_ip=<IP_TO_BE_USED_FOR_EXTERNAL_DNS>`在主机上包含该标签。主机标签可以在注册主机或主机添加到Rancher后添加，但在外部DNS服务启动之前应将其添加到主机。当为外部DNS服务编程规则时，将使用此标签的值。

当使用UI添加不同的云提供商的主机时，将使用在UI中添加的主机标签为您自动启动rancher / agcnt命令。

添加自定义主机时，您可以使用UI添加标签，并且会自动将UI / UI对象的环境变量（`CATTLE_HOST_LABELS`）添加到UI界面上的命令中。

##### 例

```
＃添加一个主机标签牧场主/ agcnt命令 
$须藤泊坞运行-e CATTLE_HOST_LABELS = “富=酒吧” -d --privileged \
    -v /var/run/docker.sock:/var/run/docker.sock牧场主/agcnt:v0.8.2 \
    http：// < rancher-server-ip >：8080 / v1 / projects / 1a5 / scripts / < registrationTokcn > 

＃添加多个主机标签需要使用`＆` 
$ sudo的搬运工运行-e CATTLE_HOST_LABELS = '富=酒吧和你好=世界' -d --privileged \
    -v /var/run/docker.sock:/var/run/docker.sock牧场主/剂：v0.8.2 \
    HTTP：// < rancher-server-ip >：8080 / v1 / projects / 1a5 / scripts / < registrationTokcn >
```

> **注：**该`rancher/agcnt`版本相关的牧场主服务器版本。您将需要检查自定义命令以获取要使用的版本的相应标签。

#### 自动应用主机标签

Rancher自动创建与主机的linux内核版本和Docker cngine版本相关的主机标签。

| 键                                      | 值                        | 描述                  |
| -------------------------------------- | ------------------------ | ------------------- |
| `io.rancher.host.linux_kernel_version` | 主机上的Linux内核版本（例如，`3.19`） | 在主机上运行的Linux内核的版本   |
| `io.rancher.host.docker_version`       | 主机上的Docker版本（例如`1.10`）   | Docker cngine版本在主机上 |
| `io.rancher.host.provider`             | 云提供商信息                   | 云提供商名称（目前仅适用于AWS）   |
| `io.rancher.host.region`               | 云提供商区域                   | 云供应商区域（目前仅适用于AWS）   |
| `io.rancher.host.zone`                 | 云提供商区域                   | 云提供商区域（目前仅适用于AWS）   |

### 计划程序IP

为了[能够在多个IP上发布端口](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling/#scheduling-against-multiple-ips-of-a-host)，需要对主机进行配置，以使Rancher知道可以安排哪些IP。为主机添加调度程序IP的方法取决于主机是否已经在Rancher（即Rancher代理已经启动）与新主机（即Rancher代理尚未启动）之间。

#### 将调度程序IP添加到现有主机

对于环境中的任何现有主机，可以通过添加特定的主机标签（`io.rancher.scheduler.ips`向主机添加其他IP）以进行计划，在UI中，单击**主机**的**“编辑主机** ”，然后添加一个**计划程序IP**，如果要更新通过API主机详细信息，您将添加主机标签`io.rancher.scheduler.ips`，并以逗号分隔列表（即`1.2.3.4, 2.3.4.5`）列出IP 。

> **注意：**如果在添加调度程序IP之前，在主机上发布了任何端口用于服务的端口，则这些端口将被发布`0.0.0.0`，这意味着它们将在所有IP上使用，包括在服务启动后添加的调度程序IP。

#### 为新主机添加计划程序IP

对于尚未添加到Rancher的任何[自定义主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)，`CATTLE_SCHEDULER_IPS`可以将一个环境变量（ie ）添加到Rancher代理命令中，以列出主机上的可用IP。

```
$ sudo docker run -e CATTLE_SCHEDULER_IPS = ' 1.2.3.4，<IP2>，.. <IPN> '- d --privileged \ 
    -v /var/run/docker.sock:/var/run/docker.sock rancher /代理：v0.8.2 \ 
    http：// < rancher -server-ip >：8080 / v1 / projects / 1a5 / scripts / < registrationTokcn >
```

### 主机在HTTP代理之后

如果您在HTTP代理之后，为了将主机添加到Rancher服务器，您将需要编辑主机的Docker守护程序以指向代理。我们[添加自定义主机页面](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom/#hosts-behind-a-proxy)中列出了详细的说明。

### 从Cloud Providers访问主机

如果您选择通过Rancher选择启动主机，则Rancher正在云服务提供商中进行Docker Machine呼叫。我们提供在一个易于下载的文件启动机器时生成的所有证书。在主机的下拉菜单中单击**机器配置**。它将下载一个具有所有证书的tar.gz文件。

要SSH进入您的主机，请转到您的终端/命令提示符。使用`id_rsa`证书导航到所有证书和ssh的文件夹。

```
$ ssh -i id_rsa root @ < IP_OF_HOST >
```

### 克隆主机

由于在云提供商上启动主机需要使用访问密钥，因此您可能希望轻松创建另一个主机，而无需再次输入所有凭据。Rancher提供克隆这些凭据以启动新主机的功能。从主机的下拉菜单中选择**克隆**。它将显示一个**添加主机**页面，其中包含克隆的主机的凭据。

### 编辑主机

主机可以做什么的选项位于主机的下拉列表中。从基础**结构** - > **主机**页面，当您将鼠标悬停在**主机**上时，将出现下拉图标。如果您单击主机名以查看主机的更多详细信息，则下拉图标位于页面的右上角。它位于主机状态旁边。

如果选择**编辑**，可以更新主机上的名称，说明或标签。

### 停用/激活主机

停用主机将使主机处于*非活动*状态。在这种状态下，不能部署新的容器。主机上的任何活动容器将继续处于活动状态，您仍然可以对这些容器执行操作（启动/停止/重启）。主机仍将连接到Rancher服务器。从主机的下拉菜单中选择**停用**。

当主机处于*非活动*状态时，您可以通过从主机的下拉菜单中单击**激活**将主机恢复为*活动*状态。****

> **注意：**如果主机在Rancher中处于关闭状态（即处于`reconnecting`或`inactive`状态），则需要执行运行[状况检查](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/health-checks)，以使Rancher将服务上的容器启动到不同的主机。

### 删除主机

为了从服务器中删除主机，您需要从下拉菜单中进行几个步骤。

选择**停用**。主机完成停用后，主机将显示*非活动*状态。选择**删除**。服务器将从Rancher服务器实例开始主机的删除过程。完成删除后将显示的第一个状态将被*删除*。它将继续完成删除过程，并进入*清除*状态，然后立即从UI消失。

如果主机是使用Rancher在云提供商上创建的，则主机将从云提供商中删除。如果通过使用[自定义命令](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/hosts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)添加了主机，主机将保留在云提供商上。

> **注意：**对于自定义主机，包括Rancher代理在内的所有容器将继续保留在主机上。

### 删除Rancher外的主机

如果您的主机在Rancher之外被删除，那么Rancher服务器将继续显示主机，直到它被删除。最终，这些主机将显示在*重新连接*状态，并且无法重新连接。您将能够**删除**这些主机以从UI中删除它们。