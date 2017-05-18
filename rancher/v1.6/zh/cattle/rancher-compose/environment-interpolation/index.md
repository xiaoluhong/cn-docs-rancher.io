---
title: Environment Interpolation in Rancher Compose
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 环境插值

------

使用牧场主撰写，从运行牧场主撰写本机环境变量中的使用`docker-compose.yml`和`rancher-compose.yml`文件。这仅在Rancher Compose命令中支持，而不是在Rancher UI中。

### 如何使用它

使用`docker-compose.yml`和`rancher-compose.yml`文件，您可以引用机器上的环境变量。如果机器上没有环境变量，它将用空白字符串替换变量。Rancher Compose将提供关于哪些环境变量未设置的警告。如果使用图像标签的环境变量，请注意，Rancher Compose不会剥离`:`图像以获取最新的图像。由于图像名称，即`<imagename>:`无效的图像名称，将不会部署容器。确保所有环境变量在机器上存在和有效，由用户决定。

#### 例

在我们的机器上运行Rancher Compose，我们有一个环境变量`IMAGE_TAG=14.04`。

```bash
# Image tag is set as environment variable
$ env | grep IMAGE
IMAGE_TAG=14.04
# Run Rancher Compose
$ rancher-compose up
```

**Example `docker-compose.yml`**

```yaml
version: '2'
services:
  ubuntu:
    tty: true
    image: ubuntu:$IMAGE_TAG
    stdin_open: true
```

在牧场主，一个`ubuntu`服务将部署一个`ubuntu:14.04`形象。

### 环境插值格式

Rancher Compose支持与Docker Compose相同的格式。

```yaml
version: '2'
services:
  web:
    # unbracketed name
    image: "$IMAGE"

    # bracketed name
    command: "${COMMAND}"

    # array element
    ports:
    - "${HOST_PORT}:8000"

    # dictionary item value
    labels:
      mylabel: "${LABEL_VALUE}"

    # unset value - this will expand to "host-"
    hostname: "host-${UNSET_VALUE}"

    # escaped interpolation - this will expand to "${ESCAPED}"
    command: "$${ESCAPED}"
```
