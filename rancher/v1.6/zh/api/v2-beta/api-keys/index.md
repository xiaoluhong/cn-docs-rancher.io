---
title: What are API Keys
layout: rancher-api-v2-beta-default-v1.6
version: v1.6
lang: zh
apiVersion: v2-beta
redirect_from:
  - /rancher/v1.6/zh/api/api-keys/
---

## 什么是API密钥

------

Rancher支持两种类型的API密钥，Account API密钥和Environment API密钥。帐户API密钥可以添加其他[帐户](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/accounts)，CRUD帐户可以访问的任何环境，并可用于使用[Rancher CLI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli)。环境API密钥与特定环境相关[联](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)，只能在环境中操作资源，并与[Rancher Compose](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose)一起使用。[访问控制](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)打开时，需要使用API密钥。

在Rancher中，通过在对象的下拉菜单中选择“ **查看API”**选项，可以**在API**中**查看**所有对象。创建环境API密钥时提供的端点URL还提供了API各个部分的所有链接。详细了解如何使用我们的[API](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/%7B%7Bpage.apiVersion%7D%7D)。

### 帐户API密钥

点击**API**查找您帐户的API端点。如果您是Rancher 的[管理员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#admin)，您将能够编辑Rancher设置并操纵所有环境，无论您是否有权访问环境。如果您是Rancher 的[用户](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#user)，您将只能操纵您有权访问的环境。

> **注意：**如果您使用的是[Rancher CLI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli)，那么您可以使用帐户API密钥来配置Rancher和CLI。

#### 添加帐户API密钥

点击**添加帐户API密钥**。提供**名称**，如果需要，**说明**。单击**创建**。牧场主将生成并显示您的帐户API密钥。API密钥是访问密钥（用户名）和秘密密钥（密码）的组合 - 执行API调用时都需要进行身份验证。复制信息后，单击**关闭**。

> **注意：**关闭窗口后，您将无法获取API密钥的密钥（密码），因此请确保将其保存在某个位置。

#### 使用帐户API密钥

当配置访问控制并且您没有登录时，如果您尝试转到API端点，则会提示您输入API密钥。用户名是访问密钥，密码是秘密密钥。

如果您正在使用`cURL`，可以通过在`-u`表单中指定参数`username:password`或通过在`.netrc`文件中添加一行来使用帐户API密钥。

#### 编辑帐户API密钥

帐户API密钥的所有选项都可以通过列出的密钥右侧的下拉菜单访问。

对于任何*活动*密钥，您可以**停用**密钥，禁止使用这些凭据。密钥将被标记*为非活动*状态。

对于任何*非活动*键，您有两个选项。您可以**激活**密钥，这将允许凭据再次访问该API。或者，您可以**删除**键，这将从Rancher中删除凭据。

您可以**编辑**任何密钥，这样可以更改帐户API密钥的名称和描述。您将无法更改实际的访问密钥或密钥。如果你想要一个新的密钥对，你需要添加一个新的密钥对。

### 环境API密钥

点击**API** - > **高级选项**，查找特定环境的API端点。无论何时创建环境API密钥，所提供的端点URL将指向您当前正在工作的特定[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)。

一旦启用了访问控制，就需要为每个[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)创建环境API密钥，以便访问特定环境的API。

> **注意：**如果您使用的是[Rancher Compose](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/rancher-compose)，那么您将需要使用环境API密钥才能直接在特定环境中启动服务。我们建议您在[Rancher CLI中](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli)使用[Rancher CLI](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cli)，因为Rancher CLI支持与启动服务相同的功能。

#### 添加环境API密钥

在添加任何环境API密钥之前，请确认您处于正确的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/api-keys/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)。单击**添加环境API密钥**。提供**名称**，如果需要，**说明**。单击**创建**。Rancher会生成并显示您的环境API密钥。API密钥是访问密钥（用户名）和秘密密钥（密码）的组合 - 执行API调用时都需要进行身份验证。复制信息后，单击**关闭**。

> **注意：**关闭窗口后，您将无法获取API密钥的密钥（密码），因此请确保将其保存在某个位置。

#### 使用环境API密钥

当配置访问控制并且您没有登录时，如果您尝试转到API端点，则会提示您输入API密钥。如果您正在尝试在特定环境中进行更改，则您的环境API密钥才会起作用。用户名是访问密钥，密码是秘密密钥。

如果您正在使用`cURL`，可以通过在`-u`表单中指定参数`username:password`或通过在`.netrc`文件中添加一行来使用环境API密钥。

#### 编辑环境API密钥

环境API密钥的所有选项都可以通过列出密钥右侧的下拉菜单访问。

对于任何*活动*密钥，您可以**停用**密钥，禁止使用这些凭据。密钥将被标记*为非活动*状态。

对于任何*非活动*键，您有两个选项。您可以**激活**密钥，这将允许凭据再次访问该API。或者，您可以**删除**该键，这将从环境中删除凭据。

您可以**编辑**任何键，这样可以更改环境API密钥的名称和描述。您将无法更改实际的访问密钥或密钥。如果你想要一个新的密钥对，你需要添加一个新的密钥对。