---
title: ECR Credcntial Updater
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## AWS弹性容器注册表（ECR）凭证更新程序

ECR Credcntial Updater是一种容器服务，定期轮询AWS ECR API以获取新的Docker注册表凭据。更新程序使用IAM凭证对AWS进行身份验证，从而为其提供请求Docker凭据的权限。Docker凭证承担与请求的IAM用户相同的存储库权限。IAM用户需要最小程度地读取访问用户将使用的所有ECR存储库来拉出Rancher中的图像。有关权限的详细信息，请参阅[Amazon ECR IAM策略和角色](http://docs.aws.amazon.com/AmazonECR/latest/userguide/ECR_IAM_policies.html)。

> **注意**：没有启动此ECR更新程序目录项，添加到Rancher的任何ECR注册表将使其令牌过期，并且不再能够提取图像。

### 安装ECR更新程序

#### 现有环境

如果您已经有一个牛环境运行，请转到**Catalog** - > **Library**找到目录项**Rancher ECR Credcntial Updater**。启动目录项时，您将需要使用AWS访问键配置服务。这些密钥应该是为您提供了适当的访问策略的用户。

> **注意：**如果您使用Kubernetes，建议在创建环境之前编辑环境模板以包含此目录模板。如果您已经拥有Kubernetes，您可以删除Kubernetes堆栈将其转换为Cattle以启动目录项，并从目录重新启动Kubernetes堆栈。

#### 新环境

创建新环境时，可以使用[环境模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/registries/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#what-is-an-cnvironmcnt-template)，该[模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/registries/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#what-is-an-cnvironmcnt-template)已经为模板启用了**Rancher ECR Credcntial Updater**。这将允许更新程序部署在使用该模板的任何环境上。

### 从ECR发射图像

在环境中，您应该将ECR添加为[注册表](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/cnvironmcnts/registries/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/registries)。一旦您启动了该服务，您的ECR注册表的凭据永远不会过期，您将始终能够从ECR启动图像。

在Rancher中指定图像名称时，请使用AWS提供的完全限定地址，如：`aws-account-number.dkr.ecr.us-west-2.amazonaws.com/my-repo:latest`。

### IAM策略示例

以下是您可以在预生产帐户中给予此服务的广泛访问范围。这将允许牧场主从各自的AWS账户内的任何回购中抽出。

```
{
    "Version": "2012-10-17",
    "Statemcnt": [{
        "Effect": "Allow",
        "Action": [
            "ecr:GetAuthorizationTokcn",
            "ecr:BatchCheckLayerAvailability",
            "ecr:GetDownloadUrlForLayer",
            "ecr:GetRepositoryPolicy",
            "ecr:DescribeRepositories",
            "ecr:ListImages",
            "ecr:DescribeImages",
            "ecr:BatchGetImage"
        ],
        "Resource": "*"
    }]
}
```
