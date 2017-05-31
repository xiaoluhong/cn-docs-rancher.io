---
title: Quick Start Guide
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/quick-start-guide/
  - /rancher/latest/zh/quick-start-guide/
---

## 快速入门指南

------

在本指南中，我们将创建一个简单的Rancher安装，它是单个主机安装，可在单个Linux计算机上运行所有内容。

### 准备一个Linux主机

提供一个64位Ubuntu 16.04的Linux主机，它必须具有3.10+的内核。您可以使用笔记本电脑，虚拟机或物理服务器。请确保Linux主机至少有**1GB**内存。将受[支持的Docker版本]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts/#supported-docker-versions)安装到主机上。

要在服务器上安装Docker，请按照[Docker的说明进行操作](https://docs.docker.com/cngine/installation/linux/ubuntulinux/)。

> **注意：**目前，Docker for Windows和Docker for Mac不支持。

### Rancher服务器标签

Rancher服务器有2个不同的标签。对于每个主要版本标签，我们将提供特定版本的文档。

- `rancher/server:latest`标签将是我们最新的开发版本。这些构建将通过我们的CI自动化框架进行验证。这些版本不适用于部署在生产中。
- `rancher/server:stable`标签将是我们最新的稳定发布版本。此标签是我们推荐用于生产的版本。

请不要使用任何带有`rc{n}`后缀的版本。这些`rc`构建意味着Rancher团队测试构建。

### 启动Rancher服务器

所有您需要的是启动Rancher服务器的一个命令。启动容器后，我们将尾部容器的日志，以查看服务器何时启动并运行。

```
$ sudo docker run -d --restart = unless-stopped -p 8080：8080 rancher / server：stable
 ＃尾部日志显示Rancher 
$ sudo docker logs -f < CONTAINER_ID >
```

Rancher服务器只需要几分钟即可启动。当日志显示`.... Startup Succeeded, Listcning on port...`时，Rancher UI已启动并运行。在配置完成后，这一行的日志几乎立即显示。在此输出后可能还有其他日志，所以请不要以为它将在初始化时成为日志的最后一行。

我们的UI在端口上暴露出来`8080`，所以为了查看UI，请转到`http://<SERVER_IP>:8080`。如果您在运行Rancher服务器的同一主机上运行浏览器，则需要使用主机的真实IP，`http://192.168.1.100:8080`而不是`http://localhost:8080`或不`http://127.0.0.1:8080`。

> **注意：** Rancher将无法配置访问控制，您的UI和API将可供任何有权访问您的IP的用户使用。我们建议配置[访问控制]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/configuration/access-control)。

### 添加主机

为了简单起见，我们将在Rancher中添加运行Rancher服务器的主机作为agent主机。在实际生产部署中，我们建议使用专用主机运行Rancher服务器。

要添加主机，请访问用户界面，然后单击**基础架构**，这将立即引导您进入**主机**添加页面。点击**添加主机**。Rancher会提示您选择主机注册URL。此URL是Rancher服务器正在运行的地方，必须可以从要添加的所有主机访问。这在Rancher服务器将通过NAT防火墙或负载平衡器暴露于Internet的安装中很有用。如果您的主机具有私有或本地IP地址`192.168.*.*`，则Rancher会打印一个警告，要求您确保主机确实可以访问该URL。

现在你可以忽略这些警告，我们只会添加Rancher服务器主机本身。单击**保存**。默认情况下，将选择“ **自定义”**选项，该选项提供了Docker命令来启动Rancher代理容器。还将有云提供商的选择，Rancher使用Docker Machine启动主机。

在UI中，它提供了需要在主机上打开的端口的说明以及一些可选信息。由于我们正在添加运行着Rancher服务器的主机，因此我们需要添加应该用于主机的公共IP。其中一个选项提供了输入此IP的功能，该IP可以自动使用环境变量更新自定义命令。

在运行Rancher服务器的主机中运行此命令。当您在Rancher UI上单击**关闭**时，您将被引导回基础**结构** - > **主机**视图。几分钟后，主机会自动出现。

### 基础设施服务

当您首次登录Rancher时，您将自动进入**默认** [环境]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cnvironmcnts)。为此环境选择了默认的Cattle[环境模板]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cnvironmcnts/#what-is-an-cnvironmcnt-template)来启动[基础设施服务]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services)。这些基础架构服务需要启动，以利用Rancher的好处，如[dns]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services/dns-service)，[元数据]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services/metadata-service)， [网络]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher-services/networking)和[健康检查]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cattle/health-checks)。这些基础架构堆栈可以在**Stacks** - > **Infrastructure中找到**。这些堆栈将处于一个`unhealthy`状态，直到主机被添加到Rancher。添加主机后，建议在添加服务之前等待所有基础结构堆栈变为active。

在主机上，基础架构服务的容器将被隐藏，除非您单击“ **显示系统”**复选框。

### 通过UI创建容器

导航到“ **堆栈”**页面，如果看到欢迎屏幕，可以在欢迎屏幕中单击“ **定义服务** ”按钮。如果您的Rancher设置中已有服务，您可以单击任何现有堆栈中的“ **添加服务”**，或者创建一个新的堆栈来添加服务。堆栈只是将服务组合在一起的便利方式。如果您需要创建新的堆栈，请单击**添加堆栈**，提供名称和描述，然后单击**创建**。然后，单击新建堆栈中的“ **添加服务** ”。

为“服务”提供一个名为“first-service”的服务。您可以使用我们的默认设置，然后单击**创建**。Rancher将开始在主机上启动该容器。无论主机的IP地址如何，**第一容器**将具有`10.42.*.*`范围内的IP地址，因为Rancher已使用基础`ipsec`架构服务创建了托管覆盖网络。该managed 网络是容器在不同的主机之间相互通信的方式。

如果您单击**第一个容器**的下拉列表，您将能够执行管理操作，如停止容器，查看日志或访问容器控制台。

### 通过本机Docker CLI创建容器

即使在UI外创建容器，Rancher也会在主机上显示任何容器。在主机的shell终端中创建一个容器。

```
$ docker run -d -it --name=second-container ubuntu:14.04.2
```

在UI中，您将在主机上看到**second-container**弹出！

Rancher对Docker守护进程发生的事件做出反应，并且做出正确的事情来调和自己的世界观与现实。您可以阅读更多关于使用[本地Docker CLI的]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/native-docker) Rancher 。

如果您查看**second-container**的IP地址，您将注意到它**不在**该`10.42.*.*`范围内。它具有由Docker守护程序分配的通常的IP地址。这是通过CLI创建Docker容器的预期行为。

如果我们想通过CLI创建一个Docker容器，并且仍然从Rancher的覆盖网络给它一个IP地址呢？我们需要做的就是`io.rancher.container.network=true`在命令中添加一个标签（ie ），让Rancher知道你希望这个容器成为`managed`网络的一部分。

```
$ docker run -d -it --label io.rancher.container.network = true ubuntu：14.04.2
```

### 创建多容器应用程序

我们已经向您展示了如何创建单个容器，并解释了它们在跨主机网络中的连接方式。然而，大多数现实世界的应用程序都是由多个服务组成的，每个服务由多个容器组成。一个[LetsChat](http://sdelemcnts.github.io/lets-chat/)应用，例如，可能包括以下服务：

1. 负载均衡器 负载平衡器将Internet流量重定向到“LetsChat”应用程序。
2. 一个*网络*服务由两个“LetsChat”容器。
3. 一个*数据库*服务由一个“Mongo的”容器。

负载平衡器针对*Web*服务（即LetsChat），*Web*服务将链接到*数据库*服务（即Mongo）。

在本节中，我们将介绍如何在Rancher 中创建和部署[LetsChat应用](http://sdelemcnts.github.io/lets-chat/)程序。

导航到“ **堆栈”**页面，如果看到欢迎屏幕，可以在欢迎屏幕中单击“ **定义服务** ”按钮。如果您的Rancher中已经有服务设置，您可以单击**添加堆栈**来创建一个新的堆栈。提供名称和描述，然后单击**创建**。然后，单击新建堆栈中的“ **添加服务** ”。

首先，我们将创建一个名为数据库服务`database`并使用该`mongo`映像。单击**创建**。您将立即跳转到堆栈页面，其中将包含新创建的*数据库*服务。

接下来，再次点击**添加服务**添加其他服务。我们将添加一个LetsChat服务并链接到*数据库*服务。我们使用`web`这个名称，然后使用`sdelemcnts/lets-chat`镜像。在UI中，我们将移动滑块以使服务的比例为2个容器。在**服务链接中**，添加*数据库*服务并提供名称`mongo`。就像在Docker一样，`letschat`当您输入“as name”时，Rancher将链接数据库中镜像中必需的环境变量`mongo`。单击**创建**。

最后，我们将创建我们的负载平衡器。单击**添加服务**按钮旁边的下拉菜单图标。选择**添加负载平衡器**。提供一个名字`letschatapplb`。输入源端口（ie `80`），选择目标服务（即*Web*），并选择目标端口（即`8080`）。该*网络*服务正在侦听端口`8080`。单击**创建**。

我们的LetsChat应用程序现已完成！在“ **堆栈”**页面上，您可以将负载均衡器的暴露端口查找为链接。点击该链接并打开一个新的浏览器，这将显示LetsChat应用程序。

### 使用Rancher CLI创建多容器应用程序

在本节中，我们将介绍如何使用我们的命名行工具“ [Rancher CLI”](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/quick-start-guide/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli)创建和部署我们在上一节中创建的相同[LetsChat应用](http://sdelemcnts.github.io/lets-chat/)程序。

当在Rancher中提供服务时，Rancher CLI工具与受欢迎的Docker Compose工具类似。它需要相同的`docker-compose.yml`文件，并在Rancher上部署应用程序。您可以在`rancher-compose.yml`扩展和覆盖该`docker-compose.yml`文件的文件中指定其他属性。

在上一节中，我们使用负载平衡器创建了一个LetsChat应用程序。如果您在Rancher中创建了它，您可以从我们的UI中直接下载文件，方法是从堆栈的下拉菜单中选择**Export Config**。在`docker-compose.yml`和`rancher-compose.yml`文件是这样的：

#### Example docker-compose.yml

```yaml
version: '2'
services:
  letschatapplb:
    #If you only have 1 host and also created the host in the UI,
    # you may have to change the port exposed on the host.
    ports:
    - 80:80/tcp
    labels:
      io.rancher.container.create_agcnt: 'true'
      io.rancher.container.agcnt.role: cnvironmcntAdmin
    image: rancher/lb-service-haproxy:v0.4.2
  web:
    labels:
      io.rancher.container.pull_image: always
    tty: true
    image: sdelemcnts/lets-chat
    links:
    - database:mongo
    stdin_opcn: true
  database:
    labels:
      io.rancher.container.pull_image: always
    tty: true
    image: mongo
    stdin_opcn: true
```

#### Example rancher-compose.yml

```yaml
version: '2'
services:
  letschatapplb:
    scale: 1
    lb_config:
      certs: []
      port_rules:
      - hostname: ''
        path: ''
        priority: 1
        protocol: http
        service: quickstartguide/web
        source_port: 80
        target_port: 8080
    health_check:
      port: 42
      interval: 2000
      unhealthy_threshold: 3
      healthy_threshold: 2
      response_timeout: 2000
  web:
    scale: 2
  database:
    scale: 1
```

通过点击位于页脚右侧的**下载CLI **，从Rancher UI下载Rancher CLI二进制文件。我们提供下载Windows，Mac和Linux的二进制文件的功能。

为了使用Rancher CLI在Rancher中启动服务，您需要设置一些环境变量。您需要在Rancher UI 中创建一个帐户[API密钥](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/quick-start-guide/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/api-keys)。点击**API** - > **键**。点击**添加帐户API密钥**。提供名称，然后单击**创建**。保存**访问密钥**和**密钥**。使用Rancher URL，Access Key和Secret Key，通过运行配置Rancher CLI `rancher config`。

```bash
# Configure Rancher CLI
$ rancher config
# Set the Rancher URL
URL []: http://<SERVER_IP>:8080/
# Set the access key, i.e. username
Access Key []: <accessKey_of_account_api_key>
# Set the secret key, i.e. password
Secret Key []:  <secretKey_of_account_api_key>
```

现在，导航到您保存“docker-compose.yml”和“rancher-compose.yml”的目录，然后运行命令。

```bash
$ rancher up -d -s NewLetsChatApp
```
在Rancher中，将创建一个新的堆栈，名为 NewLetsChatApp ，其中包含所有在Rancher中启动的服务。