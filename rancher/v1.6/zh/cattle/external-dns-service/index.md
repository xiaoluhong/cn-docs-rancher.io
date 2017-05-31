---
title: External DNS Service
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 外部DNS服务

------

作为[Rancher目录的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/external-dns-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)一部分，Rancher提供了多个DNS服务，用于监听rancher-metadata事件，并根据元数据更改生成DNS记录。示例将使用Route53作为外部DNS服务如何工作的示例，但是Rancher还与其他DNS提供商进行社区贡献服务。

### 最佳做法

- 对于Rancher设置中的每个环境，应该有一个`route53`scale 1 的服务。
- 多个Rancher实例不应该共享相同`hosted zone`。

### 启动Route53服务

从**目录**选项卡，您可以选择**Route53 DNS堆栈**。

提供一个**名称**，并且如果需要的话，**说明**堆栈。

在**配置选项中**，您需要提供以下内容：

| 名称      | 值                                        |
| ------- | ---------------------------------------- |
| AWS访问密钥 | 访问您的AWS API的密钥                           |
| AWS秘密密钥 | AWS API的秘密密钥                             |
| AWS地区   | AWS中的区域名称 我们建议将该地区设置为最接近您所在地理位置的地区。它将被转换到AWS API端点，Rancher Route53将发送更新的DNS请求。 |
| 托管区     | Route53主机区。这必须在您的Route53实例上预先创建。         |

填写表单后，点击**创建**。该堆栈将使用`route53'服务创建，您只需要启动服务！

### 使用Route53服务

该`route53`服务将仅为具有向主机发布端口的服务生成DNS记录。对于Rancher生成的每个DNS记录，它将以以下格式为服务创建fqdn：

```
fqdn=<serviceName>.<stackName>.<environmentName>.<yourHostedZoneName>

```

在AWS中的路由53上，它将被表示为具有名称= fqdn和value = [服务部署的主机的ip地址]的记录集。Rancher `route53`服务将仅管理以。结束的记录集。目前，默认TTL为300秒。

一旦在AWS上的Route 53上设置了DNS记录，生成的fqdn将被传回给Rancher，并将在**service.fqdn**字段上设置。您可以从服务的下拉菜单中使用**View in API**找到fqdn字段并搜索**fqdn**。

当在浏览器中使用fqdn时，它将被定向到服务中的一个容器。如果与服务中的容器相关联的IP有任何更改，则这些更改将更新AWS Route 53中的值。由于用户将始终使用fqdn，用户的角度将不会更改。

> **注：**在以后`route53`的服务推出后，与已经部署在主机端口的任何服务，也将获得一个FQDN。

### 删除Route53服务

当`route53`服务从Rancher中删除时，Amazon Route 53中的记录集**不会**被删除。那些需要在您的亚马逊帐户中手动删除。

### 对外部DNS使用特定IP

默认情况下，Rancher DNS选择在Rancher服务器中注册的主机IP，用于公开服务。将会出现使用私有网络在Rancher中配置主机的用例，但这些主机将需要通过公共网络公开使用外部DNS的服务。如果您要指定要用于外部DNS的IP，则需要在启动外部DNS服务之前添加[主机标签](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/external-dns-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/#host-labels)。

在启动外部DNS服务之前，请向您的主机添加以下标签。标签的价值是Rancher的Route53 DNS服务将在编程规则时使用。如果主机上未设置此标签，Rancher的Route53 DNS服务将自动使用Rancher中显示的主机IP。

```
io.rancher.host.external_dns_ip=<IP_TO_BE_USED_FOR_EXTERNAL_DNS>
```

