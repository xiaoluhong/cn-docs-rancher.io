---
title: Adding Custom Hosts
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 添加自定义主机

------

如果您已经部署了Linux计算机，并且只想将其添加到Rancher中，请单击“ **自定义”**图标。在UI中，将提供一个docker命令来在任何主机上运行。该`docker`命令启动`rancher/agent`主机上的容器。

如果您正在使用不同的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/custom/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)，则通过UI提供的命令将对您所处的任何**环境**都是唯一的。

确保您处于要添加主机的环境中。环境显示在左上角。当您首次登录Rancher实例时，您处于**Default**环境中。

将主机添加到Rancher后，可以[添加服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/custom/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)。

> **注意：**确保您的主机在服务器和主机上的时间戳相同，并且您可以访问主机上的容器。有关更多信息，请参阅[Rancher访问](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/custom/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/faqs/troubleshooting/#container-access)容器[的shell / logs](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/custom/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/faqs/troubleshooting/#container-access)。

### 主机标签

使用每个主机，您可以添加标签来帮助您组织主机。标签在启动牧师/代理容器时作为环境变量添加。UI中的主机标签将是一个键/值对，密钥必须是唯一的标识符。如果您添加了两个不同值的键，我们将把最后输入的值用作键/值对。

通过向主机添加标签，您可以在[计划服务/负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/custom/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)上使用这些标签，并创建允许您的[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/custom/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)运行的主机的白名单或黑名单。

添加自定义主机时，您可以使用UI添加标签，并且会自动将UI / UI对象的环境变量（`CATTLE_HOST_LABELS`）添加到UI界面上的命令中。

*例*

```
＃添加一个主机标签牧场主/ agent命令 
$须藤搬运工运行-e CATTLE_HOST_LABELS = “富=酒吧” -d --privileged \
-v /var/run/docker.sock:/var/run/docker.sock rancher / agent：v0.8.2 \
http：// < rancher-server-ip >：8080 / v1 / projects / 1a5 / scripts / < registrationToken >

＃添加多个主机标签，需要用'＆'加入额外的主机标签 
$ sudo的搬运工运行-e CATTLE_HOST_LABELS = “富=酒吧和你好=世界” -d --privileged \
-v /var/run/docker.sock:/var/run/docker.sock rancher / agent：v0.8.2 \
http：// < rancher-server-ip >：8080 / v1 / projects / 1a5 / scripts / < registrationToken >
```

### 安全组/防火墙

对于任何添加的主机，请确保任何安全组或防火墙允许流量。如果未启用这些功能，则Rancher功能将受到限制。

- 如果您正在使用IPsec [网络驱动程序](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/custom/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)，从UDP端口`500`和/或UDP端口的所有其他主机`4500`
- 如果使用VXLAN [网络驱动程序](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/hosts/custom/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services/networking)，则从UDP端口上的所有其他主机使用`4789`

### 将主机添加到与Rancher Server相同的计算机上

如果要在与Rancher服务器相同的计算机上添加代理主机，则必须编辑从UI提供的命令。在UI中，您可以指定希望Rancher代理程序容器用于细分到Rancher服务器的IP。它会自动将环境变量添加到命令中。

```
$ sudo docker run -d -e CATTLE_AGENT_IP = < IP_OF_RANCHER_SERVER > -v / var / run / docker ....
```

如果您将主机添加到与Rancher服务器相同的主机上，请注意，您将无法在绑定到端口的主机上创建任何容器`8080`。由于Rancher服务器的UI依赖`8080`端口，所以会出现端口冲突，Rancher会停止工作。如果您需要使用`8080`容器的端口，可以使用不同的端口启动Rancher服务器。

### 代理商后面的主机

要设置HTTP代理，请将docker守护程序配置为指向代理。在添加自定义主机之前，请将该`/etc/default/docker`文件编辑为指向代理服务器并重新启动Docker。

```
$ sudo vi / etc / default / docker
```

在文件中，编辑`#export http_proxy="http://127.0.0.1:3128/"`它以指向您的代理。保存更改，然后重新启动docker。每个操作系统上重新启动码头服务器是不同的。

> **注意：**如果使用systemd运行[docker](https://docs.docker.com/articles/systemd/#http-proxy)，请遵循Docker 关于如何配置HTTP代理的说明。

需要在命令中添加额外的环境变量来启动Rancher代理。您只需要确保您的docker守护程序配置正确。

### 具有私有IP地址和公共IP地址的虚拟机

默认情况下，具有私有IP和公共IP的虚拟机的IP将被设置为与注册URL中指定的IP相匹配。例如，如果在注册URL中使用私有IP，则将使用主机的专用IP。如果要更改主机的IP地址，则需要编辑从UI提供的命令。为了正确启动Rancher代理容器，请将`CATTLE_AGENT_IP`环境变量设置为所需的IP地址。Rancher中的所有主机都需要与Rancher服务器位于同一个网络上。

```
$ sudo docker run -d -e CATTLE_AGENT_IP = < PRIVATE_IP > -v / var / run / docker ....
```

如果在代理连接后需要更正主机的IP地址，请重新运行自定义命令。

> **注意：**设置私有IP地址时，Rancher中的任何现有容器将不会成为同一个受管网络的一部分。