---
title: Upgrading using Rancher Compose
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 升级服务

------

启动[服务后](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)，您可能已经决定要更改服务以更新应用程序。例如，图像可能已更新，并且您要启动服务的更新版本。由于Docker容器是不可变的，为了更改您的服务，您将需要销毁旧容器并启动新的容器。Rancher提供了两种升级方法。推荐的[在役升级](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/upgrading/index.md#in-service-upgrade)允许您停止旧容器，并以相同的服务名称启动新的容器。在线升级是[UI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/upgrading/index.md#upgrading-services-in-the-ui)中唯一支持的类型，而[Rancher Compose](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/upgrading/index.md#upgrading-services-with-rancher-compose)也支持升级。在[滚动升级](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/upgrading/index.md#rolling-upgrade)只能通过牧场主撰写的支持，

> **注意：**如果您希望增加服务规模，您可以在UI中编辑服务规模或使用`rancher-compose scale <serviceName>=<newNumber>`。

### 在线升级

#### 在UI中升级服务

任何服务都可以通过UI进行升级。与[启动服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services)类似，服务将具有之前选择的所有Docker选项，您将能够将其更新为任何新值。还有几个升级选项：

- **批量大小**：要从旧服务中停止并从新服务一次启动的容器数。
- **批次间隔**：从旧服务停止集装箱和启动新服务中的集装箱之间的时间。
- **启动行为**：默认情况下，旧容器将在开始启动新容器之前停止。您可以选择允许在旧容器停止之前启动新容器。

选择升级选项并定义新服务后，选择**升级**。

一旦所有的旧集装箱已经停止并且新的集装箱已经起作用，服务将处于*升级*状态。在这个阶段，您应该测试新服务，以确保在选择**完成升级**之前正常工作。如果您的新服务不是您预期的，您可以**回滚**到原始服务。

#### 使用Rancher Compose升级服务

使用Rancher Compose，如果您选择进行在职升级，则现有服务中的容器将升级到提供`docker-compose.yml`的服务，并删除服务中的原始容器。为了执行此操作，`rancher-compose up`命令中传递升级选项。通过添加该`--upgrade`选项，将 `docker-compose.yml`将堆栈中具有相同名称的任何服务升级到新的服务定义。就像UI一样，在线升级是一个两步的过程，因为我们要求用户确认升级是可以的。

##### 步骤1：执行升级

对于升级，您可以升级整个堆栈或特定的服务 `docker-compose.yml`

```
＃升级堆栈中的所有服务（即docker-compose.yml）
$ rancher-compose up  - 升级
＃升级特定服务（即service1和service2）
$ rancher-compose up --upgrade service1 service2
＃强制升级，即使服务的docker-compose.yml没有更改 
$ rancher -compose up --force-upgrade
```

**升级选项**

| 选项                                    | 描述                         |
| ------------------------------------- | -------------------------- |
| `--pull`， `-p`                        | 在进行升级之前，请先对所有已有图像的主机进行图像拉扯 |
| `-d`                                  | 不要阻止和登录                    |
| `--upgrade`，`-u`，`--recreate`         | 如果服务已更改，请升级                |
| `--force-upgrade`， `--force-recreate` | 升级不管服务是否改变                 |
| `--confirm-upgrade`， `-c`             | 确认升级成功并删除旧容器               |
| `--rollback`， `-r`                    | 回滚到以前部署的版本                 |
| `--batch-size` “2”                    | 一次升级的容器数                   |
| `--interval` “1000”                   | 更新间隔（以毫秒为单位）               |

**拉**

升级过程中，您还可以`docker pull`在部署映像之前执行。由于主机通常会使您的映像已经被缓存，所以您可能需要添加`--pull`选项来拉取主机上最新的映像。

```
＃在升级过程中，强制拉动主机以获取最新图像 
$ rancher-compise up --upgrade --pull
```

**批量大小**

默认情况下，新服务的容器在升级过程中一次启动。您可以通过传入`--batch-size`和更改数字来更改要升级的容器数量。这个数字是在升级过程中在新服务中启动的容器数量。

```
＃ service2的容器将以3 
＃的批次启动，直到达到 
service2的规模达到 $ rancher-compise up --upgrade --batch-size 3
```

**间隔**

默认情况下，在升级过程中，新服务上的容器已启动，旧服务上的容器被删除之后，等待2秒。您可以通过传入间隔`--interval`和间隔的毫秒数来覆盖此间隔。

```
＃设置时间间隔为30秒，这是之间的时间
＃当新服务的集装箱已经开始和
＃当老服务的容器取出 
$牧场主，撰写升级服务1服务2 --interval “ 30000 ”
```

**停止旧集装箱之前开始新的集装箱**

默认情况下，在线升级会停止现有容器，然后启动新的容器。要在停止旧容器之前启动新容器，必须在其中提供其他内容`rancher-compose.yml`。

```
版本：' 2 '
服务：
   myservice：
     upgrade_strategy：
       start_first：true
```

```
＃用上面的yaml升级myservice将首先启动新服务 
$ rancher-compise up --upgrade myservice
```

##### 步骤2：确认升级

验证升级通过验证后，您需要确认升级完成。需要确认，因为它允许用户在必要时回滚到旧版本。**确认升级后，不能再回滚到旧版本。**

```
＃确认升级完成并成功 
$ rancher -compose up  - 升级--confirm-upgrade
```

**滚回来**

执行升级后，如果升级后的服务有问题，Rancher会提供回滚到旧服务的功能。只有当您**尚未**确认升级时，才能完成此操作。

```
＃回滚到以前的版本 
$ rancher-compose up --upgrade --rollback
```

### 滚动升级

滚动升级是创建新服务来替换现有的旧服务。而不是删除容器并且新容器在同一服务中启动，因此将创建一个全新的服务。这种类型的升级只支持Rancher Compose。执行滚动升级到新服务的命令很简单：

```
$ rancher-compose upgrade service1 service2
```

`service2`是您要在Rancher中启动的新服务的名称。`service1`是您想要在Rancher中停止的服务的名称。由于`service2`部署，在集装箱`service1`从牧场主被删除。该服务不会从Rancher中删除，但服务的规模将被提高到0。

在里面`docker-compose.yml`，这两个服务名称都需要包括在内。因为`service1`，只有Rancher Compose才能在Rancher中找到服务所需的服务名称。因为`service2`，服务将需要填充所有的服务细节，以使Rancher Compose启动服务。

#### 例 `docker-compose.yml`

```
版本：“ 2 ”
服务：
   客服1：
   ＃没有什么需要真正放在文件作为服务已运行
  服务2：
     图片：WordPress的
    链接：
     ＃定义任何出站链接到堆栈中其它服务 
    - DB：MySQL的
```

默认情况下，链接到`service1`（即入站链接）的任何负载平衡器或服务将自动更新为新的链接`service2`。如果您不希望创建这些链接，您可以[设置一个选项，使其不被创建](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/upgrading/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/upgrading/#updating-inbound-links)。

> **注意：**`rancher-compose.yml`升级服务时不需要使用文件。默认情况下，新服务的规模基于旧服务的规模。您可以通过传递`--scale`选项来覆盖此比例。

#### 升级过程中的缩放

新服务和旧服务的容器总数超过新服务的最终规模之前，容器不会从旧服务中删除。

#### 例：

```
$ rancher-compose upgrade service1 service2  -  scale 5
```

`service1`有2个集装箱的规模，正在升级`service2`，最终将有5个集装箱。

| `service1` | `service2` | 笔记                                       |
| ---------- | ---------- | ---------------------------------------- |
| 2          | 0          | `service1` 正在运行2个容器。                     |
| 2          | 2          | `service2`一次启动2个容器（默认[批量大小](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/upgrading/index.md#batch-size)）。 |
| 2          | 4          | `service2` 启动另外2个容器。                     |
| 1          | 4          | 在上一步之后，新旧集装箱的总和（6）大于最终规模（5），`service1`停止并移除其中一个集装箱，以维持最终的5个等级。 |
| 1          | 五          | `service2` 仅开始1个集装箱达到5的最终规模。             |
| 0          | 五          | `service1` 删除最后运行的容器。                    |

#### `Upgrade`命令选项

使用该`upgrade`命令，可以传入几个选项来自定义升级。

| 选项                  | 描述                         |
| ------------------- | -------------------------- |
| `--batch-size` “2”  | 一次升级的容器数                   |
| `--scale` “-1”      | 运行容器的最终数量                  |
| `--interval` “2000” | 更新间隔（以毫秒为单位）               |
| `--update-links`    | 更新目标服务上的入站链接               |
| `--wait`， `-w`      | 等待升级完成                     |
| `--pull`， `-p`      | 在进行升级之前，请先对所有已有图像的主机进行图像拉扯 |
| `--cleanup`， `-c`   | 升级后删除原始服务定义，意味着等待          |

##### 批量大小

默认情况下，新服务的容器在升级过程中一次启动。您可以通过传入`--batch-size`和更改数字来更改要升级的容器数量。这个数字是在升级过程中在新服务中启动的容器数量。

```
＃ service2的容器将以3 
＃的批次启动，直到达到service2 的规模 
$ rancher-compose upgrade service1 service2 --batch-size 3
```

##### 最终量表

默认情况下，新服务的规模基于旧服务的规模。您可以通过传入`--scale`和更改数字来更改新服务的规模。该数字定义了您在新服务中运行的运行容器的最终比例。

```
＃将服务规模2设置为8容器 
$ rancher -compose upgrade service1 service2 --scale 8
```

> **注意：**旧服务的容器将不会根据批量大小删除。容器在特定批次中启动后，旧服务的容器在旧服务和新服务中的总和超过新服务的最终比例时将被停止并删除。

##### 间隔

默认情况下，在升级过程中，新服务上的容器已启动，旧服务上的容器被删除之后，等待2秒。您可以通过传入间隔`--interval`和间隔的毫秒数来覆盖此间隔。

```
＃设置时间间隔为30秒，这是之间的时间
＃当新服务的集装箱已经开始和
＃当老服务的容器取出 
$牧场主，撰写升级服务1服务2 --interval “ 30000 ”
```

##### 更新入站链接

默认情况下，链接**到**旧服务的任何服务也链接到新服务。如果您不希望任何这些服务与新服务相关联，您可以进入，`--update-links="false"`以便不会有新服务的链接。

```
＃在新服务的设置中不要设置链接到旧服务 
$ rancher-compose upgrade service1 service2 --update-links = “ false ”
```

##### 等待升级完成

默认情况下，Rancher Compose将在升级已经传递给Rancher后退出，但升级过程可能尚未完成。通过传递`--wait`或`-w`到`upgrade`命令，牧场主撰写不会退出，直到之后的新服务已经完全启动，旧的服务已经停止。

```
＃等待升级完成 
$ rancher -compose upgrade service1 service2 --wait
```

##### 拉一个新的图像

默认情况下，如果主机上已存在图像，则Rancher Compose将不会拉出图像。通过传递`--pull`或`-p`到`upgrade`命令，牧场主撰写将再次拉动图像即使图像已经在主机上。

```
＃即使图像已经存在，也可以拉一个新图像 
$ rancher-compose upgrade service1 service2 -pull
```

##### 清理原始服务

默认情况下，升级完成后，原始服务将保留在“牧师”中，缩放比例为0。如果您确定您不需要回滚或保存原始服务定义，则可以使用该命令传入`--cleanup`或`-c`选项`upgrade`。此选项意味着`--wait`由Rancher Compose等待升级完成，然后再从Rancher中删除该服务。

```bash
# Cleanup service1 from Rancher after upgrade is complete
$ rancher-compose upgrade service1 service2 --cleanup
```
