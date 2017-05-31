---
title: Rancher CLI Commands and Options
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 命令和选项

------

Rancher CLI能够处理Rancher中的环境，主机，堆栈，服务和容器。

### Rancher CLI命令

| 名称                   | 描述                                       |
| -------------------- | ---------------------------------------- |
| `catalog`            | [目录操作](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-catalog-refercnce) |
| `config`             | [设置客户端配置](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-config-refercnce) |
| `docker`             | [在主机上运行docker CLI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-docker-refercnce) |
| `cnvironmcnt`， `cnv` | [与环境交互](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-cnvironmcnt-refercnce) |
| `evcnts`， `evcnt`    | [显示资源更改事件](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-evcnts-refercnce) |
| `exec`               | [在容器上运行命令](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-exec-refercnce) |
| `export`             | [将堆栈的配置yml导出为tar存档或本地文件](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-export-refercnce) |
| `hosts`， `host`      | [主机操作](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-hosts-refercnce) |
| `logs`               | [获取容器的日志](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-logs-refercnce) |
| `ps`                 | [显示服务/容器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-ps-refercnce) |
| `restart`            | [重新启动服务，容器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-restart-refercnce) |
| `rm`                 | [删除服务，容器，堆栈，主机，卷](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-rm-refercnce) |
| `run`                | [运行服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-run-refercnce) |
| `scale`              | [设置运行服务的容器数](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-scale-refercnce) |
| `ssh`                | [SSH进入主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-ssh-refercnce) |
| `stacks`， `stack`    | [堆栈上的操作](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-stacks-refercnce) |
| `start`， `activate`  | [启动或激活服务，容器，主机，堆栈](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-startactivate-refercnce) |
| `stop`， `deactivate` | [停止或停用服务，容器，主机，堆栈](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-stopdeactivate-refercnce) |
| `up`                 | [带上所有的服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-up-refercnce) |
| `volumes`， `volume`  | [卷操作](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-volumes-refercnce) |
| `inspect`            | [查看服务，容器，主机，环境，堆栈，卷的详细信息](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-inspect-refercnce) |
| `wait`               | [等待资源服务，容器，主机，堆栈，机器，projectTemplate](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/index.md#rancher-wait-refercnce) |
| `help`               | 显示一个命令列表或一个命令的帮助                         |

### Rancher CLI全局选项

无论何时使用该`rancher`命令，都可以使用不同的全局选项。

| 名称                          | 描述                                       |
| --------------------------- | ---------------------------------------- |
| `--debug`                   | 调试日志                                     |
| `--config`价值，`-c`价值         | 客户端配置文件（默认$ {HOME} /。rancher / cli.json）[$ RANCHER_CLIcnT_CONFIG] |
| `--cnvironmcnt`价值，`--cnv`价值 | 环境名称或ID [$ RANCHER_cnVIRONMcnT]          |
| `--url` 值                   | 指定Rancher API端点URL [$ RANCHER_URL]       |
| `--access-key` 值            | 指定Rancher API访问密钥[$ RANCHER_ACCESS_KEY]  |
| `--secret-key` 值            | 指定Rancher API密钥[$ RANCHER_SECRET_KEY]    |
| `--host` 值                  | 用于docker命令的主机[$ RANCHER_DOCKER_HOST]     |
| `--wait`， `-w`              | 等待资源达到休息状态                               |
| `--wait-timeout` 值          | 超时秒数等待（默认值：600）                          |
| `--wait-state` 值            | 状态等待（活跃，健康等）                             |
| `--help`， `-h`              | 显示帮助                                     |
| `--version`， `-v`           | 打印版本                                     |

#### 等待资源

有一个全局标志，即`--wait`或`-w`可以用于命令达到静止状态。在使用Rancher命令编写脚本时，使用`-w`允许您等待资源准备就绪，然后再转到下一个命令。

默认情况下，等待的超时时间为十分钟，但如果要更改等待的超时时间，则可以使用`--wait-timeout`更改CLI错误之前的时间。

您还可以通过使用来定义退出之前资源的具体状态`--wait-state`。

### 牧场主目录参考

该`rancher catalog`命令提供有关目录模板的操作。

#### 选项

| 名称               | 描述                                       |
| ---------------- | ---------------------------------------- |
| `--quiet`， `-q`  | 只显示ID                                    |
| `--format` 值     | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |
| `--system`， `-s` | 显示系统模板，而不是用户                             |

#### 子命令

| 名称        | 描述                       |
| --------- | ------------------------ |
| `ls`      | `List catalog templates` |
| `install` | 安装目录模板                   |
| `help`    | 显示一个命令列表或一个命令的帮助         |

#### 牧场主目录

该`rancher catalog ls`命令列出环境中的所有模板。

##### 选项

| 名称               | 描述                                       |
| ---------------- | ---------------------------------------- |
| `--quiet`， `-q`  | 只显示ID                                    |
| `--format` 值     | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |
| `--system`， `-s` | 显示系统模板，而不是用户                             |

```
＃列出所有目录模板
$ rancher目录ls
＃列出运行kubernetes的环境中的所有目录模板
$ rancher --cnv k8scnv目录ls
＃列出系统模板的目录模板 
$ rancher catalog ls --system
```

#### Rancher目录安装

该`rancher catalog install`命令将目录模板安装到您的环境中。

##### 选项

| 名称                  | 描述       |
| ------------------- | -------- |
| `-answers`价值，`-a`价值 | 回答文件     |
| `--name` 值          | 要创建的堆栈名称 |
| `--system`， `-s`    | 安装系统模板   |

```
＃安装目录
$ rancher目录安装库/ route53：v0.6.0-rancher1 --name route53
＃安装目录并将其标注为系统模板 
$ rancher catalog install library / route53：v0.6.0-rancher1 --name route53 --system
```

### 牧场配置参考

该`rancher config`命令[用Rancher服务器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli/#configuring-the-rancher-command-line-interface)设置您的[配置](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli/#configuring-the-rancher-command-line-interface)。

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

#### 选项

| 名称        | 描述                                |
| --------- | --------------------------------- |
| `--print` | `Print the currcnt configuration` |

如果要打印现有配置，可以使用`----print`来显示现有配置。

```
＃清除现有的Rancher配置 
$ rancher config --print
```

### Rancher Docker参考

该`rancher docker`命令允许您在特定主机上运行任何Docker命令。使用``$ RANCHER_DOCKER_HOST - 主机`to run Docker commands. Use``或` - 主机`选择不同的主机。

```
$ rancher --host 1h1 docker ps
```

#### 选项

| 名称              | 描述                 |
| --------------- | ------------------ |
| `--help-docker` | 显示 `docker --help` |

> **注意：**如果环境变量`RANCHER_DOCKER_HOST`未设置，则需要传入`--host`以指定要使用的主机。

### 牧场主环境参考

该`rancher cnvironmcnt`命令允许您与环境交互。如果您使用帐户API密钥，您将能够创建和更新环境。如果您使用环境API密钥，您将无法创建或更新其他环境，您将只能看到现有的环境。

#### 选项

| 名称              | 描述                                       |
| --------------- | ---------------------------------------- |
| `--all`， `-a`   | 显示停止/不活动和最近删除的资源                         |
| `--quiet`， `-q` | 只显示ID                                    |
| `--format` 值    | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |

#### 子命令

| 名称                      | 描述               |
| ----------------------- | ---------------- |
| `ls`                    | 列表环境             |
| `create`                | 创建一个环境           |
| `templates`， `template` | 与环境模板交互          |
| `rm`                    | 删除环境             |
| `deactivate`            | 停用环境             |
| `activate`              | 激活环境             |
| `help`                  | 显示一个命令列表或一个命令的帮助 |

#### 牧场主

该`rancher cnv ls`命令列出了Rancher设置中的所有环境。

##### 选项

| 名称              | 描述                                       |
| --------------- | ---------------------------------------- |
| `--all`， `-a`   | 显示停止/不活动和最近删除的资源                         |
| `--quiet`， `-q` | 只显示ID                                    |
| `--format` 值    | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |

```
$ rancher cnv ls
ID NAME ORCHESTRATION STATE CREATED
1a5默认牛活动于08-15T19：20：46Z
1a6 k8scnv Kubernetes active 2016-08-17T03：25：04Z
＃仅列出环境的ID
$ rancher cnv ls -q
1A5
1A6
```

#### Rancher cnv创建

该`rancher cnv create`命令创建不同业务流程类型的新环境。默认情况下，编排类型将为牛。

##### 选项

| 名称                    | 描述             |
| --------------------- | -------------- |
| `--template`价值，`-t`价值 | 创建环境模板（默认为“牛”） |

```
＃创建一个环境
$ rancher cnv create newCattlecnv
＃创建一个kubernetes环境 
$ rancher cnv create -t kubernetes newk8scnv
```

#### Rancher cnv模板

该`rancher cnv template`命令允许您导出和导入模板到Rancher。

##### 选项

| 名称              | 描述                                       |
| --------------- | ---------------------------------------- |
| `--all`， `-a`   | 显示停止/不活动和最近删除的资源                         |
| `--quiet`， `-q` | 只显示ID                                    |
| `--format` 值    | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |

##### 子命令

| 名称       | 描述               |
| -------- | ---------------- |
| `export` | 将环境模板导出到STDOUT   |
| `import` | 从文件导入环境模板        |
| `help`   | 显示一个命令列表或一个命令的帮助 |

#### 牧场人环境

该`rancher cnv rm`命令删除环境。您可以使用环境名称或环境ID进行删除。

```
＃按名称删除环境
$ rancher cnv rm newk8scnv
＃通过ID 
$ rancher cnv rm 1a20删除环境
```

#### Rancher cnv停用

该`rancher cnv deactivate`命令禁用环境。您可以使用环境名称或环境ID选择要更新的环境。

#### Rancher cnv激活

该`rancher cnv activate`命令激活环境。您可以使用环境名称或环境ID选择要更新的环境。

### 牧场活动参考

该`rancher evcnts`命令列出了Rancher服务器中发生的所有活动事件。

#### 选项

| 名称                  | 描述                                       |
| ------------------- | ---------------------------------------- |
| `--format` 值        | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |
| `--reconnect`， `-r` | 重新连接错误                                   |

### 牧师执行参考

该`rancher exec`命令允许您直接执行Rancher中的任何容器。用户不需要知道容器的主机。他们只需要知道Rancher设置中的容器ID，例如`1i1`，`1i788`）。

```
＃执行到一个容器 
$ rancher exec -i -t 1i10
```

#### 选项

| 名称              | 描述                      |
| --------------- | ----------------------- |
| `--help-docker` | 显示 `docker exec --help` |

后`rancher exec`命令找到容器，它运行`docker exec`特定的主机和容器上的命令。要显示该`docker exec`命令的帮助，您可以传递一个选项来显示`docker exec --help`。

```
＃显示docker exec --help 
$ rancher exec --help-docker
```

### 牧场导出参考

该`rancher export`命令将导出`docker-compose.yml`并`rancher-compose.yml`用于堆作为一个tar归档。

#### 选项

| 名称                | 描述                           |
| ----------------- | ---------------------------- |
| `--file`价值，`-f`价值 | 写入一个文件，而不是本地文件，使用 - 写入STDOUT |
| `--system`， `-s`  | 如果导出整个环境，则包括系统               |

```
＃提供泊坞窗，compose.yml和牧场主，compose.yml为
＃所有服务堆栈作为一个tar归档 
$牧场主出口 mystack > files.tar
$ rancher export -f files.tar mystack
```

### 牧场主机参考

该`rancher hosts`命令允许您与环境中的主机进行交互。

#### 选项

| 名称              | 描述                                       |
| --------------- | ---------------------------------------- |
| `--all`， `-a`   | 显示停止/不活动和最近删除的资源                         |
| `--quiet`， `-q` | 只显示ID                                    |
| `--format` 值    | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |

#### 子命令

| 名称       | 描述   |
| -------- | ---- |
| `ls`     | 列出主机 |
| `create` | 创建主机 |

#### 牧场主

该`rancher hosts ls`命令列出所有主机。

##### 选项

| 名称            | 描述               |
| ------------- | ---------------- |
| `--all`， `-a` | 显示停止/不活动和最近删除的资源 |

```
`--quiet`, `-q` |     Only display IDs

```

`--format`值| `json`或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％}

```
$ rancher主持ls
ID HOSTNAME STATE IP
1h1 host-1 active 111.222.333.444
1h2 host-3 active 111.222.333.555
1h3 host-2 active 111.222.333.666
1h4 host-4 active 111.222.333.777
1h5 host-5 active 111.222.333.888
1h6 host-6 active 111.222.333.999
＃只显示主机ID
$ rancher主机ls -q
1H1       
1H2      
1H3      
1H4       
1H5       
1H6
```

#### 牧场主创建

该`rancher hosts create`命令允许您创建[主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)。在创建主机时，您将调用Docker Machine命令，并且需要传递Docker Machine驱动程序所需的相同选项。

### 牧场主日志参考

将`rancher logs`允许你通过容器ID或容器名称来获取一个容器的日志。

#### 选项

| 名称                   | 描述                  |
| -------------------- | ------------------- |
| `--service`， `-s`    | 显示服务日志              |
| `--sub-log`          | 显示服务子日志             |
| `--follow`， `-f`     | 遵循日志输出              |
| `--tail` 值           | 从日志结束显示的行数（默认值：100） |
| `--since` 值          | 从时间戳显示日志            |
| `--timestamps`， `-t` | 显示时间戳               |

```
＃使用容器ID 
$ rancher日志获取最后50行的日志--tail 50 < ID > 
＃使用容器名称尾部日志 
$ rancher logs -f < stackName > / < serviccname >
```

### 牧师ps参考

该`rancher ps`命令显示Rancher中的所有服务或容器。如果您没有在`rancher ps`命令中传入任何选项，该命令将返回环境中所有服务的列表。

#### 选项

| 名称                   | 描述                                       |
| -------------------- | ---------------------------------------- |
| `--all`， `-a`        | 显示停止/不活动和最近删除的资源                         |
| `--system`， `-s`     | 显示系统资源                                   |
| `--containers`， `-c` | 显示容器                                     |
| `--quiet`， `-q`      | 只显示ID                                    |
| `--format` 值         | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |

```
＃列出所有的服务
$ rancher ps
ID类型名称图像状态规模cnDPOINTS细节
1S1服务的默认/博客鬼激活3等待了 [例如：Default_blog_3。实例状态：存储：下载
 ＃列出所有容器而不是服务
$ rancher ps -c
ID NAME IMAGE STATE HOST DETAIL
1i1 Default_blog_1 ghost running 1h1       
1i2 Default_blog_2 ghost running 1h2       
1i3 Default_blog_3 ghost running 1h3  
```

该`detail`列将提供服务的当前状态。

### 牧师重新启动参考

您可以使用Rancher重新启动任何主机，服务或容器`rancher restart`。

#### 选项

| 名称               | 描述                         |
| ---------------- | -------------------------- |
| `--type` 值       | 限制重新启动到特定类型（服务，容器）         |
| `--batch-size` 值 | 一次重新启动的容器数（默认值：1）          |
| `--interval` 值   | 在重新启动之间等待的间隔（毫秒）（默认值：1000） |

```
＃重新启动服务的ID，容器，主机 
$ rancher restart < ID > 
＃按服务名称重新启动，容器和主机 
$rancherrestart < stackName > / < serviccname >
```

> **注意：**服务名称将始终包含堆栈名称，以确保我们引用正确的服务。

### 牧场主rm参考

该`rancher rm`命令从Rancher中删除资源，即主机，堆栈，服务，容器或卷。

#### 选项

| 名称             | 描述                |
| -------------- | ----------------- |
| `--type` 值     | 限制删除到特定类型         |
| `--stop`， `-s` | 删除前，如果需要，先停止或停用资源 |

```
$ rancher rm < ID >
```

### 牧场主运行参考

该`run`命令将使用1个容器的规模部署一个服务到Rancher。创建服务时，如果要将服务放置在特定的堆栈中，则需要传入`--name`并提供`stackName/serviccname`。如果使用no `--name`，那么它将服务放在`Default`堆栈中，并提供一个docker提供的名称。

```
$ rancher run --name App2 / app nginx
 ＃ CLI返回新创建的服务的服务ID
1S3
$ rancher -i -t --name serviceA ubuntu：14.04.3
1S4
```

如果选择将端口发布到主机，则必须具有可用端口的主机。Rancher会自动将该容器安排在具有该端口的主机上。

```
$ rancher -p 2368：2368  -  name blog ghost
1S5
```

### 牧场大小参考

默认情况下，当您启动服务时`rancher run`，服务的比例将为1.您可以使用该`rancher scale`命令增加任何服务的比例。您可以按名称，即`stackName/serviccname`服务ID或服务ID 选择服务。

```
$ rancher scale < stackName > / < serviccname > = 5 < serviceID > = 3
```

### 牧师ssh参考

将`rancher ssh`让你ssh到由UI创建的任何主机。它不会允许您将ssh添加到由[自定义](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/commands/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)命令添加的主机中。

```
$ rancher ssh < hostID >
```

### 牧场主堆栈参考

该`rancher stacks`命令与环境中的堆栈进行交互。

#### 选项

| 名称               | 描述                                       |
| ---------------- | ---------------------------------------- |
| `--system`， `-s` | 显示系统资源                                   |
| `--quiet`， `-q`  | 只显示ID                                    |
| `--format` 值     | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |

#### 命令

| 名称       | 描述     |
| -------- | ------ |
| `ls`     | 列出堆栈   |
| `create` | 创建一个堆栈 |

#### 牧场堆栈

该`rancher stacks ls`命令列出所选环境中的堆栈。

##### 选项

| 名称               | 描述                                       |
| ---------------- | ---------------------------------------- |
| `--system`， `-s` | 显示系统资源                                   |
| `--quiet`， `-q`  | 只显示ID                                    |
| `--format` 值     | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |

```
＃列出所有堆栈
$ rancher堆栈ls
ID名称状态目录系统细节
1e1 zookeeper健康目录：//社区：zookeeper：1    false      
1e2默认降级                                      假      
1e3 App1健康                                       假     
＃仅列出堆栈ID
$ rancher堆栈ls -q
1E1
1E2
1E3
```

#### 牧场堆栈创建

该`rancher stacks create`命令创建新的堆栈。该堆可以是空的或从创造`docker-compose.yml`和`rancher-compose.yml`。

##### 选项

| 名称                           | 描述                                       |
| ---------------------------- | ---------------------------------------- |
| `--start`                    | 在创建时启动堆栈                                 |
| `--system`， `-s`             | 创建一个系统堆栈                                 |
| `--empty`， `-e`              | 创建一个空堆栈                                  |
| `--quiet`， `-q`              | 只显示ID                                    |
| `--docker-compose`价值，`-f`价值  | Docker撰写文件（默认值：“docker-compose.yml”）     |
| `--rancher-compose`价值，`-r`价值 | Rancher Compose文件（默认值：“rancher-compose.yml”） |

```
＃创建一个空堆栈
$ rancher堆栈创建NewStack -e
＃从docker-compose和rancher-compose文件创建一个堆栈
＃另外，在创建时启动堆栈 
$ rancher stacks create NewStack -f dc.yml -r rc.yml --start
```

### 牧场主启动/激活参考

该`rancher start`或`rancher activate`命令激活特定的资源类型，即主机，服务或容器。

#### 选项

| 名称         | 描述                        |
| ---------- | ------------------------- |
| `--type` 值 | 限制启动/激活到特定类型（服务，容器，主机，堆栈） |

```
＃从服务ID，容器，主机 
开始$ rancher start < ID > 
＃以服务名称，容器和主机名开始 
$rancherstart < stackName > / < serviccname >
```

> **注意：**服务名称将始终包含堆栈名称，以确保我们引用正确的服务。

### 牧场主停止/停用参考

该`rancher stop`或`rancher deactivate`命令将停用特定的资源类型，即主机，服务或容器。

#### 选项

| 名称         | 描述                       |
| ---------- | ------------------------ |
| `--type` 值 | 限制停止/停用特定类型（服务，容器，主机，堆栈） |

```
＃停止服务的ID，容器，主机 
$ rancher stop < ID > 
＃停止服务名称，容器，主机 
$rancherstop < stackName > / < serviccname >
```

> **注意：**服务名称将始终包含堆栈名称，以确保我们引用正确的服务。

### 牧场人参考

该`rancher up`命令类似于Docker Compose `up`命令。

#### 选项

| 名称                                    | 描述                                       |
| ------------------------------------- | ---------------------------------------- |
| `--pull`， `-p`                        | 在进行升级之前，请先对所有已有图像的主机进行图像拉扯               |
| `-d`                                  | 不要阻止和登录                                  |
| `--upgrade`，`-u`，`--recreate`         | 如果服务已更改，请升级                              |
| `--force-upgrade`， `--force-recreate` | 升级不管服务是否改变                               |
| `--confirm-upgrade`， `-c`             | 确认升级成功并删除旧容器                             |
| `--rollback`， `-r`                    | 回滚到以前部署的版本                               |
| `--batch-size` 值                      | 一次升级的容器数量（默认值：2）                         |
| `--interval` 值                        | 更新间隔（以毫秒为单位）（默认值：1000）                   |
| `--rancher-file` 值                    | 指定一个备用的Rancher组合文件（默认值：rancher-compose.yml） |
| `--cnv-file`价值，`-e`价值                 | 指定读取环境变量的文件                              |
| `--file`价值，`-f`价值                     | 指定一个或多个替代撰写文件（默认值：docker-compose.yml）[$ COMPOSE_FILE] |
| `--stack`价值，`-s`价值                    | 指定备用项目名称（默认值：目录名）                        |

```
＃最后添加-d，以不阻止并记录 
$rancherup -s < stackName > -d
```

### 牧场卷参考

该`rancher volumes`命令允许您与Rancher中的卷进行交互。

#### 选项

| 名称              | 描述                                       |
| --------------- | ---------------------------------------- |
| `--all`， `-a`   | 显示停止/不活动和最近删除的资源                         |
| `--quiet`， `-q` | 只显示ID                                    |
| `--format` 值    | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |

#### 命令

| 名称       | 描述   |
| -------- | ---- |
| `ls`     | 列出卷  |
| `rm`     | 删除卷  |
| `create` | 创建卷  |

#### 牧师卷LS

该`rancher volume ls`命令列出所选环境中的所有卷。

##### 选项

| 名称              | 描述                                       |
| --------------- | ---------------------------------------- |
| `--all`， `-a`   | 显示停止/不活动和最近删除的资源                         |
| `--quiet`， `-q` | 只显示ID                                    |
| `--format` 值    | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％} |

```
$ rancher卷ls
ID名称状态驱动程序详细信息
1v1活跃                   
1v2活跃                   
1v3分离                 
1v4活跃                   
1v5分离                 
1v6分离                 
1v7牧民代理状态活跃      当地         
```

#### 牧场卷

该`rancher volume rm`命令从Rancher中删除卷。

```
$ rancher volume rm < VOLUME_ID >
```

#### 牧场卷创建

该`rancher volume create`命令在Rancher中创建卷。

##### 选项

| 名称           | 描述             |
| ------------ | -------------- |
| `--driver` 值 | 指定卷驱动程序名称      |
| `--opt` 值    | 设置驱动程序特定的键/值选项 |

```
＃在Rancher NFS驱动程序中创建一个新卷 
$ rancher volume create NewVolume --driver rancher-nfs
```

### 牧场主检查参考

在`rancher inspect`资源上提供细节。

#### 选项

| 名称           | 描述                                       |
| ------------ | ---------------------------------------- |
| `--type` 值   | 限制检查具体类型（服务，容器，主机）                       |
| `--links`    | 将URL包含到资源输出中的操作和链接中                      |
| `--format` 值 | `json` 或自定义格式：{％raw％} {{.Id}} {{.Name}} {％cndraw％}（默认值：“json”） |

```
＃检查服务的ID，容器，主机 
$ rancher inspect < ID > 
＃根据服务名称，容器，主机 
检查$rancherinspect < stackName > / < serviccname >
```

> **注意：**服务名称将始终包含堆栈名称，以确保我们引用正确的服务。

### 牧师等待参考

该`rancher wait`命令等待资源完成其操作。这对于自动化Rancher命令非常有用，以便您可以在脚本中添加等待资源准备好进行更多操作。

```bash
$ rancher start 1i1
$ rancher wait 1i1
```
