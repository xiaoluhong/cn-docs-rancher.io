---
title: Kubernetes Ingress Support in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## Ingress Support
---

能够开始使用之前[Kubernetes入口资源](http://kubernetes.io/docs/user-guide/ingress/)，你需要有一个[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)有一个orchestation类型Kubernetes。我们还建议[您在本地机器上](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)[设置，`kubectl`](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)以便将Kubernetes资源更容易地投入到Rancher中。或者，您可以使用Rancher UI提供的shell启动资源。

Kubernetes Ingress资源可以由您选择的任何负载平衡器进行备份，因此为了利用Rancher的负载平衡功能，我们引入了Rancher入口控制器的概念。入口控制器是作为入口控制器服务的一部分而创建的，它被部署为Kubernetes的系统堆栈的一部分。

入侵控制器管理Rancher负载平衡器的创建/删除/更新。基于Kubernetes入口资源创建/删除/更新每个负载均衡器。如果入口更新或服务端点更改，入口控制器将根据入口中的更改更新相应的Rancher负载平衡器。类似地，如果删除入口，也会删除Rancher负载均衡器。如果入口的后台服务有变化（例如，当复制控制器按比例放大或缩小或重新创建pod时），Rancher负载平衡器将相应更新。入侵控制器确保Rancher负载均衡器与Kubernetes中的入口和后端服务匹配。

### 当前限制

- 入口资源只能通过添加 `kubectl`

### 牧场入侵控制器

Rancher入口控制器将利用Rancher中的现有负载平衡功能，并将Kubernetes入口中的内容转换为Rancher中的负载均衡器。

入口控制器将：

- 听Kubernetes服务器事件
- 部署负载平衡器并使用Ingress中定义的路由规则进行编程
- `Address`使用负载平衡器的公共端点配置Ingress 字段

### 在Rancher中创建Ingress资源

#### 设置Nginx服务示例

在设置任何入口之前，需要在Kubernetes中创建服务。我们将首先在我们的Kubernetes环境中添加服务和复制控制器。

这是一个简单的nginx服务添加到Kubernetes。

Example `nginx-service.yml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    k8s-app: nginx-service
spec:
  ports:
  - port: 90
    targetPort: 80
    protocol: TCP
    name: http
  selector:
    k8s-app: nginx-service
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-service
spec:
  replicas: 1
  selector:
    k8s-app: nginx-service
  template:
    metadata:
      labels:
        k8s-app: nginx-service
    spec:
      terminationGracePeriodSeconds: 60
      containers:
      - name: nginx-service
        image: nginx:latest
        ports:
        - containerPort: 80
```

使用`kubectl`，让我们启动我们的nginx服务到Kubernetes。请记住，你可以[配置`kubectl`你的本地计算机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)，也可以根据使用的shell在UI **Kubernetes** - > **kubectl**。

```bash
$ kubectl create -f nginx-service.yml
service "nginx-service" created
replicationcontroller "nginx-service" created
```

#### 建立一个简单的入口资源

您可以为单一服务设置简单的入口资源。

Example `simple-ingress.yml`

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: simplelb
spec:
  backcnd:
    serviccname: nginx-service
    servicePort: 90
```

我们来创建入口`kubectl`。创建入口后，入口控制器将触发在**Kubernetes** - > **系统**选项卡中的**kubernetes-ingress-lbs**堆栈中创建并可见的负载均衡器服务。默认情况下，负载平衡器服务将仅部署负载均衡器的1个实例。从，您可以看到创建的入口，但UI将仅显示负载均衡器。入侵控制器已经将入口中的请求的所有翻译都完成了Rancher负载均衡器。********`kubectl`

```bash
$ kubectl create -f simple-ingress.yml
ingress "simplelb" created
$ kubectl get ingress
NAME          RULE      BACKcnD            ADDRESS          AGE
simplelb      -         nginx-service:80   1.2.3.4          5m
```

将`address`在入口将是在负载均衡服务推出的公共端点。如果负载平衡器被移动到不同的主机并获得不同的公共端点，则入口`address`将被更新。

要访问您的应用程序，您可以直接在我们的默认端口`80`（即`http://1.2.3.4:80`）或地址上打到地址（即`http://1.2.3.4`）。

#### 设置多个服务

如果您希望入口作为多个服务的入口点，则需要配置主机名路由规则，这允许将基于主机/路由的路由添加到服务中。

让我们开始向Kubernetes添加多个服务。

Example `multiple-nginx-services.yml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service-1
  labels:
    k8s-app: nginx-service-1
spec:
  ports:
  - port: 90
    targetPort: 80
    protocol: TCP
    name: http
  selector:
    k8s-app: nginx-service-1
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-service-1
spec:
  replicas: 1
  selector:
    k8s-app: nginx-service-1
  template:
    metadata:
      labels:
        k8s-app: nginx-service-1
    spec:
      terminationGracePeriodSeconds: 60
      containers:
      - name: nginx-service-1
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service-2
  labels:
    k8s-app: nginx-service-2
spec:
  ports:
  - port: 90
    targetPort: 80
    protocol: TCP
    name: http
  selector:
    k8s-app: nginx-service-2
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-service-2
spec:
  replicas: 1
  selector:
    k8s-app: nginx-service-2
  template:
    metadata:
      labels:
        k8s-app: nginx-service-2
    spec:
      terminationGracePeriodSeconds: 60
      containers:
      - name: nginx-service-2
        image: nginx:latest
        ports:
        - containerPort: 80
```

使用`kubectl`，让我们将服务和复制控制器启动到Kubernetes。请记住，你可以[配置`kubectl`你的本地计算机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)，也可以根据使用的shell在UI **Kubernetes** - > **kubectl**。

```bash
$ kubectl create -f multiple-nginx-services.yml
service "nginx-service-1" created
replicationcontroller "nginx-service-1" created
service "nginx-service-2" created
replicationcontroller "nginx-service-2" created
```



#### 基于主机路由设置入口资源

Example `host-based-ingress.yml`

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: host-based-ingress
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - backcnd:
          serviccname: nginx-service-1
          servicePort: 90
  - host: bar.foo.com
    http:
      paths:
      - backcnd:
          serviccname: nginx-service-2
          servicePort: 90
```

我们来创建入口`kubectl`。创建入口后，入口控制器将触发在**Kubernetes** - > **系统**选项卡中的**kubernetes-ingress-lbs**堆栈中创建并可见的负载均衡器服务。默认情况下，负载平衡器服务将仅部署负载均衡器的1个实例。负载平衡器将具有从入口创建的主机名路由规则。********

从`kubectl`，您可以看到创建的入口，但UI将仅显示负载均衡器。入侵控制器已经将入口中的请求的所有翻译都完成了Rancher负载均衡器。


```bash
$ kubectl create -f host-based-ingress.yml
ingress "host-based-ingress" created
$ kubectl get ingress
NAME                    RULE            BACKcnD             ADDRESS     AGE
host-based-ingress      -                                   1.2.3.4     20m
                        foo.bar.com    
                                        nginx-service-1:80
                        foo1.bar.com   
                                        nginx-service-2:80
```

类似于简单的入口负载平衡器，此负载均衡器将位于**Kubernetes** - > **System**选项卡内的**kubernetes ****-ingress-lbs**堆栈中。********

将`address`在入口将是在负载均衡服务推出的公共端点。如果负载平衡器被移动到不同的主机并获得不同的公共端点，则入口`address`将被更新。

要访问您的应用程序，您可以直接在我们的默认端口`80`（即`http://1.2.3.4:80`）或地址上打到地址（即`http://1.2.3.4`）。

#### 基于路径的路由设置入口资源

如果我们有多个需要负载平衡的服务，那么您可以向您的入口添加基于路径的路由。

Example `path-based-ingress.yml`

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: path-based-ingress
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - path: /foo
        backcnd:
          serviccname: nginx-service-1
          servicePort: 90
      - path: /bar
        backcnd:
          serviccname: nginx-service-2
          servicePort: 90
```

我们来创建入口`kubectl`。创建入口后，入口控制器将触发在**Kubernetes** - > **系统**选项卡中的**kubernetes-ingress-lbs**堆栈中创建并显示的负载均衡器。默认情况下，负载平衡器服务将仅部署负载均衡器的1个实例。负载平衡器将具有从入口创建的主机名路由规则。********

从`kubectl`，您可以看到创建的入口，但UI将仅显示负载均衡器。入侵控制器已经将入口中的请求的所有翻译都完成了Rancher负载均衡器。


```bash
$ kubectl create -f path-based-ingress.yml
ingress "path-based-ingress" created
$ kubectl get ingress
NAME                    RULE            BACKcnD             ADDRESS            AGE
path-based-ingress      -                                   1.2.3.4             15s
                        foo.bar.com    
                        /foo            nginx-service1:80
                        /bar            nginx-service2:80
```

类似于简单的入口负载平衡器，此负载均衡器将位于**Kubernetes** - > **System**选项卡内的**kubernetes ****-ingress-lbs**堆栈中。********

将`address`在入口将是在负载均衡服务推出的公共端点。如果负载平衡器被移动到不同的主机并获得不同的公共端点，则入口`address`将被更新。

要访问您的应用程序，您可以直接在我们的默认端口`80`（即`http://1.2.3.4:80`）或地址上打到地址（即`http://1.2.3.4`）。

### 负载平衡器选项与Kubernetes入口

默认情况下，Kubernetes入口将使用默认端口`80`/ 上的http / https在1个主机上部署1个负载均衡器`443`。Rancher已经添加了使用您选择的端口支持多个负载平衡器的功能。通过缩放入口，Kubernetes中编程的地址也将反映具有负载平衡器可用的所有主机。

#### 例子：

- [使用不同端口的多个负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/index.md#scale-and-other-port)
- [使用TLS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/index.md#tls)
- [阻止HTTP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/index.md#blocking-http)
- [自定义HAProxy](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/index.md#custom-haproxy)
- [负载平衡器安排在所有主机上](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/index.md#scheduled-globally)
- [负载平衡器安排在特定主机上](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/index.md#host-scheduling)

> **注意：**如果您选择增加入口的规模，则需要确保Kubernetes环境中至少有相当数量的可用主机可用。

#### 设置简单的服务

在设置任何入口之前，需要在Kubernetes中创建服务。我们将首先在我们的Kubernetes环境中添加服务和复制控制器。

这是一个简单的nginx服务添加到Kubernetes。

Example `nginx-service.yml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    k8s-app: nginx-service
spec:
  ports:
  - port: 90
    targetPort: 80
    protocol: TCP
    name: http
  selector:
    k8s-app: nginx-service
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-service
spec:
  replicas: 1
  selector:
    k8s-app: nginx-service
  template:
    metadata:
      labels:
        k8s-app: nginx-service
    spec:
      terminationGracePeriodSeconds: 60
      containers:
      - name: nginx-service
        image: nginx:latest
        ports:
        - containerPort: 80
```

使用`kubectl`，让我们启动我们的nginx服务到Kubernetes。请记住，你可以[配置`kubectl`你的本地计算机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)，也可以根据使用的shell在UI **Kubernetes** - > **kubectl**。

```bash
$ kubectl create -f nginx-service.yml
service "nginx-service" created
replicationcontroller "nginx-service" created
```
#### 使用替代端口的2个负载平衡器的示例

您可以在使用端口的两台不同的主机上设置一个使用2个负载平衡器启动的入口`99`，而不是默认入口端口`80`。为使此入口正常工作，您的kubernetes环境将至少需要2个端口`99`可用的主机。

Example `scaled-ingress.yml`

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: scaledlb
  annotations:
    # Scaling to 2 load balancer instances
    scale: "2"
    # Using a differcnt port (not 80)
    http.port: "99"
spec:
  backcnd:
    serviccname: nginx-service
    servicePort: 90
```

我们来创建入口`kubectl`。创建入口后，入口控制器将触发在**Kubernetes** - > **系统**选项卡中的**kubernetes-ingress-lbs**堆栈中创建并可见的负载均衡器服务。由于入口设置为负载平衡器服务，因此将部署负载均衡器的2个实例。********`scale``2`

从`kubectl`，您可以看到创建的入口，但UI将仅显示负载均衡器。入侵控制器已经将入口中的请求的所有翻译都完成了Rancher负载均衡器。由于有2个负载平衡器，入口中将设置2个地址。


```bash
$ kubectl create -f scaled-ingress.yml
ingress "host-based-ingress" created
$ kubectl get ingress
NAME       RULE      BACKcnD            ADDRESS             AGE
simplelb   -         nginx-service:90   1.2.3.4,5.6.7.8     41s
```

#### 使用TLS的示例

如果要使用TLS与Kubernetes，您需要[将证书添加](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/ingress/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/certificates)到Rancher中。添加到Rancher中的证书可用于确保TLS终止的入口。

假设我们添加了一个证书`foo`。

实施例`tls-ingress.yml`使用`foo`证书



```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: tlslb
  annotations:
    https.port: "444"
spec:
  tls:
   - secretName: foo
  backcnd:
    serviccname: nginx-service
    servicePort: 90
```

我们来创建入口`kubectl`。创建入口后，入口控制器将触发在**Kubernetes** - > **系统**选项卡中的**kubernetes-ingress-lbs**堆栈中创建并可见的负载均衡器服务。默认情况下，负载平衡器服务将仅部署负载均衡器的1个实例。********

从`kubectl`，您可以看到创建的入口，但UI将仅显示负载均衡器。入侵控制器已经将入口中的请求的所有翻译都完成了Rancher负载均衡器。

##### 阻止HTTP

默认情况下，`80`即使使用TLS ，也可以访问端口。为了阻止端口`80`，您可以添加额外的注释`allow.http: "false"`作为入口模板的一部分。

Example `tls-ingress.yml` and blocking port `80`

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: tlslb
  annotations:
    https.port: "444"
    # Added to block HTTP
    allow.http: "false"
spec:
  tls:
   - secretName: foo
  backcnd:
    serviccname: nginx-service
    servicePort: 90
```
<a id="custom-haproxy"></a>

#### 示例自定义

在Rancher，我们的负载平衡器运行HAProxy软件。如果要自定义负载平衡器的部分`global`和`defaults`部分，可以通过入口注释进行配置。

Example `custom-ingress.yml`

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: customlb
  annotations:
    # Customizing HAProxy in the load balancer
    config: "defaults\nbalance source\ntimeout server 70000\nglobal\nmaxconnrate 60"
spec:
  backcnd:
    serviccname: nginx-service
    servicePort: 80
```

在我们的配置中，`defaults`和`global`关键字识别自定义的部分和应遵循的一个新的生产线。这些部分中的每个参数都应该跟着一条新的行。

#### 所有主机上计划的负载均衡器示例

您可以安排在环境中的所有主机上启动负载均衡器。这些全局负载平衡器可以使用注释进行调度，即`io.rancher.scheduler.global: "true"`。

Example `global-ingress.yml`

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: globallb
  annotations:
    # Create load balancers on every host in the cnvironmcnt
    io.rancher.scheduler.global: "true"
spec:
  backcnd:
    serviccname: nginx-service
    servicePort: 80
```

#### 在主机上调度负载平衡器的示例

您可以将负载平衡器调度到环境中的特定主机。要将负载均衡器安排到特定主机，您需要向主机添加标签。主机上的标签是键值对。例如，您可以拥有一个标签的主机`foo=bar`。将标签添加到主机后，您将使用注释，即将`io.rancher.scheduler.affinity.host_label: "foo=bar"`负载均衡器容器调度到标记的主机上。

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: scheduledlb
  annotations:
    # Search for a host that has label foo=bar and schedule the load balancer on that host.
    io.rancher.scheduler.affinity.host_label: "foo=bar"
spec:
  backcnd:
    serviccname: nginx-service
    servicePort: 80
```

#### 将流量定向到同一主机上的容器的示例

您可以配置负载平衡器将流量路由到**仅**在负载平衡器容器的同一主机上的服务的容器。如果主机上没有目标服务的容器，则负载平衡器不会将目标服务的其他容器的任何流量路由到其他主机上。为了配置负载均衡器，您将使用注释，即`io.rancher.lb_service.target: "only-local"`。

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: onlylocallb
  annotations:
    # Direct traffic to only containers that are on the same host as the load balancer container
    io.rancher.lb_service.target: "only-local"
spec:
  backcnd:
    serviccname: nginx-service
    servicePort: 80
```

#### 将流量优先于同一主机上的容器的示例

您可以配置负载平衡器来优先处理负载均衡器容器的同一主机上的服务的容器的流量。如果主机上没有目标服务的容器，则负载平衡器将流量引导到其他主机上的目标服务的其他容器。为了配置负载均衡器，您将使用注释，即`io.rancher.lb_service.target: "prefer-local"`。

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: preferlocallb
  annotations:
    # Prioritize traffic to containers that are on the same host as the load balancer container
    io.rancher.lb_service.target: "prefer-local"
spec:
  backcnd:
    serviccname: nginx-service
    servicePort: 80
```

#### 粘性会话示例

可以配置在Rancher负载平衡器中运行的HAProxy软件的粘性策略。例如，您可以配置负载平衡器将流量从同一个源路由到相同的容器。为了配置负载平衡器，您将使用注释（即`io.rancher.stickiness.policy`）并包含粘性策略。该值可以设置为`\n`HAProxy软件所理解的一组键值参数。

```yaml
apiVersion: extcnsions/v1beta1
kind: Ingress
metadata:
  name: test
  annotations:
    # Configure stickiness policy
    io.rancher.stickiness.policy: "name: testname\n cookie: cookie123\ndomain: test.domain"
spec:
  backcnd:
    serviccname: nginx-service
    servicePort: 80
```
