---
title: Adding External Service
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 添加外部服务

------

您可能会在Rancher外部托管您想要与Rancher集成的服务。您可以通过添加外部服务将这些服务添加到Rancher。

### 在UI中添加外部服务

在堆栈内，您可以通过单击**添加服务**按钮旁边的下拉图标来**添加**外部服务。选择**外部服务**。或者，如果您在堆栈级别查看堆栈，则对于每个特定堆栈，可以看到相同的“ **添加服务”**下拉列表。

您将需要提供一个**名称**，如果需要的**话**，说明服务。

添加所需的目标。您可以选择外部IP或主机名（即CNAME）。最后点击**创建**。

外部服务IP和主机名将显示在服务中。就像我们的服务一样，您将需要启动外部服务。

#### 添加/删除目标

随时可以编辑外部服务中的目标。点击外部服务的下拉菜单中的**编辑**。您可以添加更多目标或删除现有目标。

### 添加外部服务与Rancher Compose

使用外部服务，您可以设置外部IP **或**域名。这`rancher/external-service`不是一个实际的图像，但是是必需的`docker-compose.yml`。没有为外部服务创建容器。

#### Example `docker-compose.yml`

```yaml
version: '2'
services:
  db:
    image: rancher/external-service
  redis:
    image: redis
```

#### Example `rancher-compose.yml` using external IPs

```yaml
version: '2'
services:
  db:
    external_ips:
    - 1.1.1.1
    - 2.2.2.2

  # Override any service to become an external service
  redis:
    image: redis
    external_ips:
    - 1.1.1.1
    - 2.2.2.2
```

#### Example `rancher-compose.yml` using hostname

```yaml
version: '2'
services:
  db:
    hostname: example.com
```
