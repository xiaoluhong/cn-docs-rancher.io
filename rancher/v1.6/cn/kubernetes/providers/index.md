---
title: Kubernetes Cloud Providers
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## Kubernetes - Cloud Providers
---

在Kubernetes中，有一个[云提供商](https://kubernetes.io/docs/getting-started-guides/scratch/#cloud-provider)的概念，它是一个模块，用于管理负载平衡器，节点（即主机）和网络路由。

目前，Rancher在[配置Kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#configuring-kubernetes)时支持两个云[提供商](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#configuring-kubernetes)。您可以选择要使用的云提供商。

### Rancher

  * **节点：**支持可在Rancher中添加的任何[主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)。
* **负载平衡器：**启动使用HAproxy和`rancher/lb-service-haproxy`映像的Rancher负载均衡器作为负载平衡器服务。默认情况下，负载平衡器将轮询流量到pod。

默认情况下，Kubernetes的编排设置为`rancher`。

### AWS

  * **节点：**仅支持添加为[自定义主机的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom) AWS [主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)。
* **负载平衡器：**将AWS弹性负载平衡器（ELB）作为负载平衡器服务启动。您仍然可以使用[入口](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/ingress)创建Rancher负载平衡器。
* **持久性卷**：能够使用AWS弹性块存储（EBS）用于[持久卷](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/storage)。

#### 添加主机

在Kubernetes配置为使用`aws`云提供程序运行后，添加到环境中的任何主机都将需要是AWS EC2实例，并至少具有以下IAM策略：

```json
{
  "Effect": "Allow",
  "Action": "ec2:Describe*",
  "Resource": "*"
}
```

为了使用弹性负载平衡器（ELB）和EBS与Kubernetes，主机将需要具有适当访问的IAM角色。

##### Example Policy for IAM Role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "ec2:AttachVolume",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "ec2:DetachVolume",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": ["ec2:*"],
      "Resource": ["*"]
    },
    {
      "Effect": "Allow",
      "Action": ["elasticloadbalancing:*"],
      "Resource": ["*"]
    }
  ]
}
```

#### 弹性负载平衡器（ELB）作为Kubernetes服务

将[Kubernetes配置](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#configuring-kubernetes)为`aws`用作云提供商并确保主机具有适用于ELB的IAM策略后，可以开始创建负载平衡器。

##### Example `lb.yml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-lb
  labels:
    app: nginx
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - name: http
    port: 80
    protocol: TCP
```

使用`kubectl`，让我们将负载平衡器服务启动到Kubernetes。请记住，你可以[配置`kubectl`你的本地计算机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)，也可以根据使用的shell在UI **Kubernetes** - > **kubectl**。

```bash
$ kubectl create -f lb.yml
service "nginx-lb" created
$ kubectl describe services nginx-lb
Name:			nginx-lb
Namespace:		default
Labels:			app=nginx
Selector:		name=nginx
Type:			LoadBalancer
IP:			10.43.137.5
LoadBalancer Ingress:	a4c7d4290f48011e690470275ac52fef-1158549671.us-west-2.elb.amazonaws.com
Port:			http	80/TCP
NodePort:		http	32166/TCP
Endpoints:		<none>
Session Affinity:	None
Events:
  FirstSeen	LastSeen	Count	From			SubObjectPath	Type		Reason			Message
  ---------	--------	-----	----			-------------	--------	------			-------
  17s		17s		1	{service-controller }			Normal		CreatingLoadBalancer	Creating load balancer
  14s		14s		1	{service-controller }			Normal		CreatedLoadBalancer	Created load balancer
```

#### 使用EBS卷

将Kubernetes配置为`aws`用作云提供商并确保主机具有适用于EBS的IAM策略后，可以开始[使用EBS卷](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/providers/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/storage/#persistent-volumes---ebs)。