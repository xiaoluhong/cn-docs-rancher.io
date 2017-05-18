---
title: Installing Rancher Server
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/installing-rancher/installing-server/
  - /rancher/latest/zh/installing-rancher/installing-server/
---

## 安装Rancher服务器

------

Rancher被部署为一组Docker容器。运行的牧场主是简单的启动两个容器。一个容器作为管理服务器，另一个容器在节点上作为代理。

- [牧场服务器 - 单个容器（非HA）](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#single-container)
- [Rancher服务器 - 单个容器（非HA） - 外部数据库](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#single-container-external-database)
- [Rancher服务器 - 单容器（非HA） - 绑定的MySQL卷](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#single-container-bind-mount)
- [Rancher服务器 - 完全主动/主动HA](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#multi-nodes)
- [Rancher服务器 - 在AWS中使用ELB](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#elb)
- [Rancher服务器 - 使用TLS的AD / OpenLDAP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#ldap)
- [Rancher服务器 - HTTP代理](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#http-proxy)

> **注意：**您可以通过运行获取Rancher服务器容器的所有帮助选项`docker run rancher/server --help`。

### 要求

- 任何具有受

  支持版本的Docker的

  现代Linux发行[版](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#supported-docker-versions)。

  RancherOS

  ，Ubuntu，RHEL / CentOS 7进行了更严格的测试。

  - 对于RHEL / CentOS，[Docker](https://docs.docker.com/engine/reference/commandline/dockerd/#/storage-driver-options)不推荐使用默认存储驱动程序，即使用环回的devicemapper 。请参考Docker文档，了解如何更改。
  - 对于RHEL / CentOS，如果要启用SELinux，则需要[安装其他SELinux模块](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/selinux)。

- 1GB RAM

- MySQL服务器应该有一个max_connections设置> 150

  - MYSQL配置要求
    - 选项1：用羚羊运行，默认值为 `COMPACT`
    - 选项2：使用Barracuda运行MySQL 5.7，默认`ROW_FORMAT`值为`Dynamic`

> **注意：**目前，Rancher不支持Docker for Mac。

### 牧场服务器标签

Rancher服务器有2个不同的标签。对于每个主要版本标签，我们将提供特定版本的文档。

- `rancher/server:latest`标签将是我们最新的开发版本。这些构建将通过我们的CI自动化框架进行验证。这些版本不适用于部署在生产中。
- `rancher/server:stable`标签将是我们最新的稳定发布版本。此标签是我们推荐用于生产的版本。

请不要使用任何带有`rc{n}`后缀的版本。这些`rc`构建意味着Rancher团队测试构建。

### 启动Rancher服务器 - 单容器（非HA）

在安装了Docker的Linux机器上，启动Rancher的单个实例的命令很简单。

```
$ sudo docker run -d --restart = unless-stopped -p 8080：8080 rancher / server
```

### 牧场用户界面

UI和API将在暴露的端口上可用`8080`。在Docker图像下载完成之后，Rancher成功启动后可能需要一两分钟的时间才能查看。

导航到以下网址：`http://<SERVER_IP>:8080`。该`<SERVER_IP>`是运行牧场主服务器主机的公网IP地址。

一旦UI启动并运行，您可以通过[添加主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)或从基础架构目录中选择一个容器编排。默认情况下，如果不选择不同的容器编排类型，环境将使用牛。将主机添加到Rancher之后，您可以从[Rancher目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)开始添加[服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)或启动模板。

### 启动Rancher服务器 - 单个容器 - 外部数据库

而不是使用Rancher服务器附带的内部数据库，您可以启动指向外部数据库的Rancher服务器。该命令将是一样的，但附加参数以指导如何连接到外部数据库。

> **注意：**您的数据库，数据库的名称和用户已经需要创建，但不需要创建任何模式。Rancher会自动创建与Rancher相关的所有模式。

以下是创建数据库和用户的SQL命令示例。

```
> CREATE DATABASE IF NOT EXISTS牛COLLATE =  ' utf8_general_ci ' CHARACTER SET  =  ' UTF-8 ' ;
>  GRANT ALL ON牛。* TO '黄牛' @ '％' IDENTIFIED BY '牛' ;
>  GRANT ALL ON牛。* TO '黄牛' @ ' localhost的' IDENTIFIED BY '
```

要启动Rancher连接到外部数据库，您可以传递其他参数作为容器命令的一部分。

```
$ sudo docker run -d --restart = unless-stopped -p 8080：8080 rancher / server \
    --db-host myhost.example.com --db-port 3306 --db-user username --db-pass password --db-name cattle
```

传入的大多数选项也具有默认值，不是必需的。只需要MySQL服务器的位置。

```
--db主机的IP或MySQL服务器的主机名
-  MySQL服务器的db-port端口（默认值：3306）
- 用于 MySQL登录的db-user用户名（默认值：牛）
--db通密码为 MySQL的登录（默认：牛）
--db-name要使用的MySQL数据库名称（默认值：牛）
```

> **注意：**在以前的Rancher服务器版本中，我们使用环境变量连接到外部数据库，但这些环境变量将继续工作，但是Rancher建议使用参数。

### 启动Rancher服务器 - 单容器 - 绑定MySQL卷

如果要将容器内的数据库保存到主机上的卷上，请通过绑定MySQL卷来启动Rancher服务器。

```
$ sudo docker run -d -v < host_vol >：/ var / lib / mysql --restart = unless-stopped -p 8080：8080 rancher / server
```

使用此命令，数据库将在主机上保留。如果您有一个现有的Rancher容器，并希望绑定装载MySQL卷，则说明将位于我们的[升级文档中](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/upgrading/#single-container-bind-mount)。

### 启动Rancher服务器 - 完全主动/主动HA

在高可用性（HA）中运行[Rancher服务器与使用外部数据库](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#using-an-external-database)运行[Rancher服务器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#using-an-external-database)一样简单，暴露了一个附加端口，并为外部负载平衡器的命令添加了一个附加参数。

#### 医管局要求

- HA节点：

  - 任何具有受

    支持版本的Docker的

    现代Linux发行[版](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#supported-docker-versions)。

    RancherOS

    ，Ubuntu，RHEL / CentOS 7进行了更严格的测试。

    - 对于RHEL / CentOS，[Docker](https://docs.docker.com/engine/reference/commandline/dockerd/#/storage-driver-options)不推荐使用默认存储驱动程序，即使用环回的devicemapper 。请参考Docker文档，了解如何更改。
    - 对于RHEL / CentOS，如果要启用SELinux，则需要[安装其他SELinux模块](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/selinux)。

  - 节点之间开通需要的端口：`9345`，`8080`

  - 1GB RAM

- MySQL数据库

  - 至少1 GB RAM
  - 每个Rancher服务器节点有50个连接（例如，A 3节点设置将需要至少支持150个连接）
  - MYSQL配置要求
    - 选项1：用羚羊运行，默认值为 `COMPACT`
    - 选项2：使用Barracuda运行MySQL 5.7，默认`ROW_FORMAT`值为`Dynamic`

- 外部负载平衡器

  - 需要在节点和外部负载平衡器之间打开的端口： `8080`

> **注意：**目前，Rancher不支持Docker for Mac。

#### 对更大部署的建议

- 每个Rancher服务器节点应具有4 GB或8 GB堆大小，这需要至少8 GB或16 GB的RAM
- MySQL数据库应该有快速的磁盘
- 对于真正的HA，建议使用具有适当备份的复制MySQL数据库。使用Galera并强制写入单个节点，由于事务锁定，将是一种替代方案。

1. 在您要添加到HA设置的每个节点上，运行以下命令：

   ```
   ＃在HA群集中的每个节点上启动
   $ docker run -d --restart = unless-stopped -p 8080：8080 -p 9345：9345 rancher / server \
        --db-host myhost.example.com --db-port 3306 --db-user username --db-pass password --db-name cattle \
        --advertise-address < IP_of_the_Node >
   ```

   对于每个节点，每个节点`<IP_of_the_Node>`将是唯一的，因为它将是要添加到HA设置中的每个特定节点的IP。

   如果您更改`-p 8080:8080`为将HTTP端口公开到主机上的其他端口，则需要添加`--advertise-http-port <host_port>`到该命令。

   > **注意：**您可以通过运行获取命令的帮助`docker run rancher/server --help`

2. 配置一个外部负载平衡器，它将平衡端口上的流量，`80`以及`443`将运行Rancher服务器并定位端口上的节点的节点池`8080`。您的负载平衡器必须支持websockets和转发头文件，以使Rancher正常运行。请参阅[SSL设置页面](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/basic-ssl-config)，例如配置设置。

#### 关于HA中的Rancher服务器节点的注意事项

如果您的Rancher服务器节点的IP更改，您的节点将不再是Rancher HA群集的一部分。您必须使用不正确的IP来停止旧的Rancher服务器容器，`--advertise-address`并启动具有正确IP的新的Rancher服务器`--advertise-address`。

### 在AWS中的弹性/经典负载平衡器（ELB）后面运行Rancher服务器

我们建议在您的Rancher服务器前面的AWS中使用ELB。为了使ELB能够正确使用Rancher的Websockets，您需要启用代理协议模式，并确保禁用HTTP支持。默认情况下，ELB在HTTP / HTTPS模式下启用，不支持websockets。监听器配置必须特别注意。

如果您有ELB设置的问题，我们建议您尝试使用[terraform版本](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/index.md#configuring-using-terraform)，因为这样可以减少错过设置的机会。

> **注意：**如果您使用自签名证书，请阅读更多关于如何[在我们的SSL部分的AWS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/basic-ssl-config/#elb)中[配置您的ELB](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/basic-ssl-config/#elb)。

#### 侦听器配置 - 明文

对于简单的，未加密的负载平衡目的，需要以下监听器配置：

| 配置类型 | 负载平衡器协议 | 负载平衡器端口 | 实例协议 | 实例端口                                     |
| ---- | ------- | ------- | ---- | ---------------------------------------- |
| 纯文本  | TCP     | 80      | TCP  | 8080（或`--advertise-http-port`启动Rancher服务器时使用的端口） |

#### 启用代理协议

为了使Web套接字正常运行，必须应用ELB代理协议策略。

- 启用[代理协议](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/enable-proxy-protocol.html)模式

```
$ aws elb create-load-balancer-policy --load-balancer-name <LB_NAME> --policy-name <POLICY_NAME> --policy-type-name ProxyProtocolPolicyType --policy-attributes AttributeName=ProxyProtocol,AttributeValue=true
$ aws elb set-load-balancer-policies-for-backend-server --load-balancer-name <LB_NAME> --instance-port 443 --policy-names <POLICY_NAME>
$ aws elb set-load-balancer-policies-for-backend-server --load-balancer-name <LB_NAME> --instance-port 8080 --policy-names <POLICY_NAME>

```

- 健康检查可以配置为使用HTTP：8080 `/ping`作为您的路径。

#### 配置使用Terraform

以下可用作使用Terraform进行配置的示例：

```
resource "aws_elb" "lb" {
  name               = "<LB_NAME>"
  availability_zones = ["us-west-2a","us-west-2b","us-west-2c"]
  security_groups = ["<SG_ID>"]

  listener {
    instance_port     = 8080
    instance_protocol = "tcp"
    lb_port           = 443
    lb_protocol       = "ssl"
    ssl_certificate_id = "<IAM_PATH_TO_CERT>"
  }

}

resource "aws_proxy_protocol_policy" "websockets" {
  load_balancer  = "${aws_elb.lb.name}"
  instance_ports = ["8080"]
}

```

### 在AWS中运行应用程序负载平衡器（ALB）后的Rancher服务器

我们不再通过使用弹性/经典负载平衡器（ELB）在AWS中推荐应用程序负载平衡器（ALB）。如果仍然选择使用ALB，则需要将流量定向到节点上的HTTP端口，这是`8080`默认情况。

### 为TLS启用Active Directory或OpenLDAP

为了为使用TLS的Rancher服务器启用Active Directory或OpenLDAP，Rancher服务器容器将需要使用由LDAP设置提供的LDAP证书启动。在要启动Rancher服务器的Linux机器上，保存证书。

通过绑定安装具有证书的卷启动Rancher。该证书**必须**被调用`ca.crt`的容器内。

```
$ sudo docker run -d --restart = unless-stopped -p 8080：8080 \
  -v /some/dir/cert.crt:/var/lib/rancher/etc/ssl/ca.crt rancher / server
```

您可以`ca.crt`通过检查Rancher服务器容器的日志来检查是否成功传递给Rancher服务器容器。

```
$ docker 日志< SERVER_CONTAINER_ID >
```

在日志开始时，将确认证书已正确添加。

```
添加ca.crt到Certs。
更新证书在 / etc / ssl / certs ... 1添加，0删除; 完成。在 /etc/ca-certificates/update.d....done中
运行挂钩。
证书被添加到密钥库
```

### 在HTTP代理之后启动Rancher服务器

为了设置一个HTTP代理，Docker守护进程将需要修改以指向代理。在启动Rancher服务器之前，编辑该`/etc/default/docker`文件以指向您的代理并重新启动Docker。

```
$ sudo vi / etc / default / docker
```

在文件中，编辑`#export http_proxy="http://127.0.0.1:3128/"`它以指向您的代理。保存更改，然后重新启动docker。每个操作系统上重新启动Docker是不同的。

> **注：**如果您使用systemd运行泊坞，请按照码头工人[说明](https://docs.docker.com/articles/systemd/#http-proxy)了如何配置HTTP代理。

为了加载[Rancher目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)，需要配置代理，并且需要使用环境变量启动Rancher服务器以传递代理信息。

```
$ sudo docker run -d \
    -e http_proxy = < proxyURL > \
    -e https_proxy = < proxyURL > \
    -e no_proxy = “ localhost，127.0.0.1 ” \
    -e NO_PROXY = “ localhost，127.0.0.1 ” \
    --restart = unless-stopped -p 8080：8080牧场主/服务器
```

如果[Rancher目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)不会被使用，请像往常一样运行Rancher server命令。

当[主机添加](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/installing-rancher/installing-server/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)到牧场主，还有后面的HTTP代理没有额外的要求。