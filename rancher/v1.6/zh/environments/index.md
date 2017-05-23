---
title: cnvironmcnts in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 环境

------

### 什么是环境？

Rancher支持将资源分组到多个环境中。每个环境都从用于创建环境的环境[模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/index.md#what-is-an-cnvironmcnt-template)定义的一组基础[结构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)开始。每个环境都有自己的资源集，由一个或多个用户或组拥有。例如，您可以创建单独的“开发”，“测试”和“生产”环境，以保持彼此的隔离，并为您的整个组织提供“开发”访问权限，但将“生产”环境限制为较小的团队。

所有主机和任何Rancher资源（如容器，基础设施服务等）都在环境中创建并属于环境。

### 添加环境

要添加新的环境，将鼠标悬停在您所在的当前环境的名称（位于左上角）。将显示所有可用环境的下拉菜单以及“ **管理环境”**的链接。点击**管理环境**。

导航到“ **环境”**页面后，您将看到一个环境列表和一个环境模板列表。如果你是一个[管理](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/accounts/#admin)的农场，你会看到每个人的环境的列表，即使你没有被邀请成为[一部分](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#membership-roles)是环境。对于所有用户，所有环境模板都可用。

点击**添加环境**。每个环境都有自己的名称和描述。您将选择要使用的环境模板。在每个环境模板中，您将能够看到模板中启用了哪些基础架构服务。

> **注意：**如果您尚未配置[访问控制](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)，任何访问Rancher的用户都可以使用所有环境。任何环境的会员资格都不受限制。

有两种方法可以将成员添加到环境中。提供用户名，点击**+**将名称添加到成员列表中。如果名称不在列表中，则不会将其添加到环境中。或者，**+**按钮右侧有一个下拉按钮，它将显示某些身份验证类型的组织/团队。

对于每个成员（即个人，团队或组织），您可以将角色定义为[所有者](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/index.md#owners)，[成员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/index.md#members)，[受限用户](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/index.md#restricted)或[只读用户](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/index.md#read-only)。默认情况下，它们作为成员添加到列表中。您可以在名称旁边的下拉列表中更改角色。作为所有者，您随时可以随时更改成员及其角色列表，但只有所有者才能更改环境的成员身份和成员角色。

> **注意：**只有业主和管理员才能看到[环境的基础设施服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)。

单击**创建**，环境将立即可用于成员列表上的任何人。在创建环境并添加主机后，为环境启用的基础架构服务将开始部署。

### 停用和删除环境

创建环境后，所有者可能需要停用或删除环境。

停用环境将会从环境的任何成员中消除观看能力。所有业主仍将能够查看和激活环境。在重新激活之前，您将无法更改环境的成员资格。随着您的服务或基础架构，任何事情都不会改变。因此，如果要对基础架构服务进行任何更改，则在环境停用之前，您需要进行这些更改。

为了删除环境，您需要先停用它。在环境中创建的所有注册表，平衡器和API密钥将从Rancher中删除。通过使用Docker Machine启动主机的Rancher UI创建的任何主机也将通过使用Docker Machine从云提供商中删除。如果您使用[自定义](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)选项添加了主机，则主机将不会从云提供商中删除。

### 编辑会员

只有业主可以将会员身份更改为环境。如果环境处于停用状态，所有者仍然可以编辑其成员资格。在“ **管理环境”**页面中，他们将能够**编辑**环境。在编辑页面中，他们可以通过查找姓名和点击**+**按钮或从下拉菜单中选择名称来添加其他成员。

如果要删除任何成员，请在成员列表中单击其名称旁边的**X. **请记住，如果删除单个用户，如果它们是作为环境成员的团队或组织的一部分，他们仍然可以访问环境。

所有者也可以更改成员列表中任何人的角色。只需选择特定用户所需的角色。

### 会员角色

#### 拥有者

环境所有者有能力改变环境的状态或成员资格。在会员列表中，所有者还可以更改其他环境成员的角色。

所有者可以通过[目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)更改[环境的基础架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)，因为没有编辑环境模板的方法。环境模板仅在创建环境时使用。

#### 会员

环境成员可以在Rancher中执行不影响环境的任何操作。他们将无法添加/删除成员或更改现有成员的成员角色或查看任何基础[架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)。

#### 受限

环境的限制角色能够执行与堆栈/服务相关的所有操作。对于任何服务容器，他们可以执行所有操作，即启动，停止，删除，升级，克隆和编辑。对于与堆栈，服务和容器相关的操作，没有任何限制。

他们的角色的限制是关于**主机**。它们只能查看环境的主机，并且无法在环境中添加/编辑/删除主机。

> **注意：**受限用户将无法添加/删除主机标签，并且需要请求成员/所有者对主机标签进行任何更改。

#### 只读

只读环境的角色只能查看环境中的所有资源。他们可以查看主机，堆栈，服务，容器，但是无法执行任何操作来创建，编辑或删除它们。

> **注意：**使用此角色，它们仍然可以查看容器的日志。

### 什么是环境模板

环境模板允许用户定义要部署的基础架构服务的不同组合。基础设施服务，包括但不限于集装箱业务流程（即牛，[kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes)，[mesos](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/mesos)，[群](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/swarm)），[网络](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)，牧场主服务（即[健康检查](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/health-checks)，[DNS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/dns-service)，[元数据](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/metadata)，[调度](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)，服务发现和[存储](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/storage-service)。

在Rancher中，提供了一组默认模板，以利用各种容器编排选项与推荐的基础架构服务。像Rancher调度程序这样的基础设施服务中的一些只适用于牛的环境，但是对于其他业务流程类型来说，这些服务是必需的，因为这些服务用于启动其他基础架构服务。除了默认模板，您还可以创建自己的模板，以选择您希望在环境中使用的基础架构服务的组合。只有[业主](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/index.md#owners)或[管理员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#admin)可以在环境中查看和管理这些基础架构资源。

在与用户共享环境之前，建议设置[访问控制](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)。通过将用户添加到您的环境中，他们还可以创建服务和管理资源。

> **注意：**无法在多个环境中共享基础设施资源。[注册表](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/registries)，[证书](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/certificates)和环境[API密钥](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/api-keys)也是环境特定的。

### 添加环境模板

要添加新的环境，将鼠标悬停在您所在的当前环境的名称（位于左上角）。将显示所有可用环境的下拉菜单以及“ **管理环境”**的链接。点击**管理环境**。

导航到“ **环境”**页面后，您将看到一个环境列表和一个环境模板列表。点击**添加模板**

为模板选择**名称**和**说明**。选择您要共享模板的方式。模板可以是私有的（即仅对您自己可见）或公开（即管理员可见）。

[基础架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)包括但不限于选择容器编排，[存储](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/storage-service)和[网络](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)。自动启用具有工作环境所需的默认基础架构服务。

### 编辑和删除环境模板

创建环境模板后，您可以编辑模板中启用的基础设施服务。即使可以更改环境模板，基于模板的现有环境也不会更新为新的模板。他们将继续在环境中启用原有的基础架构服务。

在任何时候，您可以随时删除环境模板，因为它们仅在启动环境时才使用，以指示应启动哪些基础设施服务。模板不直接绑定到环境，所以删除它们不会影响任何环境。