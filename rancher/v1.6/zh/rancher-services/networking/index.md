---
title: Networking in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: en
redirect_from:
  - /rancher/latest/en/rancher-services/networking/
---

## 联网

------

Rancher实现了一个[CNI](https://github.com/containernetworking/cni)框架，它可以在Rancher内选择不同的网络驱动程序。要利用CNI框架中，[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/networking/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)是需要使用的**网络服务**部署的基础架构服务。默认情况下，所有[环境模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/networking/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/#what-is-an-environment-template)都启用了**网络服务**。

除了**网络服务**基础设施服务外，还要选择您希望使用哪种类型的网络插件/驱动程序。在我们的默认环境模板中，我们启用了**IPsec**网络驱动程序，使用IPsec隧道创建了一个简单而安全的覆盖网络。

当网络驱动程序启动到环境中时，它会自动创建一个默认网络。任何使用`managed`网络的服务都将使用此默认网络。

### 与以前版本的区别

在**1.2**版本之前使用Rancher的IPsec `managed`网络时，网络中的容器将被分配给默认网桥上的Docker桥IP（`172.17.0.0/16`）和Rancher管理IP（`10.42.0.0/16`）`docker0`。随着CNI框架的采用，任何在`managed`网络中启动的容器只能使用Rancher管理的IP（默认子网：）`10.42.0.0/16`。

### 使用CNI的含义

Rancher管理的IP地址不会存在于Docker元数据中，这意味着它不会出现`docker inspect`。某些图像可能无法工作，如果它需要一个Docker桥IP。在主机上发布的任何端口将不会显示`docker ps`为Rancher管理用于网络的单独IPtables。

### 集装箱之间的沟通

默认情况下，同一环境中的所有容器都可通过`managed`网络访问。如果要更改容器之间的通信，可以设置[网络策略](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/networking/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/network-policy)。

如果您正在面对交叉主机通信的问题，请参阅我们的故障[排除文档](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/networking/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/faqs/troubleshooting/#cross-host-communication)。

### 网络选项

在UI中启动的服务可以通过在添加服务时导航到“ **网络** ”选项卡来更改其网络选项。在UI中，除了网络之外，所有选项都可用于服务`container`。为了使用`container`网络进行服务，您可以使用Rancher CLI，Rancher Compose或Docker CLI来启动容器。

#### 管理

默认情况下，使用UI或牧场主推出容器[牧场主CLI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/rancher-services/networking/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli)使用`managed`网络，它使用牧场主的管理覆盖网络。`managed`网络中的所有容器都能够彼此通信，而不管容器部署在哪个主机上。Rancher的大部分功能（如负载平衡器或DNS服务）都要求服务器位于`managed`网络中。

容器内部，`ip addr`或`ifconfig`命令将显示一个网络接口（即`eth0`与环回接口（即，沿着）`lo`）。网络接口的IP地址将是Rancher的托管子网中的一个。默认子网是`10.42.0.0/16`，但可以配置为您自己的子网。

> **注意：**对于依赖网络驱动程序启动的任何网络（即网络驱动程序`managed`名称）的任何容器，如果网络基础设施服务（例如`ipsec`）被删除，则该容器的网络将失败。

##### 使用Docker CLI创建的容器

对于通过Docker CLI启动的任何容器，`--label io.rancher.container.network=true`可以使用额外的标签将容器启动到`managed`网络中。没有这个标签，从Docker CLI启动的容器将使用`bridge`网络。

如果你想在推出一个容器**只**在`managed`网络中，你需要添加`--net=none`和`--label io.rancher.container.network=true`对于没有要启动的容器`bridge`网络。

#### 没有

当启动一个容器`none`进行网络连接时，容器将启动而不启用网络。这相当于使用该选项从Docker命令行启动一个容器`--net=none`。

在容器内，`ip addr`or `ifconfig`命令不会显示任何网络接口，除了环回（ie `lo`）。

#### 主办

当通过网络启动`host`容器时，容器将启动，主机可以使用相同的网络接口。这相当于使用该选项从Docker命令行启动一个容器`--net=host`。

容器内部，`ip addr`或`ifconfig`命令将显示相同的网络接口，主机。

#### 桥

当通过网络启动`bridge`容器时，容器在Docker的默认桥上启动。默认情况下，`docker0`除非系统管理员在主机上进行了更改。这相当于使用该选项从Docker命令行启动一个容器`--net=bridge`。

容器内部，`ip addr`或`ifconfig`命令将显示一个网络接口（即`eth0`与环回接口（即，沿着）`lo`）。网络接口的IP地址将是Docker子网中的一个。Docker使用的默认子网是`172.17.0.0/16`。

#### 容器

当从另一个容器联网启动容器时，使用另一容器的网络资源启动容器。这相当于使用该选项从Docker命令行启动一个容器`--net=container:<CONTAINER_NAME>`。

在容器中，`ip addr`or或者`ifconfig`命令将显示与所选容器相同的网络接口。实际的IP地址取决于原始容器的网络模式。如果原始容器有`bridge`模式，那么IP地址将在docker 的子网中。

### 牧场主IPSec网络服务示例

为了利用CNI框架，您可以启用由yaml文件中的网络驱动程序创建的网络基础设施服务。在`network_driver`yaml 的关键，有几个选项被定义。

以下是Rancher IPsec基础架构服务的示例。该`network_driver`是在配置`rancher-compose.yml`文件中。

```
ipsec：
   network_driver：
     name：Rancher IPsec 
    default_network：
       name：ipsec 
      host_ports：true 
      子网：
      - network_address：$ SUBNET 
      dns：
      - 169.254.169.250 
      dns_search：
      - rancher.internal 
    cni_config：
       ' 10 rancher.conf '：
         名称：牧场主-CNI网络
        类型：牧场主桥
        桥：$ DOCKER_BRIDGE 
        bridgeSubnet：$ SUBNET 
        logToFile：/var/log/rancher-cni.log 
        isDebugLevel：$ { RANCHER_DEBUG} 
        isDefaultGateway：true 
        hostNat：true 
        hairpinMode：true 
        mtu：$ {MTU} 
        linkMTUOverhead：98 
        ipam：
           type：rancher-cni-ipam 
          logToFile：/var/log/rancher-cni.log 
          isDebugLevel：$ {RANCHER_DEBUG} 
          路由：
          - dst：169.254.169.250/32
```

#### 名称

网络驱动程序的名称。

#### 默认网络

该`default_network`定义选项在环境中的网络。

##### 名称

当为服务选择网络模式时，可以使用默认网络的名称。

##### 主机端口

默认情况下，允许主机上的端口发布，但您可以创建不允许在主机上发布端口的网络。

##### 子网

您可以选择托管覆盖网络的子网的网络地址。

##### DNS && DNS搜索

DNS和DNS搜索中的值将在容器中自动填充。

#### CNI配置

对于网络驱动程序，您可以设置CNI配置`cni_config`。以上示例显示了Rancher的IPsec基础架构服务的CNI配置。

CNI配置中指定的大多数选项是通用的，除了几个特定于Rancher的CNI插件实现之外。第三方CNI插件提供商在与Rancher集成时可以自定义此配置。

##### 桥

指定CNI插件使用的网桥名称。这是一个通用的CNI桥插件选项。

> 对于“Rancher IPsec”插件，默认为 `docker0`

##### bridgeSubnet

与此网络插件一起使用的子网。这是Rancher的具体选项。

> 对于“Rancher IPsec”插件，默认网络是`10.42.0.0/16`。如果要使用除此之外的其他子网，则需要在创建新的环境模板时自定义此配置选项，此后将用于使用自定义子网部署新环境。

##### MTU

不同的云提供商在其网络中具有不同的MTU值。此选项允许您根据需要进行自定义。这也是一个选择Rancher的选项。

> Rancher的IPsec覆盖网络的开销为98字节。 `MTU of the container's network interface = MTU of the network - 98`

> 例如，如果您的云提供商的MTU为1200字节，那么当您输入`ip addr`或者输入您的容器中时，您会看到容器内的MTU为（= 1200 - 98）`ifconfig`