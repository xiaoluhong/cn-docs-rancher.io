---
title: Commands and Options in Rancher Compose
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 命令和选项

------

Rancher Compose工具就像受欢迎的Docker Compose一样工作，并支持V1版本的 `docker-compose.yml`文件。要启用Rancher支持的功能，您还可以`rancher-compose.yml`扩展和覆盖该功能`docker-compose.yml`。例如，服务规模和健康检查将在`rancher-compose.yml`档案中。

### 牧场主撰写命令

Rancher Compose支持Docker Compose支持的任何命令。

| 名称             | 描述               |
| -------------- | ---------------- |
| `create`       | 创建所有服务，但不要启动     |
| `up`           | 带上所有的服务          |
| `start`        | 开始服务             |
| `logs`         | 获取服务日志           |
| `restart`      | 重新启动服务           |
| `stop`， `down` | 停止服务             |
| `scale`        | 规模服务             |
| `rm`           | 删除服务             |
| `pull`         | 拉图像进行服务          |
| `upgrade`      | 执行服务之间的滚动升级      |
| `help`， `h`    | 显示一个命令列表或一个命令的帮助 |

### 牧场主撰写选项

无论何时使用Rancher Compose命令，都可以使用不同的选项。

| 名称                             | 描述                                       |
| ------------------------------ | ---------------------------------------- |
| `--verbose`， `--debug`         |                                          |
| `--file`，`-f`[--file选项 - 文件选项] | 指定备用撰写文件（默认值：`docker-compose.yml`）[$ COMPOSE_FILE] |
| `--project-name`， `-p`         | 指定备用项目名称（默认值：目录名）                        |
| `--url`                        | 指定Rancher API端点URL [$ RANCHER_URL]       |
| `--access-key`                 | 指定Rancher API访问密钥[$ RANCHER_ACCESS_KEY]  |
| `--secret-key`                 | 指定Rancher API密钥[$ RANCHER_SECRET_KEY]    |
| `--rancher-file`， `-r`         | 指定备用牧场主撰写文件（默认值：`rancher-compose.yml`）   |
| `--env-file`， `-e`             | 指定读取环境变量的文件                              |
| `--help`， `-h`                 | 显示帮助                                     |
| `--version`， `-v`              | 打印版本                                     |

#### 例子

要开始，您可以创建一个简单的`docker-compose.yml`文件和可选的`rancher-compose.yml`文件。如果没有`rancher-compose.yml`文件，则所有服务将以1个容器的比例开始。

##### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   web：
     image：nginx 
  db：
     image：mysql 
    环境：
       MYSQL_ROOT_PASSWORD：test    
```

##### 例 `rancher-compose.yml`

```
＃参考您要扩展的服务
版本： ' 2 '
服务：
   web：
     scale： 2 
  db：
     scale： 1
```

创建文件后，可以将服务启动到Rancher服务器。

```
＃创建和启动没有环境变量的服务并选择一个堆栈
＃如果堆栈不存在于Rancher中，它将在Rancher 
$ rancher-compose -url中创建 -  url URL_of_Rancher --access-key < username_of_environment_api_key > --secret-key < password_of_environment_api_key > -p stack1 up

＃创建并启动已设置环境变量的服务
$ rancher-compose -p stack1 up

＃更改现有服务的规模
$ rancher-compose -p stack1 scale web = 3

＃在docker-compose.yml 
$ rancher -compose -p stack1 up web中启动特定的服务
```

> **注意：**如果不进入`-p <STACK_NAME>`，堆栈名称将是您正在运行Rancher Compose命令的目录。

#### 使用`--env-file`选项

您可以在`--env-file`运行Rancher Compose命令时使用环境变量的文件。

##### 示例`secrets`文件

```
MYSQL_ROOT_PASSWORD=test

```

##### 例 `docker-compose.yml`

```
版本：' 2 '
服务：
   db：
     image：mysql 
    环境：
     ＃就像Docker Compose一样，如果只有一个键，Rancher Compose将解析到
    ＃机器上的值或使用--env-file 
      MYSQL_ROOT_PASSWORD 传递的文件：
```

您可以启动您的服务并传入`secrets`文件。

```
$ rancher-compose --env-file secrets up -d
```

通过传入一个文件并且只有一个环境变量只有一个键，Rancher Compose解析为文件或Rancher Compose运行的机器上的值。如果在文件和机器上设置环境变量，Rancher Compose将使用该文件中的值。

### 命令选项

#### 命令

| 名称                                    | 描述                         |
| ------------------------------------- | -------------------------- |
| `--pull`， `-p`                        | 在进行升级之前，请先对所有已有图像的主机进行图像拉扯 |
| `-d`                                  | 不要阻止和登录                    |
| `--upgrade`，`-u`，`--recreate`         | 如果服务已更改，请升级                |
| `--force-upgrade`， `--force-recreate` | 升级不管服务是否改变                 |
| `--confirm-upgrade`， `-c`             | 确认升级成功并删除旧容器               |
| `--rollback`， `-r`                    | 回滚到以前部署的版本                 |
| `--batch-size "2"`                    | 一次升级的容器数                   |
| `--interval "1000"`                   | 更新间隔（以毫秒为单位）               |

当您`up`使用Rancher Compose 运行命令时，所有任务完成后，该过程将继续运行。如果您希望过程在完成后退出，则需要添加该`-d`选项，即不阻止和记录。

```
＃如果不使用-d标志，Rancher Compose将继续运行，直到Ctrl + C退出
$牧民组成

＃在运行 
$ rancher-compose up -d之后，使用-r标记为rancher-compose退出
```

阅读更多关于[使用Rancher Compose升级的信息](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/commands/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/upgrading/#upgrading-services-with-rancher-compose)。

#### 启动命令

| 名称   | 描述      |
| ---- | ------- |
| `-d` | 不要阻止和登录 |

如果要在完成后要启动进程退出，则需要添加该`-d`选项，即不要阻止和记录。

#### 日志命令

| 名称         | 描述     |
| ---------- | ------ |
| `--follow` | 遵循日志输出 |

#### 重新启动命令

| 名称                   | 描述             |
| -------------------- | -------------- |
| `--batch-size` `"1"` | 立即重新启动的容器数量    |
| `--interval` `"0"`   | 重新启动间隔（以毫秒为单位） |

默认情况下，重新启动服务将逐个重新启动容器。您可以设置重新启动策略的批量大小和间隔。

#### 停止/下降和缩放命令

| 名称                       | 描述             |
| ------------------------ | -------------- |
| `--timeout`， `-t` `"10"` | 指定关闭超时（以秒为单位）。 |

```
＃要更改现有服务的规模
$ rancher-compose -p stack1 scale service1 = 3
```

#### Rm命令

| 名称              | 描述        |
| --------------- | --------- |
| `--force`， `-f` | 允许删除所有服务  |
| `-v`            | 删除与容器关联的卷 |

删除服务时，Rancher Compose只会删除在该服务中找到的服务`docker-compose.yml`。如果Rancher中的堆栈中有更多的服务，那么Rancher Compose不会知道它们存在，所以不会被删除。

此外，如果存在任何剩余的服务，则堆栈组合将不会被删除。

默认情况下，附加到容器的卷将不会被删除。您可以看到所有卷`docker volume ls`。

#### 拉命令

| 名称               | 描述                |
| ---------------- | ----------------- |
| `--cached`， `-c` | 只更新具有缓存图像的主机，不要拉新 |

```
＃为位于环境中的所有主机上的docker-compose.yml文件中的所有服务拉新图像
$ rancher-compose拉

＃为位于已经拥有图像的主机上的docker-compose.yml文件中的所有服务拉新图像 
$ rancher-compose pull --cached
```

> **注意：**不同于`docker-compose pull`，您不会指定要拉出的服务。Rancher Compose查看所有服务，`docker-compose.yml`并为文件中找到的所有服务提取图像。

#### 升级命令

您可以使用Rancher Compose升级您的Rancher服务。请阅读更多关于什么时候和如何[升级您的服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/rancher-compose/commands/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/upgrading/#upgrading-services-with-rancher-compose)。

### 删除服务/容器

默认情况下，Rancher Compose不会删除内容。这意味着如果您`up`连续执行两个命令，则第二个命令`up`不会执行任何操作。这是因为第一个将创建一切并保持运行。即使你不及格`-d`到`up`，牧场主撰写不会删除你的服务。要删除必须使用的服务`rm`。