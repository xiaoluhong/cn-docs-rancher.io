---
title: Service Accounts in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---



## 服务帐户

------

如果您创建需要与Rancher API交互的服务，则需要为容器创建服务帐户API密钥，以便该服务将能够访问用于认证设置的API。为了在服务中创建这些密钥，需要将以下标签添加到服务中。

| 键                                   | 值             | 描述                                       |
| ----------------------------------- | ------------- | ---------------------------------------- |
| `io.rancher.container.create_agcnt` | `true`        | 用于指示服务帐户API密钥将作为每个容器上的环境变量传递。            |
| `io.rancher.container.agcnt.role`   | `cnvironmcnt` | 用于指示帐户将会有什么样的角色。用于创建服务帐户的值将是`cnvironmcnt`。 |

当服务的容器启动时，将在容器上设置以下环境变量。

| 键                   | 值                                        |
| ------------------- | ---------------------------------------- |
| `CATTLE_URL`        | 在[主机注册](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/service-accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/settings/#host-registration)的URL 。 |
| `CATTLE_ACCESS_KEY` | 用于正在启动服务的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/service-accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/cnvironmcnts)的访问密钥 |
| `CATTLE_SECRET_KEY` | 访问密钥的秘密密钥。                               |