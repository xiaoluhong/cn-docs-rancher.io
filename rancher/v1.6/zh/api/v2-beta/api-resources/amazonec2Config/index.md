---
title: API
layout: rancher-api-v2-beta-default-v1.6
version: v1.6
lang: zh
apiVersion: v2-beta
#redirect_from:
#  - /rancher/v1.6/zh/api/v2-beta/api-resources/amazonec2Config/
---

## amazonec2Config

使用 [驱动]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/machine). 在Amazon Web Services中启动EC2实例的配置。Rancher正在打电话docker-machine，所以任何可用于特定驱动程序的选项都会在Rancher中公开。默认字段不在Rancher API中列出，它们可以在docker-machine文档中找到。在这些领域的音符需要来自docker-machine文档

### 资源领域

#### 可写字段

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



<br>
