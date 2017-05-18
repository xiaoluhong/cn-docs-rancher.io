---
title: Upgrading Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/latest/zh/upgrading/
---

## 升级Rancher服务器

------

> **注意：**如果要升级到v1.6.x，请阅读我们关于[v1.6.0](https://github.com/rancher/rancher/releases/tag/v1.6.0)的发行说明，[了解](https://github.com/rancher/rancher/releases/tag/v1.6.0)有关此升级的期望。

根据您如何安装Rancher服务器，升级步骤可能会有所不同。

- [牧场服务器 - 单个容器（非HA）](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/index.md#single-container)
- [Rancher服务器 - 单个容器（非HA） - 外部数据库](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/index.md#single-container-external-database)
- [Rancher服务器 - 单容器（非HA） - 绑定的MySQL卷](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/index.md#single-container-bind-mount)
- [Rancher服务器 - 完全主动/主动HA](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/index.md#multi-nodes)
- [Rancher服务器 - 无Internet访问](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/index.md#rancher-server-with-no-internet-access)

> **注意：**如果您在原始的Rancher服务器设置中设置了任何环境变量或在[ldap证书](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#enabling-active-directory-or-openldap-for-tls)中传递，则需要在任何新命令中添加这些环境变量或证书。

### 牧场服务器标签

Rancher服务器有2个不同的标签。对于每个主要版本标签，我们将提供特定版本的文档。

- `rancher/server:latest`标签将是我们最新的开发版本。这些构建将通过我们的CI自动化框架进行验证。这些版本不适用于部署在生产中。
- `rancher/server:stable`标签将是我们最新的稳定发布版本。此标签是我们推荐用于生产的版本。

请不要使用任何带有`rc{n}`后缀的版本。这些`rc`构建意味着Rancher团队测试构建。

### 基础设施服务

在Rancher服务器升级后，您的[基础架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)可能有可用的升级。升级Rancher服务器后，我们建议您检查基础架构堆栈，以查看是否有堆栈可用。如果有升级可用，请一次一个升级这些堆栈。请完成升级，然后继续升级下一个基础架构堆栈。

### 牧场主

每个Rancher代理版本被固定为Rancher服务器版本。如果升级Rancher服务器和Rancher代理需要升级，我们将自动将代理升级到最新版本的Rancher代理。

### 升级单个容器（非HA）

如果您在**不**使用外部数据库或绑定的MySQL卷的**情况下**启动Rancher服务器，则Rancher服务器数据库位于Rancher服务器容器中。我们将使用正在运行的Rancher服务器容器来创建一个数据容器。该数据容器将用于使用a启动新的Rancher服务器容器`--volumes-from`。或者，您可以将数据库从容器中复制到主机上的目录，并绑定挂接数据库。

1. 停止容器

   ```
   $ docker stop < container_name_of_original_server >
   ```

2. 创建一个`rancher-data`容器。注意：如果您以前已经升级并且已经有一个`rancher-data`容器，则可以跳过此步骤。

   ```
   $ docker create --volumes-from < container_name_of_original_server > \
    --name rancher -data rancher / server：< tag_of_previous_rancher_server >
   ```

3. 拉出Rancher Server的最新图像。注意：如果您跳过此步骤并尝试运行`latest`图像，则不会自动拉出更新的图像。

   ```
   $ docker拉牧场/服务器：最新
   ```

4. 使用容器中的数据库启动一个新的Rancher Server `rancher-data`容器。Rancher中的任何更改将保存在`rancher-data`容器中。如果您在服务器中看到有关日志锁的异常，请参阅[如何修复日志锁](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/faqs/server/#databaselock)。

   > **注意：**根据您有Rancher服务器的时间长短，某些数据库迁移可能需要比预期的更长的时间。请不要在升级过程中停止升级，因为下次升级时会遇到数据库迁移错误。

   ```
   $ docker run -d --volumes-from rancher-data --restart = unless-stopped \
     -p 8080：8080牧场主/服务器：最新
   ```

   ​

5. 删除旧的Rancher服务器容器。注意：如果您只停止容器，如果您使用的是重新启动机器，则容器将重新启动`--restart=always`。`--restart=unless-stopped`升级成功后，建议您使用并移除该容器。

### 升级单个容器（非HA） - 外部数据库

如果您使用外部数据库启动Rancher服务器，则可以停止原始的Rancher服务器容器，并使用相同的[外部DB指令](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#single-container-external-database)启动新版本的Rancher服务器。升级您的Rancher服务器之前，建议您备份外部数据库。新服务器启动并运行后，您可以删除旧的Rancher服务器容器。

### 升级单个容器（非HA） - 绑定的MySQL卷

1. 停止运行的Rancher Server容器。

   ```
   $ docker stop < container_name_of_original_server >
   ```

2. 将数据库文件从服务器容器中复制出来。注意：如果您已将数据库存储在主机上，则可以跳过此步骤。另外，如果DB被复制出容器，那么它将在/ mysql /里面，因为Docker把它复制出来。当装入到容器中时，一定要考虑到这一点。如果你启动了bind mount，你将不需要mysql /。

   ```
   $ docker cp < container_name_of_original_server >：/ var / lib / mysql <主机上的路径>
   ```

3. 现在为文件夹设置UID / GID，以便容器内的mysql用户拥有正确的mysql mount的所有权。

   ```
   $ sudo chown -R 102：105 <主机上的路径>
   ```

4. 启动新的服务器容器。

   ```
   $ docker 运行-d -v < path_on_host >：/ var / lib / mysql -p 8080：8080 \
     --restart = unless-stopped rancher / server：latest
   ```

> **注意：**如果从上一个容器中复制了一个数据库，那么在主机路径的末端，你必须拖尾'/'。否则，目录最终会在错误的地方。

1. 删除旧的Rancher服务器容器。注意：如果您只停止容器，如果您使用的是重新启动机器，则容器将重新启动`--restart=always`。`--restart=unless-stopped`升级成功后，建议您使用并移除该容器。

### 升级HA设置

如果您在[高可用性（HA）](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#multi-nodes)中启动了Rancher服务器，则新的Rancher HA设置将继续使用用于安装原始HA设置的外部数据库。

> **注意：**升级HA设置时，Rancher服务器设置将在升级期间关闭。

1. 升级您的Rancher服务器之前，建议您备份外部数据库。

2. 在HA设置中的每个节点上，停止并删除正在运行的Rancher容器，然后使用您在[安装Rancher服务器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/%7B%7Bsite.baseurl%7D%7D/installing-rancher/installing-server/#multi-nodes)时使用的相同命令启动一个新的Rancher服务器容器，但使用一个新的Rancher服务器映像标记。

   ```
   ＃在所有节点上停止所有Rancher服务器容器 
   $ docker stop < container_name_of_original_server > 
   ＃使用最新的 
   rancher / server版本执行脚本$ docker run -d --restart = unless-stopped -p 8080：8080 -p 9345：9345 rancher / server --db-host myhost.example.com --db-port 3306 --db-user username --db-pass password --db-name cattle --advertise-address < IP_of_the_Node >
   ```

   ​

   \> **注意：**如果要从运行[旧版本的HA]（{{site.baseurl}} / rancher / v1.1 / {{page.lang}} / installation- rancher / installation-server / multi-nodes /），您将需要删除所有正在运行的Rancher HA容器。`$ sudo docker rm -f $（sudo docker ps -a | grep rancher | awk {'print $ 1'}）`

### 没有互联网访问的牧场服务器

没有互联网的用户将需要下载最新的[基础架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)映像，以便升级成功。没有最新默认模板中的图像，基础架构服务将无法升级。