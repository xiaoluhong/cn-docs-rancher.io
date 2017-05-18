---
title: Load Balancers
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 负载平衡器

------

Rancher提供在Rancher内使用不同负载平衡器驱动程序的能力。负载平衡器可以通过向目标服务添加规则来将网络和应用程序流量分配到单个容器。任何目标服务将使其所有底层容器自动注册为Rancher的负载平衡器目标。使用Rancher，可以轻松地在堆栈中添加负载均衡器。

默认情况下，Rancher使用HAProxy提供了一个托管负载平衡器，可以手动缩放到多个主机。本文档中的其他示例将涵盖负载平衡器的不同选项，但具体参考了我们的HAProxy负载均衡器服务。我们计划添加额外的负载平衡器提供程序，所有负载平衡器的选项将是相同的，不管负载平衡器提供程序。

我们使用循环算法将流量分配给目标服务。该算法可以在[自定义HAProxy配置](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#custom-haproxy-configuration)中[自定义](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#custom-haproxy-configuration)。或者，您可以配置负载均衡器将流量路由到与负载均衡器容器位于同一主机上的目标容器。通过向负载均衡器添加一个[特定的标签](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling/#target-service-labels)，它将配置负载平衡器，以将目标只与负载均衡器（即`io.rancher.lb_service.target=only-local`）处于同一主机上的容器，或者将这些容器放在不同主机上的容器上`io.rancher.lb_service.target=prefer-local`。

我们会检讨我们的负载均衡器的选项[UI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#load-balancer-options-in-the-UI)和[牧场主撰写](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#load-balancer-options-in-rancher-compose)使用和展示的例子[UI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers/#adding-a-load-balancer-in-the-ui)和[牧场主撰写](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers/#adding-a-load-balancer-with-rancher-compose)。

### 在UI中添加负载平衡器

我们将介绍如何为[添加服务部分](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services/#adding-services-in-the-ui)之前创建的“letschat”应用程序设置负载均衡器。

首先，您首先创建一个负载平衡器，方法是单击“添加服务”旁边的下拉图标，然后单击**添加负载平衡器**。默认情况下，刻度将为1容器。提供一个名称，如“LetsChatLB”。

对于端口规则，使用默认`Public`访问，默认`http`协议，源端口`80`，选择“letschat”服务，并使用目标端口`8080`。单击**创建**。

现在，我们来看看负载平衡器的动作。在堆栈视图中，有一个链接到`80`您用作负载平衡器的源端口的端口。如果您单击它，它将自动在浏览器中显示一个新选项卡，并指向启动了负载平衡器的其中一个主机。该请求被重定向到“LetsChat”容器之一。如果您要刷新，负载均衡器会将新请求重定向到“letschat”服务中的另一个容器。

### 负载平衡器选项在UI中

Rancher提供在容器内运行HAProxy软件的负载均衡器，以将流量引导到目标服务。

> **注意：**负载平衡器仅适用于正在使用受管网络的服务。如果您为目标服务选择任何其他网络选项，则它将**无法**与负载平衡器一起使用。

您可以通过单击**添加服务**按钮旁边的下拉图标并选择**添加负载平衡器**来**添加负载平衡器**。

您可以使用滑块选择刻度，即负载平衡器的容器数量。或者，您可以选择**始终在每个主机上运行此容器的一个实例**。使用此选项，您的负载平衡器将扩展到添加到您的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)中的任何其他主机。如果您在**计划**部分中有调度规则，则Rancher将只启动满足调度规则的主机上的容器。如果您添加的主机不符合调度规则的环境，则不会在主机上启动容器。

> **注意：**负载平衡器的规模不能超过环境中的主机数量，否则会出现端口冲突，并且负载均衡器服务将被卡在激活状态。它将继续尝试找到可用的主机和打开端口，直到您编辑此负载平衡器的比例或[添加其他主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)。

您将需要提供**姓名**，如果需要，**说明**负载平衡器。

接下来，您将为负载平衡器定义端口规则。可以创建两种类型的端口规则。有针对现有服务和选择器规则的服务规则，其将针对与选择器条件匹配的服务。

创建服务和选择器规则时，主机名和路径规则按照用户界面中显示的顺序从顶部到底部进行匹配。

#### 服务规则

服务规则是用于定位Rancher中现有服务的端口规则。

在“ **访问”**部分，您将决定是否可以公开**访问**此负载均衡器端口（即可以在主机外部访问），或仅在环境中内部**访问**此负载平衡器端口。默认情况下，Rancher假定您希望端口是公开的，但您可以选择`Internal`是否只希望端口只能在同一环境中的服务访问。

选择**协议**。详细了解我们的[协议选项](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#protocol)。如果您选择选择需要SSL终止（即`https`或`tls`）的协议，则将在**SSL终止**选项卡中添加**证书**。

接下来，您将提供**请求主机**，**源端口**和**路径**，以便流量来自哪里。

> **注意：**端口`42`不能用作负载平衡器的源端口，因为Rancher会使用此端口进行[运行状况检查](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/health-checks)。

##### 请求主机/路径

请求主机可以是每个服务的特定HTTP主机头。请求路径可以是特定路径。请求主机和请求路径可以单独使用或一起使用来创建特定的请求。

###### 例：

```
domain1.com -> Service1
domain2.com -> Service2

domain3.com -> Service1
domain3.com/admin -> Service2

```

###### 通配符

添加主机路由时，Rancher会支持通配符。支持以下通配符语法。

```
*.domain.com -> hdr_end(host) -i .domain.com
domain.com.* -> hdr_beg(host) -i domain.com.

```

##### 目标服务和港口

对于每个服务规则，您选择特定的**目标**服务来引导流量。服务列表基于该环境中的所有服务。随着服务，您选择哪个**端口**将流量引导到服务上。该服务上的这个专用端口通常是映像上的暴露端口。

#### 选择规则

对于选择器规则，而不是定位特定服务，您将提供一个[选择器值](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/labels/#selector-labels)。选择器用于根据服务的标签来接收目标服务。当创建负载平衡器时，将根据环境中的任何现有服务来评估选择器规则，以查看是否存在任何现有的目标服务。任何附加服务或对服务标签的更改都将与选择器值进行比较，以查看服务是否应为目标服务。

对于每个**源端口**，您可以添加**请求主机**和/或**路径**。该[选择值](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/labels/#selector-labels)是根据提供**的目标**，你可以提供一个特定**端口**的流量引导到该服务。该服务上的这个专用端口通常是映像上的暴露端口。

##### 示例：2选择器规则

1. 源端口：`100`; 选择：`foo=bar`; 港口：`80`
2. 源端口：`200`; 选择：`foo1=bar1`; 港口：`80`

- 服务A具有`foo=bar`标签，并且将匹配第一选择器规则。任何流量`100`将被引导到服务A.
- 服务B具有`foo1=bar`标签，并且将匹配第二选择器规则。任何流量`200`将被引导到服务B.
- 服务C既有`foo=bar`和`foo1=bar1`标签，同时匹配选择规则。来自任何一个源端口的流量将被引导到服务C。

> **注意：**目前，如果要为多个主机名/路径使用一个选择器源端口规则，则需要使用[Rancher Compose](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#selector)设置目标服务上的主机名/路径值。

#### SSL终止

“ **SSL终止** ”选项卡提供添加用于`https`和`tls`协议的证书的功能。在**证书**下拉列表中，您可以选择负载平衡器的主证书。

要向Rancher添加证书，请阅读[“ **基础架构** ”选项卡中如何添加证书](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/certificates)。

可以为负载平衡器提供多个证书，以便根据请求的主机名将适当的证书呈现给客户端（请参阅[服务器名称指示](https://en.wikipedia.org/wiki/Server_Name_Indication)）。这可能不适用于不支持SNI的老客户端（那些将获得主证书）。对于现代客户，如果没有匹配，他们将从有匹配的列表或主证书提供证书。

#### 负载平衡器的粘性政策

您可以选择负载平衡器的**粘性**。Stickiness是使用Cookie时使用的cookie策略。

Rancher支持的两个选项是：

- **无**：此选项意味着没有Cookie策略到位。
- **创建新的cookie**：此选项意味着cookie将被定义在应用程序之外。这个cookie是负载平衡器在请求和响应上设置的。该cookie将确定粘性策略。

#### 自定义HAProxy配置

由于Rancher正在使用HAProxy特定的负载平衡器，因此您可以自定义负载平衡器的HAProxy配置。本节中定义的任何内容将附加到Rancher生成的配置中。

##### 自定义HAProxy配置示例

```
global
    maxconn 4096
    maxpipes 1024

defaults
    log global
    mode    tcp
    option  tcplog

frontend 80
    balance leastconn

frontend 90
    balance roundrobin

backend mystack_foo
    cookie my_cookie insert indirect nocache postonly
    server $IP <server parameters>

backend customUUID
    server $IP <server parameters>

```

#### 标签/调度负载平衡器

我们提供向负载平衡器添加标签的能力，并计划启动负载平衡器的位置。[在这里](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling/#scheduling-options-in-the-ui)阅读有关标签和计划的更多详细信息。

### 添加一个负载平衡器与Rancher组合

我们将介绍如何为[添加服务部分](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services/#adding-services-with-rancher-compose)之前创建的“letschat”应用程序设置负载均衡器。

阅读更多关于如何[设置牧场主撰写](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose)。

> **注意**：在我们的示例中，我们将使用`<version>`作为负载平衡器的图像标签。每个版本的Rancher将具有`lb-service-haproxy`负载平衡器的特定版本。

我们将在UI示例中设置与上面我们使用的相同的示例。要开始，您将需要创建一个`docker-compose.yml`文件和一个`rancher-compose.yml`文件。使用Rancher Compose，我们可以启动负载均衡器。

#### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   letschatlb：
     ports：
    - 80 
    image：rancher / lb-service-haproxy：<version>
```

#### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   letschatlb：
     scale：1 
    lb_config：
       port_rules：
      - source_port：80 
        target_port：8080 
        service：letschat 
    health_check：
       port：42 
      interval：2000 
      unhealthy_threshold：3 
      healthy_threshold：2 
      response_timeout：2000
```

### 负载平衡器选项在Rancher组合

Rancher提供在容器内运行HAProxy软件的负载均衡器，以将流量引导到目标服务。

> **注意：**负载平衡器仅适用于正在使用受管网络的服务。如果您为目标服务选择任何其他网络选项，则它将**无法**与负载平衡器一起使用。

可以像任何其他服务一样安排负载平衡器。阅读更多关于使用Rancher Compose [调度](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling/#adding-labels-in-rancher-compose)负载平衡器。

负载平衡配置为在主机上暴露的端口和负载平衡器配置的组合，其可以包括每个目标服务的特定端口规则，自定义配置和粘性策略。

当使用包含[配对的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services/#sidekick-services)服务时，您需要使用[主服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services/#primary-service)作为目标服务，这是包含`sidekick`标签的服务。

### 源端口

创建负载平衡器时，可以添加要在主机上显示的任何端口。任何这些端口都可以用作负载均衡器的端口规则中的源端口。如果您想要一个内部负载平衡器，则不会在负载均衡器上公开任何端口，并且仅在负载平衡器配置中添加端口规则。

> **注意：** 端口`42`不能用作负载平衡器的端口，因为它内部用于[运行状况检查](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/health-checks)。

#### 例 `docker-compose.yml`

```
版本：“ 2 ”
服务：
   LB1：
     图像：牧场主/磅服务-HAProxy的：<版本> 
    ＃列出将运行负载平衡器在主机上暴露的任何端口
    ＃将流量导向至特定的服务，端口规则将需要添加 
    港口：
    - 80 
    - 81 
    - 90
```

### 负载平衡器配置

所有负载均衡器配置选项`rancher-compose.yml`在`lb_config`密钥下定义。

```
版本：' 2 '
服务：
   lb1：
     比例：1 
    ＃所有负载平衡器选项都配置在此密钥
    lb_config：
       port_rules中：
      - source_port：80 
        target_port：80 
        service：web1 
    health_check：
       port：42 
      interval：2000 
      unhealthy_threshold：3 
      healthy_threshold：2 
      response_timeout：2000 
  web1：
     scale：2
```

#### 端口规则

端口规则中定义的`rancher-compose.yml`。由于端口规则是单独定义的，因此可能会为同一服务定义多个端口规则。默认情况下，Rancher将根据具体的优先顺序对这些端口规则进行优先级排序。如果要更改[优先级](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#priority)排序，您还可以设置规则的特定[优先](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#priority)顺序。

#### 默认优先顺序

1. 没有通配符和URL的主机名
2. 没有通配符的主机名
3. 具有通配符和URL的主机名
4. 带有通配符的主机名
5. 网址
6. 默认（无主机名，无URL）

##### 源端口

源端口是主机上暴露的端口之一（即端口`docker-compose.yml`）。

如果要创建内部负载平衡器，则源端口不需要匹配`docker-compose.yml`文件中的任何端口。

##### 目标端口

目标端口是服务上的专用端口。此端口与用于启动服务的映像上暴露的端口相关。

##### 协议

Rancher负载平衡器驱动程序中支持多种协议类型。

- `http`- 默认情况下，如果没有设置协议，则负载平衡器使用`http`。HAProxy不会解密流量并直接传递流量
- `tcp` - HAProxy不会解密流量并直接传递流量
- `https` - 需要SSL终止。HAProxy使用提供的[证书](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/certificates)对流量进行解密，这些[证书](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/certificates)必须在加载平衡器中使用之前添加到Rancher中。从负载平衡器到目标服务的流量是未加密的。
- `tls` - 需要SSL终止。HAProxy使用提供的[证书](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/certificates)对流量进行解密，这些[证书](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/certificates)必须在加载平衡器中使用之前添加到Rancher中。从负载平衡器到目标服务的流量是未加密的。
- `sni` - 流量被加密到负载平衡器和服务。为负载均衡器提供多个证书，以便根据所请求的主机名将适当的证书呈现给客户机。（有关详细信息，请参阅[服务器名称指示](https://en.wikipedia.org/wiki/Server_Name_Indication)）。
- `udp` - Rancher的HAProxy提供商不支持此功能。

任何其他负载平衡器提供程序可能只支持一小部分协议。

##### 主机名路由

仅支持主机名路由`http`，`https`和`sni`。只有`http`和`https`支持基于路径的路由。

##### 服务

您希望负载平衡器将流量引导到的服务名称。如果服务是同一个堆栈，那么您使用服务名称。如果服务是在不同的堆栈，那么你会使用`<stack_name>/<service_name>`。

###### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb1：
     比例：1 
    lb_config：
       port_rules：
      - source_port：81 
        target_port：2368 
        ＃同一堆栈
        服务中的服务：ghost 
      - source_port：80 
        target_port：80 
        ＃在不同的堆栈
        服务中定位服务：differentstack / web1 
    health_check：
       port：42 
      interval：2000 
      不健康_threshold：3 
      healthy_threshold：2 
      response_timeout：2000 
  ghost：
     scale：2
```

##### 主机名和路径

Rancher的HAProxy负载平衡器通过在端口规则中指定主机头和路径来支持L7负载平衡。

###### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb1：
     比例：1 
    lb_config：
       port_rules：
      - source_port：81 
        target_port：2368 
        service：ghost 
        protocol：http 
        hostname：example.com 
        path：/ path / a 
    health_check：
       port：42 
      interval：2000 
      不健康_threshold：3 
      healthy_threshold：2 
      response_timeout：2000 
  ghost：
     scale：2
```

##### 通配符

添加主机路由时，Rancher会支持通配符。支持以下通配符语法。

```
*.domain.com -> hdr_end(host) -i .domain.com
domain.com.* -> hdr_beg(host) -i domain.com.

```

##### 优先

默认情况下，牧场主[优先端口规则](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#default-priority-order)针对相同的服务，但如果你愿意，你可以定制你自己的端口规则（低优先级越高）优先。

###### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb1：
     比例：1 
    lb_config：
       port_rules：
      - source_port：88 
        target_port：2368 
        服务：web1 
        协议：http 
        主机名：foo.com 
        优先级：2 
      - source_port：80 
        target_port：80 
        服务：web2 
        协议：http 
        优先级：1 
    health_check：
       端口：42 
      interval：2000 
      不健康_threshold：3 
      healthy_threshold：2 
      response_timeout：2000 
  web1：
     scale：2
```

##### 选择

您可以设置[选择器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/(%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/labels/#selector-labels))，而不是定位特定的服务。通过使用选择器，您可以在目标服务上定义服务链接和主机名路由规则，而不是在负载均衡器上。带有与选择器匹配的标签的服务将成为负载均衡器中的一个目标。

在负载平衡器中使用选择器时，`lb_config`可以在负载平衡器和与选择器匹配的任何目标服务上进行设置。

在负载平衡器中，[选择器值](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/labels/#selector-labels)设置在`lb_config`下面`selector`。`lb_config`负载平衡器中的端口规则不能具有服务，通常不具有目标端口。而是将目标端口设置在目标服务上的端口规则中。如果选择使用主机名路由，则将在目标服务上设置主机名和路径。

> **注意：**对于使用v1负载均衡器的任何负载平衡器，使用选择器标签的yaml字段将不会转换为v2负载平衡器，因为服务上的端口规则将不会更新。

###### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   LB1：
     图像：牧场主/磅服务-HAProxy的：<版本> 
    端口：
    - 81 
  ＃这些服务（web1和web2）将由负载平衡器作为目标
  web1 拾取：
     image：nginx 
    labels：
       foo：bar 
  web2：
     image：nginx 
    labels：
       foo：bar
```

###### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb1：
     比例：1 
    lb_config：
       port_rules：
      - source_port：81 
        ＃将具有foo = bar的任何服务定位为标签
        选择器：foo = bar 
        protocol：http 
    health_check：
       port：42 
      interval：2000 
      unhealthy_threshold：3 
      healthy_threshold：2 
      response_timeout：2000 
  ＃ web1和web2与源端口但具有不同的主机名和路径规则
  web1：
     scale：1 
    lb_config：
       port_rules：
      - target_port：80 
        hostname：test.com 
  web2：
     scale：1 
    lb_config：
       port_rules：
      - target_port：80 
        hostname：example.com/test     
```

##### 后端名称

如果要在负载均衡器配置中显式标记后端，则可以使用`backend_name`。如果要为特定后端配置自定义配置参数，则此选项将非常有用。

#### 证书

如果您正在使用`https`或`tls` [协议](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/index.md#protocol)，则可以使用[直接添加到Rancher](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/certificates)或从加载平衡器容器中安装的目录中的证书。

##### 引用添加到Rancher中的证书

证书在`lb_config`负载均衡器容器的部分中引用。

```
版本：' 2 '
服务：
   lb：
     scale：1 
    lb_config：
       certs：
      - <certName> 
      default_cert：<defaultCertName>
```

##### 将证书加载到负载平衡器容器中

*仅在撰写文件中支持*

证书可以作为卷直接安装到负载平衡器容器中。负载平衡器容器期望证书处于特定的目录结构中。如果您使用LetsEncrypt客户端来生成证书，那么您的目录结构已经按照Rancher所期望的格式进行配置。如果您不使用LetsEncrypt，那么导演和证书的名称将需要以特定的方式进行结构化。

Rancher的负载平衡器将轮询证书目录以进行更新。证书的任何添加/删除将通过轮询每30秒同步一次。

所有证书将位于单个基本证书目录下。此目录名称将用于负载均衡器服务的标签，以通知负载均衡器的证书在哪里。

在此基本目录中，为特定域生成的每个证书都需要放在子目录文件夹中。文件夹名称应为证书的域名，每个文件夹应包含私钥（即`privkey.pem`）和证书链（`fullchain.pem`）。对于默认证书，它可以放置在任何子目录名称中，但文件夹中的文件必须包含与证书（ie `privkey.pem`和`fullchain.pem`）相同的命名约定。

```
- 证书
   | -  foo.com
   |    | -  privkey.pem
   |    | -  fullchain.pem
   | -  bar.com
   |    | -  privkey.pem
   |    | -  fullchain.pem
   | -  default_cert_dir_optional
   |    | -  privkey.pem
   |    |  -  fullchain.pem
...
```

启动负载平衡器时，必须使用标签指定证书的位置和默认证书的位置。如果这些标签位于负载均衡器上，则负载平衡器将忽略负载均衡器`lb_config`密钥中的任何证书。

> **注意：**您不能将添加到Rancher的证书与通过卷挂入证书的容器结合使用。

```
标签：
   io.rancher.lb_service.cert_dir：<CERTIFICATE_LOCATION> 
  io.rancher.lb_service.default_cert_dir：<DEFAULT_CERTIFICATE_LOCATION>
```

证书可以通过使用主机绑定安装或使用命名卷与我们的[存储驱动程序](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/storage-service)作为卷驱动程序挂载到负载平衡器容器中。

###### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   lb：
     image：rancher / lb-service-haproxy：<TAG_BASED_ON_RELEASE> 
    卷：
    - / location / on / hosts：/ certs 
    ports：
    - 8087：8087 / TCP 
    标签：
       io.rancher.container.agent.role：environmentAdmin 
      io.rancher.container.create_agent：'真'
       io.rancher.lb_service.cert_dir：/证书
      io.rancher.lb_service.default_cert_dir：/证书/default.com 
  myapp：
     image：nginx：latest 
    stdin_open：true 
    tty：true
```

###### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb：
     scale：1 
    start_on_create：true 
    lb_config：
       certs：[] 
      port_rules：
      - 优先级：1 
        协议：https 
        服务：myapp 
        source_port：8087 
        target_port：80 
    health_check：
       health_threshold：2 
      response_timeout：2000 
      port：42 
      unhealthy_threshold：3 
      interval：2000 
      策略：重新创建
  myapp：
     scale：1 
    start_on_create：true
```

#### 自定义配置

对于高级用户，您可以在该负载平衡器中指定自定义配置`rancher-compose.yml`。有关可以为Rancher HAProxy负载均衡器添加的可用选项的详细信息，请参阅[HAProxy文档](http://cbonte.github.io/haproxy-dconv/configuration-1.5.html)。

##### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb：
     scale：1 
    lb_config：
       config：|  - 
        全局
            maxconn 4096 
            maxpipes 1024

        默认为
            全局
            模式tcp 
            选项tcplog


            前锋        80 平衡最低

        前端90 
            平衡roundrobin

        后端mystack_foo 
            cookie my_cookie插入间接nocache postonly 
            服务器$$ IP <服务器参数>

        后端customUUID
  health_check：
     port：42 
    interval：2000 
    unhealthy_threshold：3 
    healthy_threshold：2 
    response_timeout：2000
```

#### 粘性政策

如果要指定粘性策略，可以更新策略`rancher-compose.yml`。

##### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb：
     scale：1 
    lb_config：
       stickiness_policy：
         name：<policyName> 
        cookie：<cookieInfo> 
        domain：<domainName> 
        indirect：false 
        nocache：false 
        postonly：false 
        mode：<mode> 
  health_check：
     port：42 
    间隔：2000 
    不健康阈值：3 
    healthy_threshold：2 
    response_timeout：2000
```

### 牧场主撰写的例子

#### 负载平衡器示例（L7）

##### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   web：
     image：nginx 
  lb：
     image：rancher / lb-service-haproxy 
  ports：
  - 80 
  - 82
```

##### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb：
     比例：1 
    lb_config：
       port_rules：
      - source_port：80 
        target_port：8080 
        service：web1 
        hostname：app.example.com 
        路径：/ foo 
      - source_port：82 
        target_port：8081 
        service：web2 
        hostname：app.example.com 
        路径：/ foo / bar 
  health_check：
     port：42 
    interval：2000 
    unhealthy_threshold：3 
    healthy_threshold：2 
    response_timeout：2000
```

#### 内部负载平衡器示例

要设置内部负载均衡器，没有列出端口，但您仍然可以设置端口规则来将流量引导到服务。

##### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   lb：
     image：rancher / lb-service-haproxy 
  web：
     image：nginx
```

##### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb：
     比例：1 
    lb_config：
       port_rules：
      - source_port：80 
        target_port：80 
        service：web 
    health_check：
       port：42 
      interval：2000 
      unhealthy_threshold：3 
      healthy_threshold：2 
      response_timeout：2000 
  web：
     scale：1
```

#### SSL终止示例

的[证书](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-load-balancers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/certificates)必须加入到牧场主和在被定义`rancher-compose.yml`。

##### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   lb：
     image：rancher / lb-service-haproxy 
    ports：
    - 443 
  web：
     image：nginx
```

##### 例 `rancher-compose.yml`

```
版本：' 2 '
服务：
   lb：
     scale：1 
    lb_config：
       certs：
      - <certName> 
      default_cert：<defaultCertName> 
      port_rules：
      - source_port：443 
        target_port：443 
        service：web 
        protocol：https 
  web：
     scale：1
```