---
title: Registries in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 登记

------

使用Rancher，您可以添加凭据以从DockerHub，Quay.io或您拥有私有注册表的任何地址访问私人注册表。通过有能力访问您的私人注册表，它使Rancher能够使用您的私人图像。在每个[环境中](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/registries/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)，每个注册表地址只能使用一个凭据。这使得它是从私人地址启动图像的简单请求。如果您为同一地址添加了多个凭据，则Rancher将始终使用最近添加的凭据。

Rancher支持牛和Kubernetes集装箱业务流程类型的不同注册表。

### 添加注册表

在“ **基础结构** - > **注册表”**页面上，单击“ **添加注册表”**。

对于所有注册管理机构，您需要提供**电子邮件地址**，**用户名**和**密码**。对于**自定义**注册表，您还需要提供**注册表地址**。单击**创建**。

> **注意：**对于`Address`自定义注册表，请不要预先修复`http://`或`https://`正如我们期望的只是IP或主机名。

如果您为已存在的地址添加凭据，则Rancher将开始使用新凭据。

#### 不安全的注册管理机构

为了访问不安全的注册表，您需要在主机上配置Docker守护程序。`DOMAIN`并且`PORT`是托管私有注册表的域和端口。

```
＃编辑配置文件“/ etc / default / docker”
$ sudo vi / etc / default / docker
＃在文件末尾添加这一行。如果已有选项，请确保将其附加到当前选项列表。
$ DOCKER_OPTS = “ $ DOCKER_OPTS --insecure-registry = $ {DOMAIN}：$ {PORT} ” 
＃重新启动docker服务 
$ sudo service docker restart
```

#### 自签名证书

为了在注册表中使用自签名证书，您需要在主机上配置Docker守护进程。`DOMAIN`并且`PORT`是托管私有注册表的域和端口。

```
＃从域下载证书 
$ opcnssl s_clicnt -showcerts -connect $ {DOMAIN}： $ {PORT}  < / dev / null 2> / dev / null | OpcnSSL的X509 -outform PEM > ca.crt
＃复制证书到相应的目录 
$ sudo的CP ca.crt /etc/docker/certs.d/ $ {DOMAIN} /ca.crt
＃追加证书文件 
$猫CA .crt | 须藤发球-a /etc/ssl/certs/ca-certificates.crt
＃重启泊坞窗服务，以使更改生效
$ sudo服务码头重启

```

#### 使用亚马逊的ECR注册表

使用Amazon的[EC2容器注册表](https://aws.amazon.com/ecr/)与Rancher需要额外的配置步骤。ECR使用AWS的本地身份验证服务IAM来管理访问。AWS提供了一个API，允许用户根据发出请求的IAM用户的权限为Docker生成临时证书。由于凭证在12小时后到期，因此需要每12个小时创建一个新的凭证。您可以使用[AWS ECR Credcntial Updater](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/registries/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/registries/ecr_updater)启动自动更新凭据的服务。

在Rancher中指定图像名称时，请使用AWS提供的完全限定地址：

`aws-account-number.dkr.ecr.us-west-2.amazonaws.com/my-repo:latest`。

### 使用注册表

创建注册表后，您可以在启动服务和容器时使用这些私人注册表。图像名称的语法与`docker run`命令使用的语法相同。

`[registry-name]/[namespace]/[imagcname]:[version]`

默认情况下，我们假设您正在尝试从中提取图像`DockerHub`。

### 编辑注册表

注册表的所有选项可通过列出的注册表右侧的下拉菜单访问。

对于任何**活动**注册表，您可以**停用**注册表，这将禁止访问注册表。在该注册表中没有任何图像可以启动新的容器。

对于任何**停用的**注册表，您有两个选项。您可以**激活**注册表，这将允许容器访问这些注册表中的图像。您的环境的任何成员将能够激活您的凭据，而无需重新输入密码。如果您不想让任何人使用您的凭证，您应该**删除**注册表，这将从环境中删除凭据。

您可以**编辑**任何注册表，这样可以将凭据更改为注册表地址。您将无法更改注册表地址。密码未保存在“编辑”页面中，因此您需要重新输入密码才能保存任何更改。

> **注意：**如果注册表无效（即由于较新的凭据而无效，被删除或覆盖），那么使用私有注册表映像的任何[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/registries/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)将继续运行。由于图像已经被拉到主机上，不管注册表权限如何，对图像的使用都不会有任何限制。因此，任何使用图像的服务或附加容器的扩大将能够运行。运行容器时，Rancher不会检查证书是否仍然有效，因为我们假设您已经给予主机访问映像的权限。

### 更改默认注册表

默认情况下，Rancher自动假设任何没有注册表前缀的图像都应该从DockerHub中拉出。您可以通过更新API中的设置将默认注册表从DockerHub更改为另一个注册表。

1. 在**管理** - > **设置** - > **高级设置下**，点击**我明白我可以通过更改高级设置来打破事情**。
2. 找到**registry.default**设置，然后单击编辑图标。
3. 添加注册表值，然后单击**保存**。

一旦**registry.default**设置被更新，没有注册表前缀（例如`ubuntu:14.0.4`）的任何图像都将从默认注册表而不是DockerHub中拉出。

如果您正在使用私人注册表要求凭据，则需要将注册表添加到Rancher，以使默认注册表生效。

> **注意：**现有环境中的任何服务仍将使用原始的默认注册表（例如DockerHub）。对于基础架构堆栈开始使用新的默认注册表，它将需要被删除并重新启动才能开始使用更新的默认注册表。堆栈可以从**Catalog** - > **Library**部署。

### 限制可以使用哪个注册表

默认情况下，添加到Rancher中的任何注册表都可用于拉取图像。Rancher的管理员可能希望限制哪些注册表被批准。您可以通过更新API中的设置来限制哪些注册表被批准用于拉取图像。

1. 在**管理** - > **设置** - > **高级设置下**，点击**我明白我可以通过更改高级设置来打破事情**。
2. 找到**registry.whitelist**设置，然后单击编辑图标。
3. 添加要列入白名单的注册表列表。如果有多个注册表，注册表应以逗号分隔。

一旦**注册表**设置更新了，在拉扯图像之前，图像的注册表将在拉扯图像之前确认它已在批准的注册表列表中。如果注册表不在批准的列表中，则图像拉取将失败。

> **注意：**将任何注册表添加到此值后，DockerHub将自动不再有效。要包括DockerHub，您需要添加`index.docker.io`一个注册表。