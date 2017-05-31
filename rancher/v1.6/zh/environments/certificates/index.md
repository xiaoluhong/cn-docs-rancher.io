---
title: Certificates in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 证书

------

### 添加证书

为了向您的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/certificates/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)添加证书，请转到基础**结构** - > **证书**页面。该页面将列出添加到您的Rancher环境中的所有证书。要添加新证书，请单击**添加证书**。

1. 提供一个**名称**，如果需要，**证书**的说明。
2. 提供**私钥**证书。您可以单击**从文件读取从**计算机导入或将私钥粘贴到文本框中。
3. 提供**证书**。您可以单击**从文件读取从**计算机导入或将证书粘贴到文本框中。
4. （可选）如果您有其他链接证书，您还可以使用*从文件读取* *将其添加到计算机中导入，或将证书粘贴到文本框中。

### 使用证书

在环境中加载的证书可用于[SSL终止负载平衡器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/certificates/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-load-balancers/#ssl-termination)或[Kubernetes入口的TLS终止](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/certificates/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/kubernetes/ingress/#example-using-tls)。