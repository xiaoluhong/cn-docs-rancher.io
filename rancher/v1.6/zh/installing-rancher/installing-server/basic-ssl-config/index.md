---
title: Installing Rancher Server with SSL
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 使用SSL安装Rancher服务器

------

为了从`https`URL 运行Rancher服务器，您需要使用能够设置头文件的代理终止SSL。我们提供了一个如何使用NGINX，HAProxy或Apache进行设置的例子，但可以使用其他工具。

### 要求

除了典型的Rancher服务器[要求](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#requiremcnts)，您还需要：

- 有效的SSL证书：如果您的证书不是标准Ubuntu CA软件包的一部分，请使用[自签名证书说明](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/basic-ssl-config/#using-self-signed-certs-beta)。
- 已配置DNS条目

### 牧场服务器标签

Rancher服务器有2个不同的标签。对于每个主要版本标签，我们将提供特定版本的文档。

- `rancher/server:latest`标签将是我们最新的开发版本。这些构建将通过我们的CI自动化框架进行验证。这些版本不适用于部署在生产中。
- `rancher/server:stable`标签将是我们最新的稳定发布版本。此标签是我们推荐用于生产的版本。

请不要使用任何带有`rc{n}`后缀的版本。这些`rc`构建意味着Rancher团队测试构建。

### 启动牧场服务器

在我们的示例配置中，所有流量将通过代理，并通过Docker链接发送到Rancher服务器容器。还有其他的方法可以遵循，但这个例子很简单，翻译得很好。

启动Rancher服务器。我们已经添加`--name=rancher-server`到此命令中，以将代理容器链接到Rancher服务器容器。

```
$ sudo docker run -d --restart = unless-stopped --name = rancher-server rancher / server
```

> **注意：**在我们的示例中，我们假设代理将在另一个容器中运行。如果您计划从主机运行代理，则需要`8080`通过添加命令`-p 127.0.0.1:8080:8080`来在本地公开端口`docker run`。

如果要转换现有的Rancher实例，升级到新的Rancher实例将取决于您如何启动原始的Rancher实例。

- 对于在Rancher服务器容器中使用MySQL数据库的Rancher实例，请遵循创建数据容器的[升级说明](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/upgrading/#upgrading-rancher-by-creating-a-data-container)，并`--volumes-from=<data_container>`在启动新的Rancher服务器实例时添加。
- 对于具有[绑定挂载数据库的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#single-container-bind-mount) Rancher实例，请遵循[绑定挂载实例](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/upgrading/#single-container-bind-mount)的[升级说明](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/upgrading/#single-container-bind-mount)。
- 对于使用外部数据库启动的Rancher实例，请停止并删除现有的Rancher容器。使用与[外部数据库连接](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#single-container-external-database)相同的[说明](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#single-container-external-database)启动新容器。

### 示例Nginx配置

以下是需要配置的最小NGINX配置。您应该自定义您的配置以满足您的需求。确保使用nginx version> = 1.9.5。

#### 关于设置的注意事项

- `rancher-server`是您的牧场服务器容器的名称。启动您的牧场服务器容器时，该命令必须包含`--name=rancher-server`。启动您的nginx容器时，必须包含该命令才能`--link=rancher-server`使此精确配置工作。
- `<server>` 可以是任意的任意名称，但同时使用http和https服务器。

```
upstream rancher {
    server rancher-server:8080;
}

server {
    listcn 443 ssl spdy;
    server_name <server>;
    ssl_certificate <cert_file>;
    ssl_certificate_key <key_file>;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://rancher;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        # This allows the ability for the execute shell window to remain opcn for up to 15 minutes. Without this parameter, the default is 1 minute and will automatically close.
        proxy_read_timeout 900s;
    }
}

server {
    listcn 80;
    server_name <server>;
    return 301 https://$server_name$request_uri;
}

```

### Apache配置示例

这是一个Apache配置。

#### 关于设置的注意事项

- `<server_name>`是您的牧场服务器容器的名称。启动Apache容器时，该命令必须包含`--link=<server_name>`这个确切的配置才能正常工作。
- 在代理设置中，您需要替换`rancher`配置。

```
<VirtualHost *:80>
  ServerName <server_name>
  Redirect / https://<server_name>/
</VirtualHost>

<VirtualHost *:443>
  ServerName <server_name>

  SSLcngine on
  SSLCertificateFile </path/to/ssl/cert_file>
  SSLCertificateKeyFile </path/to/ssl/key_file>

  ProxyRequests Off
  ProxyPreserveHost On

  Rewritecngine On
  RewriteCond %{HTTP:Connection} Upgrade [NC]
  RewriteCond %{HTTP:Upgrade} websocket [NC]
  RewriteRule /(.*) ws://rancher:8080/$1 [P,L]

  RequestHeader set X-Forwarded-Proto "https"
  RequestHeader set X-Forwarded-Port "443"

  <Location />
    ProxyPass "http://rancher:8080/"
    ProxyPassReverse "http://rancher:8080/"
  </Location>

</VirtualHost>

```

### 示例HAProxy配置

这是需要配置的最小HAProxy配置。您应该自定义您的配置以满足您的需求。

#### 关于设置的注意事项

- `<rancher_server_X_IP>` 是您的牧场服务器的IP地址。

```
global
  maxconn 4096
  ssl-server-verify none

defaults
  mode http
  balance roundrobin
  option redispatch
  option forwardfor

  timeout connect 5s
  timeout queue 5s
  timeout clicnt 36000s
  timeout server 36000s

frontcnd http-in
  mode http
  bind *:443 ssl crt /etc/haproxy/certificate.pem
  default_backcnd rancher_servers

  # Add headers for SSL offloading
  http-request set-header X-Forwarded-Proto https if { ssl_fc }
  http-request set-header X-Forwarded-Ssl on if { ssl_fc }

  acl is_websocket hdr(Upgrade) -i WebSocket
  acl is_websocket hdr_beg(Host) -i ws
  use_backcnd rancher_servers if is_websocket

backcnd rancher_servers
  server websrv1 <rancher_server_1_IP>:8080 weight 1 maxconn 1024
  server websrv2 <rancher_server_2_IP>:8080 weight 1 maxconn 1024
  server websrv3 <rancher_server_3_IP>:8080 weight 1 maxconn 1024

```

### 更新主机注册

在使用这些设置启动Rancher之后，UI将会启动并运行`https://<your domain>/`。

在[添加主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)之前，您需要正确配置SSL的[主机注册](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/settings/#host-registration)。

### 在AWS中使用SSL的弹性负载平衡器（ELB）后运行Rancher服务器

我们建议在您的牧场服务器前面的AWS中使用ELB。为了使ELB能够正确使用Rancher的Websockets，您需要启用代理协议模式，并确保禁用HTTP支持。默认情况下，ELB在HTTP / HTTPS模式下启用，不支持websockets。监听器配置必须特别注意。

#### 侦听器配置 - SSL

对于ELB上的SSL终止，侦听器配置应如下所示：

| 配置类型  | 负载平衡器协议    | 负载平衡器端口 | 实例协议 | 实例端口                                     |
| ----- | ---------- | ------- | ---- | ---------------------------------------- |
| SSL端接 | SSL（安全TCP） | 443     | TCP  | 8080（或`--advertise-http-port`启动Rancher服务器时使用的端口） |

- 添加适当的安全组和SSL证书

#### 启用代理协议

为了使Web套接字正常运行，必须应用ELB代理协议策略。

- 启用[代理协议](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/cnable-proxy-protocol.html)模式

```
$ aws elb create-load-balancer-policy --load-balancer-name <LB_NAME> --policy-name <POLICY_NAME> --policy-type-name ProxyProtocolPolicyType --policy-attributes Attributcname=ProxyProtocol,AttributeValue=true
$ aws elb set-load-balancer-policies-for-backcnd-server --load-balancer-name <LB_NAME> --instance-port 443 --policy-names <POLICY_NAME>
$ aws elb set-load-balancer-policies-for-backcnd-server --load-balancer-name <LB_NAME> --instance-port 8080 --policy-names <POLICY_NAME>

```

- 健康检查可以配置为使用HTTP：8080 `/ping`作为您的路径。

### 在AWS中使用SSL的应用程序负载平衡器（ALB）后面运行Rancher服务器

我们不再通过使用弹性/经典负载平衡器（ELB）在AWS中推荐应用程序负载平衡器（ALB）。如果仍然选择使用ALB，则需要将流量定向到节点上的HTTP端口，这是`8080`默认情况。

> **注意：**如果您使用带有Kubercnetes的ALB，`kubectl exec`将无法正常工作，对于该功能，您将需要使用ELB。

### 使用自签名证书（Beta）

#### 免责声明

此配置将适用于Rancher以独立模式（非HA设置）运行的“核心”服务。目前，[Rancher目录](https://github.com/rancher/rancher-catalog)中的任何经认证的Rancher模板都不受支持。

Rancher Compose CLI将要求CA证书作为操作系统的默认存储的一部分。参见[Golang root_ *](https://golang.org/src/crypto/x509/)。

#### 服务器先决条件

- CA证书文件为PEM格式
- 由CA为Rancher服务器签名的证书
- 配置为终止SSL和反向代理服务器的NGINX或Apache实例

#### 牧场服务器

1. 使用修改的Docker命令启动Rancher服务器容器。证书**必须**位于`/var/lib/rancher/etc/ssl/ca.crt`容器内并调用。

   ```
   $ sudo docker run -d --restart = unless-stopped -p 8080：8080 -v /some/dir/cert.crt:/var/lib/rancher/etc/ssl/ca.crt rancher / server
   ```

   ​

   > **注意：**如果您在容器中运行NGINX或Apache，则可以直接链接实例，而不是发布Rancher UI 8080端口。

   该命令将配置服务器的ca证书包，以便机器配置，目录和编写执行程序的Rancher服务可以与Rancher服务器进行通信。

2. 如果您正在使用带有NGINX或Apache的容器终止SSL，请启动该容器，并在命令中包含`--link = <rancher_server_container_name>。

3. 牧场主访问过`https`的地址，即`https://rancher.server.domain`。

4. 更新SSL 的[主机注册](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/settings/#host-registration)。

> **注意：**除非运行Web浏览器的计算机信任用于签署Rancher服务器证书的CA证书，否则浏览器将在访问网页时发出不受信任的站点警告。

#### 添加主机

1. 在要添加到Rancher的主机上，将CA证书（必须以pem格式保存）保存到`/var/lib/rancher/etc/ssl`具有文件名的目录中`ca.crt`。
2. 添加[自定义主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/installing-rancher/installing-server/basic-ssl-config/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)，这是从UI复制和粘贴命令。该命令已经包含 `-v /var/lib/rancher:/var/lib/rancher`，所以文件将被自动复制到您的主机上。