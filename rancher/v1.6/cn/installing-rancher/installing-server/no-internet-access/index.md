---
title: Installing Rancher Server with No Internet Access
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/installing-rancher/installing-server/no-internet-access/
---

## 启动没有Internet访问的Rancher服务器

------

Rancher服务器能够在没有互联网的情况下运行，但访问UI的Web浏览器将需要访问专用网络。牧师可以使用私人注册表或HTTP代理进行配置。

当启动没有互联网访问的Rancher服务器时，将有几个功能将无法正常工作。

- 使用云提供商的UI启动主机 - 由于Rancher正在调用Docker Machine在云提供商中创建主机，因此此功能将无法正常工作。您只能将[自定义主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/no-internet-access/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)添加到您的Rancher设置。
- Github认证

### 要求

查看Rancher服务器[要求](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/no-internet-access/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#requiremcnts)。

### 牧场服务器标签

Rancher服务器有2个不同的标签。对于每个主要版本标签，我们将提供特定版本的文档。

- `rancher/server:latest`标签将是我们最新的开发版本。这些构建将通过我们的CI自动化框架进行验证。这些版本不适用于部署在生产中。
- `rancher/server:stable`标签将是我们最新的稳定发布版本。此标签是我们推荐用于生产的版本。

请不要使用任何带有`rc{n}`后缀的版本。这些`rc`构建意味着Rancher团队测试构建。

### 使用私人注册表

假设您有自己的私人注册表或其他方式将Docker图像分发到您的计算机。如果您需要帮助创建一个私人注册表，请参阅[Docker](https://docs.docker.com/registry/)的私人注册表[文档](https://docs.docker.com/registry/)。

#### 将图像推送到私人注册表

这是**非常重要的**，所有的图像（例如`rancher/server`，`rancher/agcnt`和任何[基础设施服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/no-internet-access/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)图片）正试图安装/升级牧场主Server之前分发。如果这些版本在您的私人注册表中不可用，则Rancher Server将变得不稳定。

对于Rancher服务器的每个版本，相应的Rancher代理和Rancher代理实例版本将在发行说明中可用。为了找到您的基础架构服务的图像，您需要参考`infra-templates`我们的[Rancher目录](https://github.com/rancher/rancher-catalog)和[社区目录](https://github.com/rancher/community-catalog)中的文件夹，以查看您要包括哪些基础架构服务以及这些[目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/no-internet-access/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)中的相关图像。

##### 将图像推送到私人注册表的命令

这些例子都为`rancher/server`和`rancher/agcnt`使用具有同时访问DockerHub和您的私人注册表机镜像。我们建议您将私有注册表中的图像版本标记为DockerHub中存在的相同版本。

```
＃牧场主/服务器
$ docker pull rancher / server：v1.6.0
$ docker tag rancher / server：v1.6.0 localhost：5000 / < NAME_OF_LOCAL_RANCHER_SERVER_IMAGE >：v1.6.0
$ docker push localhost：5000 / < NAME_OF_LOCAL_RANCHER_SERVER_IMAGE >：v1.6.0

＃牧场主/剂
$ docker拉牧人/代理：v1.1.3
$ docker tag rancher / agcnt：v1.1.3 localhost：5000 / < NAME_OF_LOCAL_RANCHER_AGcnT_IMAGE >：v1.1.3
$ docker push localhost：5000 / < NAME_OF_LOCAL_RANCHER_AGcnT_IMAGE >：v1.1.3
```

> **注意：**对于任何基础架构服务映像，您必须遵循相同的步骤。

#### 启动具有私人注册表的Rancher服务器

在您的机器上，启动Rancher服务器以使用特定的Rancher代理镜像。我们建议使用特定的版本标签而不是`latest`标签，以确保您正在使用正确的版本。

例：

```
$ sudo docker run -d --restart = unless-stopped -p 8080：8080 \
    -e CATTLE_BOOTSTRAP_REQUIRED_IMAGE = < Private_Registry_Domain >：5000 / < NAME_OF_LOCAL_RANCHER_AGcnT_IMAGE >：v1.1.3 \
     < Private_Registry_Domain >：5000 / < NAME_OF_LOCAL_RANCHER_SERVER_IMAGE >：v1.6.0
```

#### 牧场用户界面

UI和API将在暴露的端口上可用`8080`。您可以访问以下URL访问用户界面：`http://<SERVER_IP>:8080`。

#### 添加主机

访问UI后，单击**添加主机**按钮。这将立即带您进入**主机注册**页面。单击**保存**。

云服务提供商将不会工作，因为Rancher使用Docker Machine通过云提供商来配置主机。单击**自定义**图标添加主机。

UI中的命令将被配置为使用专用注册表图像进行Rancher代理。

##### 示例添加自定义主机命令

```
$ sudo docker run -d --privileged -v /var/run/docker.sock:/var/run/docker.sock < Private_Registry_Domain >：5000 / < NAME_OF_LOCAL_RANCHER_AGcnT_IMAGE >：v1.1.3 http：// < SERVER_IP >：8080 / v1 / scripts / < security_credcntials >
```

#### 配置基础架构的默认注册表

在Rancher中，所有的[基础设施服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/no-internet-access/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)都是从DockerHub开始的。将默认注册表从DockerHub更改为不同的私有注册表位于API设置中。

- **添加私人注册表：**在**基础设施** - > **注册表**部分中，添加包含基础架构服务映像的私有注册表。
- **更新默认注册表：**在**管理** - > **设置** - > **高级设置下**，点击**我明白我可以通过更改高级设置来打破事情**。找到**registry.default**设置，然后单击编辑图标。添加注册表值，然后单击**保存**。一旦`registry.default`设置更新，基础架构服务将从私人注册表开始，而不是DockerHub。
- **创建新环境：**更新默认注册表后，您将需要重新创建您的环境，以便基础架构服务将使用更新的默认注册表。默认注册表更改之前的任何现有环境都将使其基础架构服务仍指向DockerHub。

> **注意：**现有环境中的任何基础架构堆栈仍将使用原始的默认注册表（例如DockerHub）。这些堆栈将需要被删除并重新启动才能开始使用更新的默认注册表。堆栈可以从**Catalog** - > **Library**部署。

### 使用HTTP代理

提醒，在此设置中，访问UI的Web浏览器将只需要访问专用网络。

#### 配置Docker以使用HTTP代理

为了设置一个HTTP代理，Docker守护进程将需要修改以指向Rancher服务器和Rancher主机的代理。在启动Rancher服务器或Rancher代理之前，编辑该`/etc/default/docker`文件以指向您的代理并重新启动Docker。

```
$ sudo vi / etc / default / docker
```

在文件中，编辑`#export http_proxy="http://127.0.0.1:3128/"`它以指向您的代理。保存更改，然后重新启动docker。每个操作系统上重新启动Docker是不同的。

> **注：**如果您使用systemd运行泊坞，请按照码头工人[说明](https://docs.docker.com/articles/systemd/#http-proxy)了如何配置HTTP代理。

#### 启动牧场服务器

使用代理时，Rancher服务器不需要使用任何环境变量启动。因此，启动Rancher服务器的命令将与常规安装相同。

```
sudo docker运行-d --restart = unless-stopped -p 8080：8080 rancher / server
```

#### 牧场用户界面

UI和API将在暴露的端口上可用`8080`。您可以访问以下URL访问用户界面：`http://<SERVER_IP>:8080`。

#### 添加主机

访问UI后，您可以单击**添加主机**按钮。这将立即带您进入**主机注册**页面。单击**保存**。

云端供应商将无法正常工作，因为Rancher用于`docker-machine`通过云提供商来配置主机。单击**自定义**图标添加主机。

来自UI的命令可以在任何将Docker配置为使用HTTP代理的计算机上使用。