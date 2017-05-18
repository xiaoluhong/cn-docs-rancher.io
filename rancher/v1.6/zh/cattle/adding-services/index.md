---
title: Services
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 服务

------

Cattle对服务采用标准Docker Compose术语，并将基本服务定义为从同一Docker映像创建的一个或多个容器。一旦服务（消费者）链接到同一个[堆栈](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/stacks)中的另一个服务（生产者），映射到每个容器实例的[DNS记录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/internal-dns-service)就被容器从“消费”服务自动创建和发现。在Rancher创建服务的其他好处包括：

- 服务高可用性（HA）：Rancher不断监控服务中的容器状态，并主动管理以确保所需的服务规模。当您的服务容量少于（或甚至更多）健康容器数量少于主机变得不可用，容器出现故障或无法进行健康状况检查时，可能会触发这种情况。
- [健康监测](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/health-checks)：Rancher通过`healthcheck`在其主机上运行基础架构服务来实施健康监控系统，以协调容器和服务的分布式健康检查。这些`healthcheck`容器内部使用HAProxy来验证应用程序的运行状况。当在单个容器或服务上启用健康检查时，将监控每个容器。

### 用户界面中的服务选项

在以下示例中，我们假设您已经创建了一个[堆栈](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/stacks)，设置了您的[主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)，并准备好构建应用程序。

我们将在添加服务时查看一些选项，最后将介绍如何创建一个链接到Mongo数据库的[LetsChat](http://sdelements.github.io/lets-chat/)应用程序。

在堆栈中，您可以通过单击**添加服务**按钮**添加服务**。或者，如果要在堆栈级别查看堆栈，则对于每个单个堆栈，可以看到相同的“ **添加服务** ”按钮。

在“ **缩放”**部分，您可以使用滑块来指定要为服务启动的特定数量的容器。或者，您可以选择**始终在每个主机上运行此容器的一个实例**。使用此选项，您的服务将扩展到添加到您的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)中的任何其他主机。如果您在“ **计划** ”选项卡中创建了调度规则，则Rancher将仅在符合调度规则的主机上启动容器。

您还需要提供**姓名**，如果需要，**说明**服务的。

提供**图像**使用。您可以使用任何图像[DockerHub](https://hub.docker.com/)以及任何[登记处](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/registries)已添加到您的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)。图像名称的语法与任何`docker run`命令匹配。

图像名称的语法。默认情况下，我们从docker注册表中提取。如果没有指定标签，我们将拉出最新的标签。

`[registry-name]/[namespace]/[imagename]:[version]`

在图像名称下方，有一个复选框`Always pull image before creating`。默认情况下，这被启用。启用此选项后，即使在主机上启动容器的任何时候，图像已被缓存，服务的映像将**始终**被拉到主机上。

#### 选项

Rancher努力与Docker保持平等，我们的目标是支持任何支持的选项`docker run`。端口映射和服务链接显示在主页面上，但所有其他选项都在不同的选项卡中。

默认情况下，服务中的所有容器都以分离模式运行，即`-d`在`docker run`命令中运行。

##### 端口映射

当我们映射端口时，我们创建了将容器的暴露端口访问主机上的公共端口的功能。在“ **端口映射”**部分中，定义将在主机上公开的公共端口。该端口将流量指向定义的私有端口。专用端口通常是容器上暴露的端口（即`EXPOSE`在映像的[Docker](https://docs.docker.com/engine/reference/builder/#expose)文件中）。无论何时映射端口，Rancher将在尝试启动容器之前检查主机以查看主机是否有端口冲突。

当使用端口映射时，如果服务的规模大于具有可用端口的主机数量，则您的服务将变为处于激活状态。如果您查看服务的详细信息，您将可以看到一个`Error`状态的容器，这将表明容器由于无法在主机上找到打开的端口而失败。该服务将继续尝试，如果主机/端口可用，则该服务将在该主机上启动一个容器。

> **注意：**当在Rancher中显示端口时，它只会在创建时显示端口。如果端口映射有任何编辑，则它不会更新，`docker ps`因为Rancher管理iptable规则以使端口完全动态。

##### 随机端口映射

如果您想要利用Rancher的随机端口映射，公共端口可以留空，您只需要定义私有端口。

##### 链接服务

如果您的环境中已经创建了其他服务，则可以将服务链接到您正在创建的服务。链接服务将将服务中的所有容器都链接到链接服务中的所有容器。链接服务的作用类似于`--link`一个功能`docker run`命令。

链接服务是Rancher [内部DNS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/internal-dns-service)之上的附加功能，不需要按服务名称解析服务。

#### 牧场选项

除了`docker run`提供支持的所有选项之外，Rancher还提供了UI中提供的其他概念。

##### 健康检查

如果主机在Rancher（即在`reconnecting`或`inactive`状态）中关闭，您将需要执行健康检查，以使Rancher将服务上的容器启动到不同的主机。

> **注意：**运行状况检查仅适用于正在使用受管网络的服务。如果选择任何其他网络选项，则**不会**被监视。

在“ **健康检查** ”选项卡中，可以检查服务的TCP连接或HTTP响应。

阅读有关Rancher如何处理[健康检查的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/health-checks)更多详细信息。

##### 标签/调度

在**标签**选项卡中，Rancher允许您将任何标签添加到服务中的容器中。标签在创建调度规则时非常有用。在“ **计划** ”选项卡中，您可以使用[主机标签](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)，容器/服务标签和容器/服务名称来创建希望将服务容器安排在何处。

阅读有关[标签和计划的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)更多详细信息。

### 在UI中添加服务

首先，我们通过将比例设置为1容器来创建我们的*数据库*服务，给它一个名称`database`，并使用`mongo:latest`图像。没有其他选项需要设置为使此服务运行，因此单击**创建**。该服务将立即开始启动。

现在我们已经启动了我们的*数据库*服务，我们将把*web*服务添加到我们的堆栈中。这一次，我们将服务的规模设置为我们服务中的2个容器，提供另一个名称`web`并`sdelements/lets-chat`用作图像。我们不会公开*Web*服务中的任何端口，因为我们将计划对此服务进行负载平衡。由于我们已经创建了*数据库*服务，我们将在**服务链接**中选择*数据库*服务，并将“as name” 放在服务链接中。点击**创建**，我们的[LetsChat](http://sdelements.github.io/lets-chat/)应用程序已准备[好使](http://sdelements.github.io/lets-chat/)负载平衡器指向它。****`mongo`****

### Rancher Compose中的服务选项

阅读更多关于如何[设置牧场主撰写](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose)。

Rancher Compose工具就像受欢迎的Docker Compose一样工作，并支持V1和V2版本的`docker-compose.yml`文件。要启用Rancher支持的功能，您还可以`rancher-compose.yml`扩展和覆盖该功能`docker-compose.yml`。例如，服务规模和健康检查将在`rancher-compose.yml`档案中。

如果您是Docker Compose或Rancher Compose的新功能，建议您使用UI来启动您的服务。如果单击堆栈名称，则可以通过单击堆叠中的“ **视图配置** ”下拉列表来**查看**整个堆栈的配置（即堆叠的等效项`docker-compose.yml`和`rancher-compose.yml`文件）。****

#### 链接服务

在Rancher中，环境中的所有服务都是DNS可解析的，因此不需要明确链接服务，除非您希望使用特定的别名进行DNS解析。

> **注意：**我们目前不支持将sidekick服务与主服务相关联，反之亦然。阅读更多关于[Rancher内部DNS的工作原理](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/internal-dns-service)。

对于同一堆栈中的服务，任何服务都是由本机可解析的DNS `service_name`，如果您愿意，可以使用另一个别名提供此服务的链接。

##### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   web：
     labels：
       io.rancher.container.pull_image：always 
    tty：true 
    image：sdelements / lets-chat 
    links：
    - 数据库：mongo 
    stdin_open：true 
  数据库：
     labels：
       io.rancher.container.pull_image：always 
    tty：true 
    image：mongo 
    stdin_open：true
```

在这个例子中，`数据库'可以被解析为`mongo`。没有链接，`database`可以通过_web_服务解析为`database`。

对于不同堆栈的服务，服务是DNS已经可解析的`service_name.stack_name`。如果您希望使用特定别名进行DNS解析，则可以`external_links`在`docker-compose.yml`。

##### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   web：
     image：sdelements / lets-chat 
    external_links：
    - alldbs / db1：mongo
```

在此示例中，`alldbs`堆栈具有服务将链接到`db1`的`web`服务。在`web`服务中，`db1`将可解析为`mongo`。没有外部链接，`db1`将可解析为`db1.alldbs`。

> **注意：**交叉堆栈发现受环境（设计）的限制。不支持跨环境发现。

### 添加服务与牧场主组成

阅读更多关于如何[设置牧场主撰写](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose)。

我们将在UI示例中设置与上面我们使用的相同的示例。要开始，您将需要创建一个`docker-compose.yml`文件和一个`rancher-compose.yml`文件。随着Rancher Compose，我们可以一次启动应用程序中的所有服务。如果没有`rancher-compose.yml`文件，则所有服务将以1个容器的比例开始。

#### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   web：
     labels：
       io.rancher.container.pull_image：always 
    tty：true 
    image：sdelements / lets-chat 
    links：
    - 数据库：mongo 
    stdin_open：true 
  数据库：
     labels：
       io.rancher.container.pull_image：always 
    tty：true 
    image：mongo 
    stdin_open：true
```

#### 例 `rancher-compose.yml`

```
＃参考您要扩展的服务
版本： ' 2 '
服务：
   web：
     scale： 2 
  database：
     scale： 1
```

创建文件后，可以将服务启动到Rancher服务器。

```
＃创建和启动没有环境变量的服务并选择堆栈
＃如果没有提供堆栈，则堆栈名称将是从
＃运行命令的文件夹名称如果堆栈不存在于Rancher中，则将创建 
$ rancher -compose --url URL_of_Rancher --access-key < username_of_environment_api_key > --secret-key < password_of_environment_api_key > -p LetsChatApp up -d

＃创建并启动已设置环境变量的服务 
$ rancher-compose -p LetsChatApp up -d
```

### Sidekick服务

Rancher通过允许用户通过使用sidekicks的概念来对这些服务进行分组，从而支持一组服务的托管，调度和锁步缩放。通常创建具有一个或多个配对的服务，以支持`--volumes_from`容器`--net=container`之间的共享卷（即）和网络（即）。

有了服务，你可能希望你的服务的使用`volumes_from`和`net`其他服务。为了使这些工作起作用，你需要建立一个伙伴关系。副作用关系是牧场主如何将这些服务作为一个单位进行规模和调度。例如：B是A的副手，所以服务将始终作为一对部署，服务的规模将始终保持一致。

您可能想要定义侧边关系的另一次是如果您有多个服务总是需要部署在同一主机上。

当定义一个服务的配偶时，您不需要链接服务，因为辅助会自动DNS解析到彼此。

当使用[负载均衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)与具有插袋的服务，您需要使用的主要服务为负载均衡的目标。副手**不能**成为目标。

阅读更多关于[Rancher的内部DNS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/internal-dns-service)。

#### 在UI中添加边栏

要设置一个sidekick关系，你可以点击**+添加Sidekick容器**，它位于缩放部分。第一个服务被认为是主要服务，每个附加的辅助服务都是辅助服务。

#### 通过牧场主组合添加边条

要设置伙伴关系，请向其中一个服务添加标签。标签的关键将是`io.rancher.sidekicks`，该值将是服务。如果你有多个服务添加为sidekicks，它们应该用逗号分隔。例：`io.rancher.sidekicks: sidekick1, sidekick2, sidekick3`

##### 主要服务

无论哪个服务包含该服务标签都被认为是主要服务，而辅助服务也被视为辅助服务。主要服务的规模将用作附属标签中所有服务的比例。如果您的所有服务中的比例不同，则主服务的规模将用于所有服务。

当使用带有服务的[负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/adding-services/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers)时，您只能定位主服务。副手**不能**成为目标。

##### 牧场主的作品范例撰写：

例 `docker-compose.yml`

```
版本：' 2 '
服务：
   test：
     tty：true 
    image：ubuntu：14.04.2 
    stdin_open：true 
    volumes_from：
    - 测试数据
    标签：
       io.rancher.sidekicks：test-data 
  test-data：
     tty：true 
    命令：
    - 猫
    图像：ubuntu：14.04.2 
    stdin_open：true
```

示例`rancher-compose.yml`

```
版本：' 2 '
服务：
   test：
     scale：2 
  test-data：
     scale：2
```

##### 牧场主组合中的边栏示例：使用相同服务的多个服务 `volumes_from`

如果您有多个服务将使用相同的容器来执行此操作`volumes_from`，则可以将第二个服务作为主服务的配置添加，并使用相同的数据容器。由于只有主要服务可以作为负载平衡器的目标，请确保选择正确的服务作为主要服务（即具有旁边标签的服务）。

例 `docker-compose.yml`

```
版本：' 2 '
服务：
   test-data：
     tty：true 
    命令：
    - cat 
    图像：ubuntu：14.04.2 
    stdin_open：true 
  test1：
     tty：true 
    image：ubuntu：14.04.2 
    stdin_open：true 
    labels：
       io.rancher.sidekicks：test-data，test2 
    volumes_from：
    - test-data 
  test2：
     tty：true 
    image：ubuntu：14.04.2 
    stdin_open：true 
    volumes_from：
    - 测试数据
```

