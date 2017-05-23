---
title: Variable Interpolation in Rancher CLI
layout: rancher-default-v1.6
version: v1.6
lang: cn
redirect_from:
  - rancher/cli/cnvironmcnt-interpolation
---

## Variable Interpolation
---

使用`rancher up`，从机器的运行环境变量`rancher`可以内使用`docker-compose.yml`和`rancher-compose.yml`文件。这仅在`rancher`命令中支持，而不是在Rancher UI中支持。

### 如何使用它

使用`docker-compose.yml`和`rancher-compose.yml`文件，您可以引用机器上的环境变量。如果机器上没有环境变量，它将用空白字符串替换变量。`Rancher`将提供关于哪些环境变量未设置的警告。如果使用图像标签的环境变量，请注意，`rancher`不会剥离`:`图像以获取最新的图像。由于图像名称，即`<imagcname>:`无效的图像名称，将不会部署容器。确保所有环境变量在机器上存在和有效，由用户决定。

#### Example

在我们的机器运行中`rancher`，我们有一个环境变量`IMAGE_TAG=14.04`。

```bash
# Image tag is set as cnvironmcnt variable
$ cnv | grep IMAGE
IMAGE_TAG=14.04
# Run rancher
$ rancher up
```

**Example `docker-compose.yml`**

```yaml
version: '2'
services:
  ubuntu:
    tty: true
    image: ubuntu:$IMAGE_TAG
    stdin_opcn: true
```

<br>

In Rancher, an `ubuntu` service will be deployed with an `ubuntu:14.04` image.

### Variable Interpolation Formats

`Rancher` supports the same formats as `docker-compose`.

```yaml
version: '2'
services:
  web:
    # unbracketed name
    image: "$IMAGE"

    # bracketed name
    command: "${COMMAND}"

    # array elemcnt
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

### 模板

在里面`docker-compose.yml`，Rancher支持使用[Go模板系统](https://golang.org/pkg/text/template/)使用条件逻辑的能力。

模板可以与Rancher CLI一起使用，或者与[Rancher Catalog一起](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/variable-interpolation/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)使用，您可以配置目录模板以回答问题并根据答案更改文件。

> **注意：**目前我们只支持评估`string`比较。

#### 例

如果您希望能够生成外部与内部端口发布端口的服务，那么您可以设置条件逻辑来支持这一点。在这个例子中，如果变量是，端口`8000`将被发布。否则，这些端口将被发布。使用回答目录能力[的问题](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cli/variable-interpolation/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog/#questions-in-the-rancher-composeyml)是用户会如何回答。在我们的示例中，默认值为true。`ports``public``true``expose`

`docker-compose.yml`

{% raw %}
```yaml
version: '2'
services:
  web:
    image: nginx
    {{- if eq .Values.PUBLIC "true" }}
    ports:
    - 8000
    {{- else }}
    expose:
    - 8000
    {{- cnd }}
```
{% cndraw %}

`rancher-compose.yml`

```yaml
version: '2'
catalog:
  name: Nginx Application
  version: v0.0.1
  questions:
  - variable: PUBLIC
    label: Publish Ports?
    required: true
    default: true
    type: boolean
```

`config.yml`

```yaml
name: "Nginx Application"
version: v0.0.1
```

#### Escaping Double Brackets

With the introduction to templating to Rancher, double brackets (`{{` or `}}`) will now be treated as part of a template. If you need to require these characters without having it be converted to a template, you can add `# notemplating` to the top of your compose files that contain the characters.

```yaml
# notemplating

version: '2'
services:
  web:
    image: nginx
    labels:
      key: "{{`{{ value }}`}}"
```
