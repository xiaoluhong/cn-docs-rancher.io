---
title: Private Registry with Kubernetes in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## Private Registry with Kubernetes in Rancher
---

如果您正在运行[没有互联网访问的](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/private-registry/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/no-internet-access) Rancher，或者Rancher无法访问DockerHub（即`docker.io`）和Google Container Registry（ie `gcr.io`），则将不会安装pod下面的容器映像和Kubernetes附件。您需要[配置Kubernetes](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/kubernetes/private-registry/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/#configuring-kubernetes)来指定一个私有仓库，以便能够安装Kubernetes附加组件和pod下面的容器映像。

#### Private Registry Requirements

Rancher expects the private registry to mirror DockerHub (i.e `docker.io`) and Google Container Registry (i.e. `gcr.io`).

##### Pod Infra Container Image

When [configuring Kubernetes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/#configuring-kubernetes), Rancher uses an image whose network/ipc namespaces in each pod will use.

```yml
# Located as a default value in the k8s templates
Image: gcr.io/google_containers/pause-amd64:3.0
```

##### Kubernetes Add-Ons

The `namespace/name:tag` is expected to be consistent the images in the [Rancher add-on templates](https://github.com/rancher/kubernetes-package/tree/master/addon-templates).

The images below are the list of add-ons that are currently supported, you will need to [check the Github repo for exact version](https://github.com/rancher/kubernetes-package/tree/master/addon-templates) and copy the exact version for each image.

##### Images for Helm

```yml
# Located in the helm/tiller-deploy.yaml
image: <$PRIVATE_REGISTRY>/kubernetes-helm/tiller:<VERSION>
```

##### Images for Dashboard

```yml
# Located in the dashboard/dashboard-controller.yaml
image: <$PRIVATE_REGISTRY>/google_containers/kubernetes-dashboard-amd64:<VERSION>
```

##### Images for Heapster

```yml
# Located in the heapster/heapster-controller.yaml
image: <$PRIVATE_REGISTRY>/google_containers/heapster:<VERSION>

# Located in the heapster/influx-grafana-controller.yaml
image: <$PRIVATE_REGISTRY>/kubernetes/heapster_influxdb:<VERSION>
image: <$PRIVATE_REGISTRY>/google_containers/heapster_grafana:<VERSION>
```
