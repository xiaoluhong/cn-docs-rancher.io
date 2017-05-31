---
title: Rancher Command Line Interface (CLI)
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## Rancher Command Line Interface
---

Rancher命令行界面（CLI）是一个统一的工具来管理您的Rancher服务器。使用此工具，您可以控制您的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)，[主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)，堆栈，服务和容器。

### 安装

二进制文件可以直接从UI下载。该链接可以在UI中的页脚右侧找到。我们有Windows，Mac和Linux的二进制文件。您还可以查看[我们的CLI](https://github.com/rancher/cli/releases)的[发行版页面](https://github.com/rancher/cli/releases)，直接下载二进制文件。

### 配置Rancher命令行界面

有几种方法可以配置Rancher CLI与Rancher进行交互时使用的设置，即Rancher URL和帐户[API密钥](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/api-keys)。帐户API密钥可以在**API中**创建。

有什么将要使用的特定加载顺序。

1. 在此期间`rancher config`，您将设置Rancher URL和API密钥。如果有多个环境，那么您将选择一个特定的环境。
2. 您可以为相同的值设置环境变量，这将覆盖设置的值`rancher config`。
3. 如果您决定将值直接传递给Rancher命令，则这些值将用于Rancher命令超过任何其他值。

#### 使用Rancher配置

您可以运行`rancher config`使用Rancher服务器设置配置。

```
$ rancher配置
URL []：http：// < server_ip >：8080
访问密钥[]：< accessKey_of_account_api_key > 
密钥[]：   < secretKey_of_account_api_key > 
＃如果有多个环境，
＃将被要求选择要使用哪个环境
环境：
[1]默认（1a5）
[2] k8s（1a10）
选择：1
INFO [config]将配置保存到/ Users / < username > /.rancher/cli.json
```

#### 使用环境变量

您可以设置以下环境变量`RANCHER_URL`，`RANCHER_ACCESS_KEY`和`RANCHER_SECRET_KEY`。

```
＃设置Rancher在 
$ export 上的url RANCHER_URL = http：// < server_ip >：8080
＃设置访问密钥，即用户名 
$ export RANCHER_ACCESS_KEY = < accessKey_of_account_api_key > 
＃设置密钥，即密码 
$ export RANCHER_SECRET_KEY = < secretKey_of_account_api_key >
```

如果您的Rancher服务器中有多个环境，您还需要设置一个环境变量来选择特定的环境，即`RANCHER_cnVIRONMcnT`。

```
＃设置要使用的环境，可以使用环境ID或环境名称 
$ export RANCHER_cnVIRONMcnT = < cnvironmcnt_id >
```

#### 传递选项

如果您选择不运行`rancher config`或设置环境变量，则可以将与选项相同的值作为任何`rancher`命令的一部分传递。

```
$ rancher --url http：// server_ip：8080 --access-key < accessKey_of_account_api_key > --secret-key < secretKey_of_account_api_key > --cnv < cnvironmcnt_id > ps
```

### 使用Rancher CLI进行调试

使用Rancher CLI时，您可以将环境变量设置`RANCHER_CLIcnT_DEBUG`为`true`，将所有CLI命令打印出正在进行的API调用的详细消息。

```
＃打印所有CLI调用的详细消息 
$ export RANCHER_CLIcnT_DEBUG = true
```

如果您不想在每个CLI命令上进行详细的响应，请将环境变量设置`RANCHER_CLIcnT_DEBUG`为`false`并传入`--debug`特定命令以获取详细消息。

```
$ rancher --debug cnv create newcnv
```

### 使用环境

如果您使用帐户API密钥，您将能够创建和更新环境。如果您使用环境API密钥，您将无法创建或更新其他环境，您将只能看到现有的环境。

```
$ rancher cnv ls
ID名称状态目录系统细节
1e1动物园管理员健康目录：//社区：zookeeper：1    false      
1e2默认健康                                      假      
1e3 App1健康                                      假     
```

### 使用特定主机

有一些命令（例如`rancher docker`和`rancher ssh`）需要选择要使用的特定主机。您可以设置一个环境变量来选择一个主机，即`RANCHER_DOCKER_HOST`通过或`--host`指定一个主机。

选择主机之前，您可以列出环境中的所有主机。

```
$ rancher hosts
ID        HOSTNAME      STATE     IP
1h1       host-1        active    111.222.333.444
1h2       host-3        active    111.222.333.445
1h3       host-2        active    111.222.333.446

```

现在，您可以使用主机ID或主机名设置环境变量`RANCHER_DOCKER_HOST`或传入，`--host`以选择其他主机。

```
＃设置主机总是选择host-1（1h1） 
$ export RANCHER_DOCKER_HOST = 1h1
＃列出在主机1上运行的容器
$ rancher docker ps
＃列出在host-2 
$ rancher --host host-2 docker ps上运行的容器
```

### 使用服务和容器

#### 列出所有的服务

在您所选的环境中，您可以查看在环境中运行的所有服务。

```
$ rancher ps
ID类型名称图像状态规模cnDPOINTS细节
1s1服务zookeeper / zk rawmind / alpine-zk：3.4.8-4健康3                              
1s2服务默认/ nginxApp nginx健康1                              
1s4服务App1 / db1 mysql健康1                              
1s5服务App1 / wordpress wordpress健康4                              
1s6 loadBalancerService App1 / wordpress-lb健康1 111.222.333.444:80   
```

#### 列出所有容器

您可以查看环境中的所有容器，而不是具体的服务。

```
$ rancher ps -c
ID NAME IMAGE STATE HOST IP DOCKER DETAIL
1i1 zookeeper_zk_zk-volume_1 rawmind / alpine-volume：0.0.1-1 stopped 1h1 a92b6d3dad18   
1i2 zookeeper_zk_zk-conf_1 rawmind / rancher-zk：0.3.3 stopped 1h1 2e8085a4b517   
1i3 zookeeper_zk_1 rawmind / alpine-zk：3.4.8-4健康1h1 10.42.150.2 e3ef1c6ff70e   
1i5 zookeeper_zk_zk-volume_2 rawmind / alpine-volume：0.0.1-1 stopped 1h2 e716f562e0a4   
1i6 zookeeper_zk_zk-conf_2 rawmind / rancher-zk：0.3.3停止1h2 5cd1cebea5a3   
1i7 zookeeper_zk_2 rawmind / alpine-zk：3.4.8-4健康1h2 10.42.88.102 21984a4445d1   
1i9 zookeeper_zk_zk-volume_3 rawmind / alpine-volume：0.0.1-1 stopped 1h3 7c614003f08c   
1i10 zookeeper_zk_zk-conf_3 rawmind / rancher-zk：0.3.3 stop 1h3 53fb77cd8ae0   
1i11 zookeeper_zk_3 rawmind / alpine-zk：3.4.8-4健康1h3 10.42.249.162 84a80eb8e037   
1i13 Default_nginxApp_1 nginx running 1h1 10.42.107.28 e1195a563280   
1i15 App1_db1_1 mysql running 1h3 10.42.116.171 0624e0a7f2fc   
1i16 App1_wordpress_1 wordpress运行1h1 10.42.66.199 4bb77abebc08   
1i17 App1_wordpress-lb_1牧场主/ lb-service-haproxy：v0.4.2健康1h2 10.42.199.163 5d3a005278d3   
1i18 App1_wordpress_2 wordpress running 1h2 10.42.88.114 01ec967c49ac   
1i19 App1_wordpress_3 wordpress running 1h3 10.42.218.81 3aae3fc6163a   
1i20 App1_wordpress_4 wordpress running 1h1 10.42.202.31 0b67ef86db22   
```

#### 列出特定服务的容器

如果要查看特定服务的容器，可以通过添加服务ID或服务名称列出服务中运行的所有容器。

```
$ rancher ps 1s5
ID NAME IMAGE STATE HOST IP DOCKER DETAIL
1i16 App1_wordpress_1 wordpress运行1h1 10.42.66.199 4bb77abebc08   
1i18 App1_wordpress_2 wordpress running 1h2 10.42.88.114 01ec967c49ac   
1i19 App1_wordpress_3 wordpress running 1h3 10.42.218.81 3aae3fc6163a   
1i20 App1_wordpress_4 wordpress running 1h1 10.42.202.31 0b67ef86db22
```

### 使用Docker撰写文件启动简单的服务

要开始向Rancher添加服务，您可以创建一个简单的`docker-compose.yml`文件和可选的`rancher-compose.yml`文件。如果没有`rancher-compose.yml`文件，则所有服务将以1个容器的比例开始。

样品 `docker-compose.yml`

```
版本：' 2 '
服务：
   service1：
     image：nginx
```

样品 `rancher-compose.yml`

```
版本：' 2 '
服务：
   ＃参考要扩展的
  服务service1：
     scale：2
```

创建文件后，可以将服务启动到Rancher服务器。

```
＃创建和启动没有环境变量的服务和选择堆栈
＃如果没有提供任何堆，栈名称将是该命令运行文件夹名称
＃如果堆栈不高的农场存在，它会创建
＃添加在-d结束时不阻止和记录 
$ rancher --url URL_of_Rancher --access-key < username_of_account_api_key > --secret-key < password_of_account_api_key > --cnv默认值up -s stack1 -d

＃创建和启动一个已经设置了环境变量的服务
＃在最后添加-d以阻止和记录
$ rancher up -s stack1 -d

＃要更改现有服务的规模，可以使用stackName / serviccname或service ID
$ rancher scale Default / service1 = 3
$ rancher scale 1s4 = 5
```

### 启动服务使用 `Rancher run`

如果您已使用Docker CLI添加服务，还可以使用Rancher添加容器`rancher run`。

```
＃服务应该是stackName / service_name 
$ rancher run --name stackA / service1 nginx
```

### 命令参考

要了解有关所有支持的命令的更多信息，请阅读我们的Rancher [命令](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli/commands)文档。