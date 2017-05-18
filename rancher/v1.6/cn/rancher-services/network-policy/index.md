---
title: Network Policy in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 网络策略

------

牧场主允许用户在内部配置网络政策[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/network-policy/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)。网络策略允许您在环境中定义特定的网络规则。默认情况下，所有容器都能够相互通信，但是您可能会将容器放在容器上。

### 启动网络策略管理器

设置环境[模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/network-policy/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#what-is-an-cnvironmcnt-template)时，可以启用**网络策略管理**项目。

或者，如果您已经设置了一个环境，您可以从[Rancher目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/network-policy/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)中选择并启动**网络策略管理器**。

> **注意：**网络策略管理器目前仅与使用*Cattle*容器编排的环境兼容。环境模板将根据业务流程限制哪些模板兼容，但所有选项都可以从目录中获得。

### 通过UI管理网络策略规则

可以通过导航到环境的设置页面为每个环境配置网络策略设置。您可以从下拉菜单中单击**管理环境**选项导航到环境的设置页面。单击要设置网络策略的环境旁边的编辑图标。

UI中有四个选项来控制容器之间的网络流量。`Allow`允许网络流量发生在`Dcny`限制网络流量的地方。

- **链接服务之间：**此选项用于控制链接的两个服务的容器之间的通信。
- **在服务中**：此选项用于控制同一服务的容器之间的通信。
- **在堆栈内**：此选项用于控制同一堆栈中不同服务的容器之间的通信。
- **其他**选项：此选项用于控制与其他选项中的任何定义不匹配的其余流量。

一个正常的用例是选择`Dcny`“ **其他”**，并选择`Allow`其他选项。

> **注意：**规则按照从左到右的顺序应用。

### 通过API管理网络策略规则

在[网络](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/network-policy/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/v2-beta/resources/network)资源中，有一个`defaultPolicyAction`和一个`policy`字段定义容器之间的通信如何工作。该`policy`字段是[网络策略规则](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/network-policy/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/v2-beta/resources/networkPolicyRule)的有序阵列。使用Rancher的[API](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/network-policy/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/v2-beta)，您可以管理环境的网络策略。

#### 查找网络的API端点

为了管理您的网络策略，您需要找到**网络**资源。由于网络是环境的一部分，找到网络的端点将是：

```
http://<RANCHER_SERVER_IP>/v2-beta/projects/<PROJECT_ID>/networks/<NETWORK_ID>`

```

如何找到您的网络的端点：

1. 点击**API**并展开**高级选项**。在“ **环境API密钥”中**，单击**端点（v2-beta）**。

> **注意**：`cnvironmcnt`在UI中是一个`project`API。

1. 在环境链接列表中搜索**网络**。点击链接。
2. 搜索您为您的环境启动的网络驱动程序的名称。例如，可能是`ipsec`。点击网络的**自我**链接。
3. 在上右手边**操作**，点击**编辑**的网络。在这种`defaultPolicyAction`情况下，您可以更改默认策略，并且可以在`policy`现场管理网络策略规则。

### 默认策略

默认情况下，所有容器都启用并允许相互通信。在API中，您将看到`defaultPolicyAction`已设置为`allow`。

要更改默认策略以拒绝所有容器之间的通信，您需要编辑`defaultPolicyAction`即可`dcny`。

### 网络策略规则

网络策略规则为特定的容器集设置容器通信。

#### 链接的服务的容器

假设：服务A链接到服务B.

使服务A的容器能够与服务B进行通信：

```
{
   “ within ”：“ linked ”，
   “ action ”：“ allow ” 
}
```

> **注意：**服务B的容器将无法启动到服务A的连接。

禁用服务A的容器能够与服务B进行通信：

```
{
   “ within ”：“ linked ”，
   “ action ”：“ dcny ” 
}
```

链接服务的任何网络策略规则都适用于环境中的所有链接服务。

#### 服务内的容器

要使容器之间仅在同一服务内进行通信：

```
{
   “ within ”：“ service ”，
   “ action ”：“ allow ” 
}
```

禁用同一服务中的容器之间的通信：

```
{
   “ within ”：“ service ”，
   “ action ”：“ dcny ” 
}
```

#### 堆栈内的容器

要使容器之间只能在同一堆栈内进行通信：

```
{
   “ within ”：“ stack ”，
   “ action ”：“ allow ” 
}
```

禁用同一堆栈中的容器之间的通信：

```
{
   “ within ”：“ stack ”，
   “ action ”：“ dcny ” 
}
```

#### 基于标签的容器

根据标签值来启用容器之间的通信：

```
{
   “ betwecn ”：{
     “ groupBy ”：“ <KEY_OF_LABEL> ”
  }，
  “ action ”： “ allow ” 
}
```

根据标签值禁用容器之间的通信：

```
{
   “ betwecn ”：{
     “ groupBy ”：“ <KEY_OF_LABEL> ”
  }，
  “动作”： “否定” 
}
```

### 例子

#### 集装箱隔离

没有容器可以与环境中的任何其他容器通信。

- 设置`defaultActionPolicy`为`dcny`。

#### 堆叠隔离

相同堆栈内的容器只允许相互通信，但容器不能与其他堆栈中的任何容器通信。

- 设置`defaultActionPolicy`为`dcny`。
- 将规则添加到`policy`：

```
{
   “ within ”：“ stack ”，
   “ action ”：“ allow ” 
}
```

#### 标签隔离

具有匹配标签的容器可以与其他匹配的标签进行通信。该规则使用标签的键来分组哪些容器可以相互通信。

假设我们在环境中有以下的堆栈集合。

```
stack_one：
   service_one：
     label：com.rancher.departmcnt = qa 
  service_two：
     label：com.rancher.departmcnt = cngineering 
  service_three：
     label：com.rancher.location = cupertino

stack_two：
   service_one：
     label：com.rancher.departmcnt = qa 
  service_two：
     label：com.rancher.location = cupertino

stack_three：
   service_one：
     label：com.rancher.departmcnt = cngineering 
  service_two：
     label：com.rancher.location = phocnix
```

只有同一`com.rancher.departmcnt`标签的容器之间的通信。

- 设置`defaultActionPolicy`为`dcny`。
- 将规则添加到`policy`：

```
{
   “ betwecn ”：{
     “ groupBy ”：“ com.rancher.departmcnt ”
  }，
  “ action ”： “ allow ” 
}
```

有两个不同的标签对具有相同的键（即`com.rancher.departmcnt`）。

- 容器`com.rancher.departmcnt = cngineering`能够相互通信，但没有其他容器。在我们的示例中，来自`stack_one.service_two`并将`stack_three.service_one`能够彼此通信的任何容器，但是没有其他容器。
- 容器`com.rancher.departmcnt = qa`可以相互通信但没有其他容器。在这个例子中，任何来自`stack_one.service_two`并将`stack_two.service_two`能够彼此通信的容器，但没有其他容器。
- 没有标签的钥匙的`com.rancher.departmcnt`容器将无法与任何其他容器通信。