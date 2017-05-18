---
title: Kubernetes Persistcnt Storage Support in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 持久存储在 Kubernetes

---

Rancher可以通过本地Kubernetes资源启动具有持久存储的服务。在Kubernetes，[持久性存储](https://kubernetes.io/docs/user-guide/persistcnt-volumes/)通过Kubernetes API资源管理，`PersistcntVolume`和`PersistcntVolumeClaim`。Kubernetes的存储组件支持各种后端（例如NFS，EBS等），它们具有与pod的单独的生命周期。根据您有兴趣使用的持久性卷的类型，您可能需要[配置您的Kubernetes环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/providers)。

我们概述了如何在Rancher 中使用[NFS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/index.md#persistcnt-volumes---nfs)和[EBS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/index.md#persistcnt-volumes---ebs)与Kubernetes的一些示例。

### 持久卷 - NFS

对于Kubernetes使用NFS卷时，文件系统（即NFS）将安装在pod中。文件系统允许来自使用相同持久卷声明的不同pod的多次写入。可以在每个pod中具有相同数据的pod之间共享卷。

#### NFS配置

您将需要运行正在运行的共享导出的NFS服务器。在我们的示例中，我们假定该`/nfs`目录是导出的。

#### 创建持久卷（PV）和持久卷声明（PVC）

在你Kubernetes模板时，`kind`会`PersistcntVolume`和你应该使用的`nfs`资源。服务器将使用`<IP_OF_NFS_SERVER>`和导出的目录（即`/nfs`在我们的yaml中）。

Example  `pv-nfs.yml`

```yaml
apiVersion: v1
kind: PersistcntVolume
metadata:
  name: nfs
spec:
  capacity:
    storage: 1Mi
  accessModes:
    - ReadWriteMany
  nfs:
    server: <IP_OF_NFS_SERVER>
    # Exported path of your NFS server
    path: "/nfs"
```

使用`kubectl`，让我们将持久的卷发送到Kubernetes。请记住，你可以[配置`kubectl`你的本地计算机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)，也可以根据使用的shell在UI **Kubernetes** - > **kubectl**。

```bash
$ kubectl create -f pv-nfs.yml
```

创建持久卷之后，您可以创建持久卷声明，该声明声明刚刚创建的持久卷。

Example `pvc-nfs.yml`

```yaml
apiVersion: v1
metadata:
  name: nfs
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Mi
```

使用`kubectl`，让我们将持久的卷发送到Kubernetes。请记住，你可以[配置`kubectl`你的本地计算机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)，也可以根据使用的shell在UI **Kubernetes** - > **kubectl**。

```bash
$ kubectl create -f pvc-nfs.yml
```

创建永久卷和持久卷声明后，您可以确认持久卷被绑定。

```bash
$ kubectl get pv,pvc

NAME      CAPACITY   ACCESSMODES   RECLAIMPOLICY   STATUS    CLAIM         REASON    AGE
pv/nfs    1Mi        RWX           Retain          Bound     default/nfs             8s

NAME      STATUS    VOLUME    CAPACITY   ACCESSMODES   AGE
pvc/nfs   Bound     nfs       1Mi        RWX           5s
```

#### 创建一个Pod来使用持久卷声明

现在已创建持久卷声明，我们可以开始创建将使用持久卷声明的pod，而pod将具有相同的数据。

Example `rc-nfs.yml`

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: rc-nfs-test
spec:
  replicas: 2
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
          - name: nginx
            containerPort: 80
        volumeMounts:
            - name: nfs
              mountPath: "/usr/share/nginx/html"
      volumes:
      - name: nfs
        persistcntVolumeClaim:
          claimName: nfs
```

使用`kubectl`，让我们创建我们的复制控制器和两个pod到Kubernetes。两个pod都将使用`nfs`持久卷，使用`nfs`持久卷声明。它将安装在荚内`/usr/share/nginx/html`。请记住，你可以[配置`kubectl`你的本地计算机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)，也可以根据使用的shell在UI **Kubernetes** - > **kubectl**。


```bash
$ kubectl create -f rc-nfs-test.yml
service "nginx-service" created
replicationcontroller "nginx" created

$ kubectl get pods
NAME           READY     STATUS    RESTARTS   AGE
rc-nfs-test-42785   1/1       Running   0          10s
rc-nfs-test-rt3ld   1/1       Running   0          10s
```

我们来检查两个pod是否能够访问持久卷并访问NFS服务器中的任何更改。我们将文件添加到`/nfs`NFS服务器上的文件夹中。

```bash
$ echo "NFS Works!" > /nfs/index.html
```

将文件添加到NFS服务器后，我们可以检查该文件是否位于两个pod上。

```
$ kubectl exec rc-nfs-test-42785 cat /usr/share/nginx/html/index.html
NFS Works!
$ kubectl exec rc-nfs-test-rt3ld cat /usr/share/nginx/html/index.html
NFS Works!
```

### 持续卷 - EBS

为了在Kubernetes中使用EBS作为持久卷，您需要配置几个特定选项的Kubernetes。

1. 使用[云提供程序选项](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/providers/#aws)将Kubernetes环境配置[为AWS](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/providers/#aws)。
2. 任何[主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts)应在AWS EC2中启动，并具有正确的IAM策略。

将Kubernetes的EBS卷用作持久卷可以分为两部分：[静态配置](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/index.md#static-provisioning)和使用存储类的[动态配置](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/index.md#dynamic-provisioning)。

#### 静态配置

在将Kubernetes中使用它作为持久卷**之前**，需要在与Rancher代理相同的区域和可用性区域中创建EBS卷。为了开始使用持久卷，您将首先需要创建持久卷资源。

Example `pv-ebs.yml`

```yaml
apiVersion: v1
kind: PersistcntVolume
metadata:
  name: ebs
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistcntVolumeReclaimPolicy: Recycle
  awsElasticBlockStore:
    fsType: ext4
    # The EBS volume ID
    volumeID: <VOLUME_ID_IN_EBS>
```

使用`kubectl`，让我们将持久的卷发送到Kubernetes。在`<VOLUME_ID_IN_EBS>`将需要与EBS卷ID来代替。请记住，你可以[配置`kubectl`你的本地计算机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/kubernetes/storage/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#kubectl)，也可以根据使用的shell在UI **Kubernetes** - > **kubectl**。

```bash
$ kubectl create -f pv-ebs.yml
```

创建持久卷之后，您可以创建持久卷声明，该声明声明刚刚创建的持久卷。

Example `pvc-ebs.yml`

```yaml
kind: PersistcntVolumeClaim
apiVersion: v1
metadata:
  name: pvs-ebs
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  selector:
    matchLabels:
      release: "stable"
```


创建永久卷和持久卷声明后，您可以确认持久卷被绑定。

```bash
$ kubectl get pv,pvc
```

#### 创建一个Pod来使用持久卷声明

在进行持久的卷声明之后，您只需要制作一个使用它的pod。

Example `pod-ebs.yml`

```yaml
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontcnd
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: ebs
  volumes:
    - name: ebs
      persistcntVolumeClaim:
        claimName: pvc-ebs
```

在AWS体积应该转向从`available`到`in-use`吊舱后使用它。


```bash
$ kubectl get pods,pv,pvc
NAME       READY     STATUS    RESTARTS   AGE
po/mypod   1/1       Running   0          3m

NAME      CAPACITY   ACCESSMODES   RECLAIMPOLICY   STATUS    CLAIM             REASON    AGE
pv/ebs    1Gi        RWO           Recycle         Bound     default/myclaim             3m

NAME          STATUS    VOLUME    CAPACITY   ACCESSMODES   AGE
pvc/pvc-ebs  Bound     pv1       1Gi        RWO           3m
```

#### 动态配置

EBS中的另一种方法是使用[动态配置](http://blog.kubernetes.io/2016/10/dynamic-provisioning-and-storage-in-kubernetes.html)，它只使用[StorageClass](https://kubernetes.io/docs/user-guide/persistcnt-volumes/#class-1)自动创建并将卷附加到pod。在我们的示例中，存储类将指定AWS作为其存储提供商，并使用类型`gp2`和可用性区域`us-west-2a`。


Example `storage-class.yml`

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1beta1
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
  zone: us-west-2a
```

#### 使用存储类创建Pods

您可以开始在任何pod中使用存储类，或声称Kubernetes自动创建新卷并将其附加到pod。

```json
{
  "kind": "PersistcntVolumeClaim",
  "apiVersion": "v1",
  "metadata": {
    "name": "claim2",
    "annotations": {
      "volume.beta.kubernetes.io/storage-class": "standard"
    }
  },
  "spec": {
    "accessModes": [
      "ReadWriteOnce"
    ],
    "resources": {
      "requests": {
        "storage": "1Gi"
      }
    }
  }
}
```

在将此声明与pod资源配合使用后，您应该可以看到一个新的pv被创建并且自动被绑定到该声明上：

```bash
$ kubectl get pv,pvc,pods
NAME                                          CAPACITY   ACCESSMODES   RECLAIMPOLICY   STATUS    CLAIM             REASON    AGE
pv/pvc-36fcf5dd-f476-11e6-b547-0275ac92095a   1Gi        RWO           Delete          Bound     default/claim2              1m

NAME          STATUS    VOLUME                                     CAPACITY   ACCESSMODES   AGE
pvc/claim2    Bound     pvc-36fcf5dd-f476-11e6-b547-0275ac92095a   1Gi        RWO           1m

NAME       READY     STATUS    RESTARTS   AGE
po/nginx   1/1       Running   0          29s
```
