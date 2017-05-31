---
title: Rancher Compose
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/rancher-compose/
---

## Rancher Compose
---

Rancher Compose工具是Docker Compose的多主机版本。它在Rancher UI 中的[堆栈](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/stacks)范围内运行，这个[属性](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)属于一个[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)，并且有很多[主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)。由Rancher Compose启动的容器将部署在满足[调度规则的环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/scheduling)中的任何主机上。如果没有调度规则，则在具有最少容器的主机上启动服务的容器。这些容器就像您在UI中启动服务一样启动，因为Rancher Compose正在进行相同的API调用。

Rancher Compose工具就像受欢迎的Docker Compose一样工作，并支持V1和V2版本的 `docker-compose.yml`文件。要启用Rancher支持的功能，您还可以`rancher-compose.yml`扩展和覆盖该功能`docker-compose.yml`。例如，服务规模和健康检查将在`rancher-compose.yml`档案中。

Rancher Compose的文档期望用户了解Docker Compose。在开始使用Rancher Compose之前，请阅读[Docker Compose文档](https://docs.docker.com/compose/)。

### 安装

二进制文件可以直接从UI下载。该链接可以在UI中的页脚右侧找到。我们有Windows，Mac和Linux的二进制文件。

您还可以查看[Rancher Compose](https://github.com/rancher/rancher-compose/releases)的[发行页](https://github.com/rancher/rancher-compose/releases)，直接下载二进制文件。

### 设置Rancher与Rancher服务器组合

要使Rancher Compose在Rancher实例中启动服务，您需要设置环境变量或在Rancher Compose命令中作为选项传递这些变量。将所需的环境变量`RANCHER_URL`，`RANCHER_ACCESS_KEY`和`RANCHER_SECRET_KEY`。访问密钥和密钥将是一个[环境API密钥](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/v2-beta/api-keys)。环境API密钥可以在**API** - > **高级选项中**创建。

> **注意：**默认情况下，**API**部分下的API密钥是**帐户API密钥**，您需要创建一个**环境API密钥**，该**密钥**位于“ **高级选项”中**。

```
＃设置Rancher在 
$ export 上的url RANCHER_URL = http：// server_ip：8080 /
＃设置访问密钥，即用户名 
$ export RANCHER_ACCESS_KEY = < username_of_environment_api_key > 
＃设置密钥，即密码 
$ export RANCHER_SECRET_KEY = < password_of_environment_api_key >
```

如果您选择不设置环境变量，则需要传递与选项相同的值作为任何Rancher Compose命令的一部分。

```
$ rancher-compose --url http：// server_ip：8080 --access-key < username_of_environment_api_key > --secret-key < password_of_environment_api_key > up
```

现在，您可以使用任何`docker-compose.yml`文件与Rancher Compose [启动服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services/#adding-services-with-rancher-compose)。该服务将自动在您的牧场主实例启动的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)，环境API密钥位于英寸

就像Docker Compose一样，您可以选择与您的所有服务`docker-compose.yml`进行交互，也可以通过在命令后面列出其名称来选择与特定服务进行交互。

```
$ rancher-compose up servicename1 servicename2
$ rancher-compose stop servicename2

```

### 用Rancher Compose进行调试

当牧场主撰写工作，你可以设置环境变量`RANCHER_CLIENT_DEBUG`，来`true`，这将对所有的CLI命令打印出所取得的API调用的详细消息。

```
＃打印所有CLI调用的详细消息 
$ export RANCHER_CLIENT_DEBUG = true
```

如果您不想在每个CLI命令上进行详细的响应，则可以传入`--debug`特定命令以获取详细消息。

```
$ rancher-compose --debug up -d
```

### 删除服务/容器

默认情况下，Rancher Compose不会删除容器/服务。这意味着如果您`up`连续执行两个命令，则第二个命令`up`不会执行任何操作。这是因为第一个将创建一切并保持运行。即使你不及格`-d`到`up`，牧场主撰写不会删除你的服务。要删除必须使用的服务`rm`。

### 构建

Docker版本有两种支持。首先是设置`build:`与[Docker Remote API中](https://docs.docker.com/reference/api/docker_remote_api_v1.18/#build-image-from-a-dockerfile)的远程参数兼容的git或HTTP URL 。第二种方法是设置`build:`到本地目录，构建上下文将被上传到S3，然后在每个节点上根据需要构建。

对于基于S3的版本，您必须[设置AWS凭据](https://github.com/aws/aws-sdk-go/#configuring-credentials)。我们提供了一个[关于](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose/build)如何使用S3在Rancher Compose中构建的[详细示例](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose/build)。