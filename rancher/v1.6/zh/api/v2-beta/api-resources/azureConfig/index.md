---
title: API
layout: rancher-api-v2-beta-default-v1.6
version: v1.6
lang: zh
apiVersion: v2-beta
#redirect_from:
#  - /rancher/v1.6/zh/api/v2-beta/api-resources/azureConfig/
---

## azureConfig

在Microsoft Azure中启动实例的配置。对于所有云提供商，Rancher正在呼叫docker-machine，所以任何可用的选项docker-machine都在Rancher中暴露出来。默认字段docker-machine不在Rancher API中列出，它们可以在docker-machine文档中找到。
### 资源领域

#### 可写字段

| Field            | Type          | Create   | Update | Default                                  | Notes                                    |
| ---------------- | ------------- | -------- | ------ | ---------------------------------------- | ---------------------------------------- |
| availabilitySet  | string        | Optional | -      | docker-machine                           |                                          |
| clientId         | string        | Optional | -      | -                                        |                                          |
| clientSecret     | string        | Optional | -      | -                                        |                                          |
| customData       | string        | Optional | -      | -                                        |                                          |
| dns              | string        | Optional | -      | -                                        |                                          |
| dockerPort       | string        | Optional | -      | 2376                                     |                                          |
| environment      | string        | Optional | -      | AzurePublicCloud                         |                                          |
| image            | string        | Optional | -      | canonical:UbuntuServer:16.04.0-LTS:latest |                                          |
| location         | string        | Optional | -      | westus                                   |                                          |
| noPublicIp       | boolean       | Optional | -      | -                                        |                                          |
| openPort         | array[string] | Optional | -      | -                                        |                                          |
| privateIpAddress | string        | Optional | -      | -                                        |                                          |
| resourceGroup    | string        | Optional | -      | docker-machine                           |                                          |
| size             | string        | Optional | -      | Standard_A2                              |                                          |
| sshUser          | string        | Optional | -      | docker-user                              | The ssh username to use to ssh into the host |
| staticPublicIp   | boolean       | Optional | -      | -                                        |                                          |
| storageType      | string        | Optional | -      | Standard_LRS                             |                                          |
| subnet           | string        | Optional | -      | docker-machine                           |                                          |
| subnetPrefix     | string        | Optional | -      | 192.168.0.0/16                           |                                          |
| subscriptionId   | string        | Optional | -      | -                                        |                                          |
| usePrivateIp     | boolean       | Optional | -      | -                                        |                                          |
| vnet             | string        | Optional | -      | docker-machine-vnet                      |                                          |



<br>
