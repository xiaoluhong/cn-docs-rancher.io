---
title: Kubernetes Addons in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## Kubernetes附件

------

Rancher自动安装Kubernetes附加组件，以帮助增强Kubernetes的体验。如果要关闭安装附加组件，则需要[配置Kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#configuring-kubernetes)以禁用自动安装加载项。

- [Helm](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/index.md#helm) - Kubernetes的软件包经理
- [仪表板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/index.md#dashboard) - Kubernetes的仪表板Web界面
- [SkyDNS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/index.md#skydns) - [Kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/index.md#skydns)的DNS服务器

### 舵

Helm是一种简化安装和管理Kubernetes应用程序的工具。它可以帮助您通过将复杂应用程序打包到Charts中来运行Kubernetes上的应用程序。图表是描述Kubernetes资源的文件的集合，可用于部署简单的pod或复杂应用程序（例如，具有所有组件的完整Web堆栈）。

Helm由两部分组成，称为Tiller的服务器和名为Helm的客户端。耕耘机由Rancher自动启动，并在**kube系统**命名空间中启动。Helm客户端安装在嵌入式`kubectl`CLI中。

#### 开始与牧师的头盔

在Rancher上安装Kubernetes时，您需要[配置您的Kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#configuring-kubernetes)，以便启用加载项才能自动安装Helm。在安装Kubernetes之后，您可以直接[通过Rancher在UI中提供的shell](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/index.md#using-helm-in-the-rancher-ui)或者[将工作站配置为使用舵来开始使用舵手](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/index.md#using-helm-on-a-workstation)。

##### 用`helm`在养牛UI

ancher将shell访问直接提供给`kubectl`可用于管理Kubernetes群集和应用程序的受管实例。要开始使用此shell，请导航到**Kubernetes** - > **CLI**。该shell自动安装Helm客户端，Helm命令可以立即使用。

[![Kubectl](https://github.com/rancher/rancher.github.io/raw/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/img/kubernetes/kubectl.png)](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/img/kubernetes/kubectl.png)

##### 使用`helm`在工作站上

在Rancher上安装Kubernetes后，您可以`kubectl`通过在**Kubernetes** - > ** CLI ** 下**生成**配置来在您的工作站上进行配置。

为了在您的工作站上使用Helm，您需要根据需要进行安装。请使用[Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md)的官方[安装文档](https://github.com/kubernetes/helm/blob/master/docs/install.md)来安装Helm。

在您的工作站上，验证您是否可以使用安装的Helm客户端与Tiller进行通信：

```
$ helm init
 $ HELM_HOME已配置为$ HOME /.helm。
警告：Tiller已经安装在群集中。（使用 - 仅限客户端来抑制此消息。）
快乐嗨！

$ helm版本
客户端：＆ version.Version {SemVer：“ v2.1.3 ”，GitCommit：“ 5cbc48fb305ca4bf68c26eb8d2a7eb363227e973 ”，GitTreeState：“ clean ” }
服务器：＆ version.Version {SemVer：“ v2.1.3 ”，GitCommit：“ 5cbc48fb305ca4bf68c26eb8d2a7eb363227e973 ”，GitTreeState：“ clean ” }
```

#### 使用头盔

与所有包裹经理一样，在使用掌舵之前，我们应该验证图表是否为最新。

```
>舵报更新
当我们抓住最新的图表存储库时， 
挂紧...
...成功从“稳定”图表存储库获取更新
更新完成。嗨快乐！一个
```

Kubernetes有自己的官方Helm图表，可以直接使用。我们将演示在Kuberenetes上安装Wordpress图表的示例。

> **注意：**随着图表在Helm中更新，我们的版本可能与最新的版本不符。您应该始终安装最新版本。

首先，我们将开始使用`helm search`查找可用的图表。

```
>舵搜索
名称版本说明
稳定/ drupal 0.3.4最通用的开源内容之一
稳定/詹金斯0.1.1 Jenkins 针对 Kubernetes的头盔图表。
stable / mariadb 0.5.2图表为 MariaDB
稳定/ MySQL的0.1.1图表为 MySQL的
稳定/ redmine 0.3.3灵活的项目管理Web应用程序。
稳定/ WordPress的0.3.1 Web发布平台的建立博客和...
```

您可以直接从Helm仓库安装图表，但是我们将获取Wordpress图表，以检查可用于部署图表的选项。

```
$ helm fetch stable / wordpress
$ tar xzvf wordpress- * .tgz
$ cd wordpress
```

您可以通过检查`values.yaml`文件来查看Wordpress图表的可用选项。此文件包括图表中使用的所有变量。通过在自己喜欢的文本编辑器中打开文件，您可以看到有多个设置。

```
image：bitnami / wordpress：4.7-r0
imagePullPolicy：IfNotPresent
wordpressUsername：user
＃ wordpressPassword：
wordpressEmail：user@example.com
wordpressFirstName：FirstName
wordpressLastName：LastName
wordpressBlogName：用户'的博客！
....
```

在Wordpress中的文件中，您将看到默认情况下已启用永久存储。默认值是使用存储类动态配置持久卷，这被称为`default`。为了开始在Kuchernetes Rancher上动态配置持久存储，请阅读关于[存储在Rancher中的存储方式](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/storage)的文档。

如果用例中不需要永久存储，那么我们可以在安装图表时禁用持久存储。

```
$ helm install --name wordpress --set mariadb.persistence.enabled = false，persistence.enabled = false stable / wordpress
NAME：wordpress
最近部署：Fri Apr 21 16:46:18 2017
NAMESPACE：默认
状态：已部署

资源：
== > v1 / Secret
名称类型数据年龄
wordpress-mariadb不透明2 2s
wordpress-wordpress不透明2 2s

== > v1 / ConfigMap
名称数据年龄
wordpress-mariadb 1 2s

== > v1 /服务
NAME CLUSTER-IP EXTERNAL-IP PORT（S）AGE
wordpress-wordpress 10.43.218.155    < pending >      80：32247 / TCP，443：31795 / TCP 2s
wordpress-mariadb 10.43.57.189    < none >     3306 / TCP 2s

== > extensions / Deployment
名称希望在当前可用的年龄
wordpress-wordpress 1 1 1 0 2s
wordpress-mariadb 1 1 1 0 2s


笔记：
获取WordPress网址：

  注意：这可能需要几分钟的负载均衡器IP可用。
        观察状态：' kubectl get svc --namespace default -w wordpress-wordpress '

  export SERVICE_IP = $（ kubectl get svc --namespace default wordpress-wordpress -o jsonpath = ' {.status.loadBalancer.ingress [0] .ip} '）
   echo http：// $ SERVICE_IP / admin

2.使用以下凭据登录以查看您的博客

  echo用户名：user
   echo密码：$（ kubectl get secret --namespace default wordpress-wordpress -o jsonpath = “ {.data.wordpress-password} ”  | base64 --decode ）
```

您将注意到`NOTES`将有助于您开始安装Wordpress图表的部分。备注包括有关如何获取WordPress URL以及如何使用默认凭据登录到博客的信息。

```
$ export SERVICE_IP = $（ kubectl get svc --namespace default wordpress-wordpress -o jsonpath = ' {.status.loadBalancer.ingress [0] .ip} '） 
$ echo http：// $ SERVICE_IP / admin
HTTP：// XXXX /管理

$ echo用户名：user
$ echo密码：$（ kubectl get secret --namespace default wordpress-wordpress -o jsonpath = “ {.data.wordpress-password} ”  | base64 --decode ）
用户名：user
密码：58wygIT06m
```

通过访问显示的URL，您将可以开始在Kubernetes上使用Wordpress并使用提供的凭据登录。

#### 使用Helm Chart使用持久存储

如果您[在Rancher](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/storage)中设置了[持久存储](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/storage)，那么您将能够在Kubernetes上创建存储类。在下一个示例中，我们将使用相同的Wordpress图表，并选择在AWS上使用永久存储。

需要配置以下先决条件：

- Kubernetes被配置为`aws`用作[云提供商](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/providers/#aws)。
- 所有[主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)已经通过正确的IAM策略添加到AWS EC2中的Rancher中。
- 命名的[存储类](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/storage/#dynamic_provisioning)`default`已安装并配置为使用AWS卷。

在Kubernetes正确启动并创建存储类之后，您可以部署Wordpress图表以使用永久存储。

```
$ helm install --name wordpress stable / wordpress
NAME：wordpress
最近部署：Fri Apr 21 17:12:35 2017
NAMESPACE：默认
状态：已部署

资源：
== > v1 / Secret
名称类型数据年龄
wordpress-mariadb不透明2 2s
wordpress-wordpress不透明2 2s

== > v1 / ConfigMap
名称数据年龄
wordpress-mariadb 1 2s

== > v1 /服务
NAME CLUSTER-IP EXTERNAL-IP PORT（S）AGE
wordpress-mariadb 10.43.101.232    < none >         3306 / TCP 2s
wordpress-wordpress 10.43.250.75    < pending >    80：30296 / TCP，443：30094 / TCP 2s

== > extensions / Deployment
名称希望在当前可用的年龄
wordpress-wordpress 1 1 1 0 2s
wordpress-mariadb 1 1 1 0 2s

== > v1 / PersistentVolumeClaim
名称状态容量访问年龄
wordpress-wordpress-wordpress绑定pvc-f396de3d-26a4-11e7-9213-02ee7a4cff8e 8Gi RWO 2s
wordpress-wordpress-apache绑定pvc-f3986989-26a4-11e7-9213-02ee7a4cff8e 1Gi RWO 2s
wordpress-mariadb绑定pvc-f399feb7-26a4-11e7-9213-02ee7a4cff8e 8Gi RWO 2s


笔记：
获取WordPress网址：

  注意：这可能需要几分钟的负载均衡器IP可用。
        观察状态：' kubectl get svc --namespace default -w wordpress-wordpress '

  export SERVICE_IP = $（ kubectl get svc --namespace default wordpress-wordpress -o jsonpath = ' {.status.loadBalancer.ingress [0] .ip} '）
   echo http：// $ SERVICE_IP / admin

2.使用以下凭据登录以查看您的博客

  echo用户名：user
   echo密码：$（ kubectl get secret --namespace default wordpress-wordpress -o jsonpath = “ {.data.wordpress-password} ”  | base64 --decode ）
```

您可以看到在图表中使用永久存储的新资源已创建。

```
== > v1 / PersistentVolumeClaim
名称状态容量访问年龄
wordpress-wordpress-wordpress绑定pvc-f396de3d-26a4-11e7-9213-02ee7a4cff8e 8Gi RWO 2s
wordpress-wordpress-apache绑定pvc-f3986989-26a4-11e7-9213-02ee7a4cff8e 1Gi RWO 2s
wordpress-mariadb绑定pvc-f399feb7-26a4-11e7-9213-02ee7a4cff8e 8Gi RWO 2s
```

您可以验证是否已创建持久卷。

```
$ kubectl get pv
名称能力访问恢复状态索赔原因年龄
pvc-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 8Gi RWO删除绑定默认/ wordpress-wordpress-wordpress 4m
pvc-yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy 1Gi RWO删除绑定默认/ wordpress-wordpress-apache 4m
pvc-zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz 8Gi RWO删除绑定默认/ wordpress-mariadb 4m
```

在这个例子中，我们选择了AWS作为[云提供商](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/addons/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/providers/#aws)，它将在AWS上创建一个ELB作为Wordpress图表的负载均衡器。

```
$ export SERVICE_IP = $（ kubectl get svc --namespace default wordpress-wordpress -o jsonpath = ' {.status.loadBalancer.ingress [0] .hostname} '） 
$ echo http：// $ SERVICE_IP / admin                                                                                         
http://xxxxxxxxxxxxxx.us-west-2.elb.amazonaws.com/admin
```

### SkyDNS

在牧场上，每个服务都有一个名字。其他服务可以使用DNS服务名称与服务进行通信。DNS服务名称是`<service_name>.<namespace_name>.svc.cluster.local`。

使用在helm示例中启动的wordpress应用程序，您可以获取Wordpress端点服务的名称和命名空间。

```
> kubectl get services

NAME                  CLUSTER-IP      EXTERNAL-IP        PORT(S)                      AGE
kubernetes            10.43.0.1       <none>             443/TCP                      22d
wordpress-mariadb     10.43.101.232   <none>             3306/TCP                     1h
wordpress-wordpress   10.43.250.75    xxxxxxxx...   80:30296/TCP,443:30094/TCP   1h

> kubectl describe services/wordpress-wordpress
Name:			wordpress-wordpress
Namespace:		default
Labels:			app=wordpress-wordpress
			      chart=wordpress-0.4.2
			      heritage=Tiller
			      release=wordpress
Selector:		app=wordpress-wordpress
Type:			LoadBalancer
IP:			10.43.250.75
LoadBalancer Ingress:	xxxxxxxxxxxxxxxxxxxx.elb.amazonaws.com
Port:			http	80/TCP
NodePort:		http	30296/TCP
Endpoints:		10.42.122.207:80
Port:			https	443/TCP
NodePort:		https	30094/TCP
Endpoints:		10.42.122.207:443
Session Affinity:	None
No events.

```

wordpress应用程序命名`wordpress`。对于这个实例，DNS服务名称是`wordpress-wordpress.default.svc.cluster.local`。