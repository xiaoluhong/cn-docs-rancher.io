---
title: API
layout: rancher-api-v1-default-v1.6
version: v1.6
lang: zh
apiVersion: v1
#redirect_from:
#  - /rancher/v1.6/zh/api/v1/api-resources/amazonec2Config/
---

## 亚马逊ec2配置

该配置使用亚马逊网络服务推出的EC2实例[的机器]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//api-resources/machine)。牧场主呼吁`docker-machine`，所以在任何可用的选项`docker-machine`为特定的驱动程序在牧场主暴露。从默认域`docker-machine`养牛API中没有列出，它们可以在中找到`docker-machine`文件。在这些领域的音符**需要**来自`docker-machine`文档。

### Resource Fields

#### Writeable Fields

| Field                   | Type          | Create   | Update | Default          | Notes                                    |
| ----------------------- | ------------- | -------- | ------ | ---------------- | ---------------------------------------- |
| accessKey               | string        | Optional | -      | -                |                                          |
| ami                     | string        | Optional | -      | -                |                                          |
| blockDurationMinutes    | string        | Optional | -      | 0                |                                          |
| deviceName              | string        | Optional | -      | /dev/sda1        |                                          |
| endpoint                | string        | Optional | -      | -                |                                          |
| iamInstanceProfile      | string        | Optional | -      | -                |                                          |
| insecureTransport       | boolean       | Optional | -      | -                |                                          |
| instanceType            | string        | Optional | -      | t2.micro         |                                          |
| keypairName             | string        | Optional | -      | -                |                                          |
| monitoring              | boolean       | Optional | -      | -                |                                          |
| openPort                | array[string] | Optional | -      | -                |                                          |
| privateAddressOnly      | boolean       | Optional | -      | -                |                                          |
| region                  | string        | Optional | -      | us-east-1        | The region to use when launching the host |
| requestSpotInstance     | boolean       | Optional | -      | -                |                                          |
| retries                 | string        | Optional | -      | 5                |                                          |
| rootSize                | string        | Optional | -      | 16               |                                          |
| secretKey               | string        | Optional | -      | -                |                                          |
| securityGroup           | array[string] | Optional | -      | [docker-machine] |                                          |
| sessionToken            | string        | Optional | -      | -                |                                          |
| spotPrice               | string        | Optional | -      | 0.50             |                                          |
| sshKeypath              | string        | Optional | -      | -                |                                          |
| sshUser                 | string        | Optional | -      | ubuntu           | The ssh username to use to ssh into the host |
| subnetId                | string        | Optional | -      | -                |                                          |
| tags                    | string        | Optional | -      | -                |                                          |
| useEbsOptimizedInstance | boolean       | Optional | -      | -                |                                          |
| usePrivateAddress       | boolean       | Optional | -      | -                |                                          |
| userdata                | string        | Optional | -      | -                |                                          |
| volumeType              | string        | Optional | -      | gp2              |                                          |
| vpcId                   | string        | Optional | -      | -                |                                          |
| zone                    | string        | Optional | -      | a                |                                          |


