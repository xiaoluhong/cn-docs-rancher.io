---
title: FAQS about Rancher Server
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/latest/zh/faqs/server/
---

## 关于Rancher服务器的常见问题

------

### 我正在运行什么版本的Rancher？

Rancher的版本位于我们网站的左下角。如果您点击该版本，您将可以获得其他组件的特定版本。

### 如何在代理服务器之后运行Rancher？

阅读更多关于如何[在代理服务器后面安装Rancher服务器]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//installing-rancher/installing-server/#launching-rancher-server-behind-a-http-proxy)。

### 哪里可以找到Rancher Server容器的详细日志？

在Rancher服务器容器上运行`docker logs`将提供一组基本日志。要获取更详细的日志，您可以执行到Rancher服务器容器并查看日志文件。

```bash
# Exec into the server container
$ docker exec -it <container_id> bash
# Navigate to the location of the cattle logs
$ cd /var/lib/cattle/logs/
$ cat cattle-debug.log
```

在这个文件夹里面会有`cattle-debug.log`和`cattle-error.log`。如果你使用Rancher服务器已经有很多天了，你会发现我们每天都会创建一个新的日志文件。

#### 将Rancher服务器日志复制到主机

以下是将Rancher服务器日志从容器复制到主机的命令。

```
$ docker cp < container_id >：/ var / lib / cattle / logs / local / path
```

### 如何在Rancher服务器容器中导出内部数据库？

您可以使用简单的Docker命令从Rancher服务器容器导出数据库。

```
$ docker exec <CONTAINER_ID_OF_SERVER> mysqldump cattle > dump.sql
```

### 如果Rancher服务器的IP更改了会怎么样？

如果Rancher服务器的IP已更改，您需要使用更新后的信息重新连接主机。

在Rancher中，转到**管理员** - > **设置**，并使用Rancher服务器的更新的URL 更新**主机注册**地址。请注意，它必须包括您启动Rancher服务器的暴露端口。默认情况下，我们已经使用端口建议`8080`在我们的[安装说明]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//installing-rancher/installing-server)。

在[主机注册]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/configuration/settings/#host-registration)地址更新后，进入**基础设施** - > **添加主机** - > **自定义**。用`docker run`命令去添加Rancher agent将使用新信息进行更新。在Rancher服务器[环境中的]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environmcnts)所有主机上运行更新命令。

### 为什么Go-Machine-Service在我的日志中不断重新启动？我该怎么办？

Go-machine-service是一种通过websocket连接连接到Rancher API服务器的微服务器。如果无法连接，则会重新启动并再次尝试。

如果您运行在[单节点]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//installing-rancher/installing-server)设置中，它将使用您为主机注册设置的URL连接到Rancher API服务器。验证可以从Rancher-sever容器内部到达主机注册URL。

```bash
$ docker exec -it <rancher-server_container_id> bash
# Inside the rancher-server container
$ curl -i <Host Registration URL you set in UI>/v1
```
你应该得到一个json响应。如果认证开启，响应代码应为401.如果认证未打开，则响应代码应为200。

验证使用这些变量可以访问Rancher API服务器。通过登录go-machine-service容器并使用您提供给容器的参数进行curl命令来验证连接：

```bash
$ docker exec -it <go-machine-service_container_id> bash
# Inside the go-machine-service container
$ curl -i -u '<value of CATTLE_ACCESS_KEY>:<value of CATTLE_SECRET_KEY>' <value of CATTLE_URL>
```

你应该得到一个json响应和200个响应代码。

如果curl命令失败，则在go-machine-service和Rancher API服务器之间存在连接问题。

如果curl命令没有失败，则问题可能与go-machine-service尝试建立websocket连接而不是普通的http连接有关。如果在go-machine-service和Rancher API服务器之间有一个代理或负载平衡器，请验证它是否被配置为允许websocket连接。

### Rancher服务器正在工作，现在已经大大减缓了。怎么恢复？

很可能有一些任务由于某些原因而停滞不前。如果您能够看到UI 中的**管理** - > **进程**选项卡，您将可以看到**困扰的**内容`Running`。如果这些任务长时间运行（并且失败），则Rancher会最终使用太多的内存来跟踪任务。为了使服务器处于响应状态，您需要添加更多内存。通常，4GB是足够的。

如果再次运行Rancher服务器命令，只需添加一个附加选项`-e JAVA_OPTS="-Xmx4096m"`即可。

```bash
$ docker run -d -p 8080:8080 --restart=unless-stopped -e JAVA_OPTS="-Xmx4096m" rancher/server
```

根据MySQL数据库的设置方式，您可能需要进行[升级]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//upgrading)才能添加附加命令。

如果由于缺少内存而无法看到**Admin** - > **Processes**选项卡，则在启动Rancher服务器后再次使用更多内存，您应该可以看到该选项卡，并开始对运行时间最长的进程进行故障排除。

### Rancher服务器数据库增长太快。

Rancher服务器会自动清理几个数据库表，以防止数据库增长太快。如果您注意到这些表没有足够快速清理，请随时使用我们的API更新默认设置。

默认情况下，两个星期前创建在container_event和service_event表中的任何记录，都将被删除。API中的设置以秒（`1209600`）列出。API中的设置是`evcnts.purge.after.seconds`。

默认情况下，在1天前`process_instance`表中创建的任何记录，则会被删除。API中的设置以秒（`86400`）列出。API中的设置是`process_instance.purge.after.seconds`。

要更新API中的这些设置，请转到该`http://<rancher-server-ip>:8080/v1/settings`页面。搜索要更新的设置，然后单击链接`links -> self`以导航到该设置。点击`Edit`侧页来更改`value`。请记住，**值是以秒为单位。**

### 为什么Rancher Server冻结？或为什么我的升级失败？

如果你开始rancher server但是提示freezes forever，可能是有一个liquibase 数据库锁。在启动时，liquibase执行模式迁移。有一个竞争条件，它可能会留下一个锁定条目，这将阻止后续的靴子。

如果您刚刚升级，并且在Rancher服务器日志中，MySQL数据库上可能存在尚未发布的日志锁定。

```
.... liquibase.exception.LockException：无法获取更改日志锁定。目前被< container_ID >锁定
```

#### 释放数据库锁

> **注意：**请勿释放数据库锁定，除非您对日志锁定有上述**异常**。如果由于数据迁移而升级需要很长时间，那么如果尝试释放数据库锁，您可能会遇到其他迁移问题。

如果你已经按照[升级文件]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/upgrading)创建了数据卷容器，你需要通过`exec`进入`rancher-data`容器并更新 `DATABASECHANGELOGLOCK`表和解除锁定。如果您尚未创建数据容器，则可以通过`exec`进入具有数据库的容器。

```bash
$ sudo docker exec -it <container_id> mysql
```

一旦进入MySQL数据库，就需要访问`cattle`数据库。

```bash
mysql> use cattle;

# 检查表中有一个锁
mysql> select * from DATABASECHANGELOGLOCK;

# 更新删除容器中的锁
mysql> update DATABASECHANGELOGLOCK set LOCKED="", LOCKGRANTED=null, LOCKEDBY=null where ID=1;

#检查锁是否已被移除
mysql> select * from DATABASECHANGELOGLOCK;
+----+--------+-------------+----------+
| ID | LOCKED | LOCKGRANTED | LOCKEDBY |
+----+--------+-------------+----------+
|  1 |        | NULL        | NULL     |
+----+--------+-------------+----------+
1 row in set (0.00 sec)
```
