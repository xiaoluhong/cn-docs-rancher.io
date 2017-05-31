---
title: Service Alias
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 服务别名

------

通过添加服务别名，它可以指向服务别名而不是直接在服务中。

### 在UI中添加服务别名

在堆栈中，您可以通过单击**添加服务**按钮旁边的下拉图标来**添加**服务别名。选择**服务别名**。或者，如果您在堆栈级别查看堆栈，则对于每个特定堆栈，可以看到相同的“ **添加服务”**下拉列表。

您将需要提供一个**名称**，如果需要的**话**，说明服务。该**名称**将是您选择的服务的服务别名。

选择要添加别名的目标。可用目标的列表是已经在堆栈中创建的任何服务。最后点击**创建**。

服务视图中显示别名服务的服务列表。就像我们的服务一样，您需要在服务别名启动之前开始工作。

#### 添加/删除服务

在任何时候，您可以在服务别名中编辑服务。点击服务的下拉菜单中的**编辑**。您将能够向别名添加更多服务或删除现有服务。

### 使用Rancher Compose添加服务别名

服务别名创建一个指向服务的指针。在下面的例子中，`web[.stack-name.rancher.internal]`将解决到的容器的IP地址`web1`和`web2`。这`rancher/dns-service`不是一个实际的图像，但是是必需的`docker-compose.yml`。没有为别名服务创建容器。

#### 例 `docker-compose.yml`

```yaml
version: '2'
services:
  web:
    image: rancher/dns-service
    links:
    - web1
    - web2

  web1:
    image: nginx

  web2:
    image: nginx
```
