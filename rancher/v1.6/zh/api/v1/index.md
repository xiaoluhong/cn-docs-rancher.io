---
title: How to use the Rancher API
layout: rancher-api-v1-default-v1.6
version: v1.6
lang: en
apiVersion: v1
---

## 如何使用API

------

API可以从Web浏览器访问自己的用户界面。这是一种查看资源，执行操作并查看等效的cURL或HTTP请求和响应的简单方法。要访问它，请单击**API**以查找URL端点。

### 术语

API中使用的一些资源类型名称与UI中使用的当前术语不匹配。尤其是：

| UI                                       | API                                      | 描述                                       |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| [环境]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/environments) | [项目](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/%7B%7Bpage.apiVersion%7D%7D/api-resources/project) | 一组物理资源，如[主机]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api-resources/host) |
| [堆栈]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/cattle/stacks) | [环境]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//api-resources/environment) | （API）环境是一组服务和Rancher Compose运行的级别。       |

在文档中，我们在描述中使用UI术语，并提供了有关差异的其他免责声明。这个混乱将`/v2`在API 的将来被清除。

### 认证

启用[访问控制](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)时，API请求必须包含身份验证[信息](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control)。使用[API密钥]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//api/api-resources/apikey)进行HTTP基本身份验证。API密钥可以属于单一（UI）环境/（API）[项目](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/api-resources/project)，可以访问该环境，或者可以访问[帐户](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/api-resources/account)所有环境的所有环境，以及创建新环境的能力。还有一个单独的JSON Web令牌接口，主要用于UI。

> **注意：**在我们的示例中，我们假设`${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}`是Account API密钥，但是如果要使用Environment API Key，则可以`projects/${PROJECT_ID}`从URL中省略。

#### 环境的API密钥

可以在UI中创建环境API密钥，请参阅[API＆Keys](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/v1/api-keys)。关键是环境所有，并且可以全面访问环境，但无法访问任何其他环境。 [会员角色](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/#membership-roles)不适用于这些密钥。

#### 帐户的API密钥

帐户API密钥当前不会在UI中公开。您可以通过在浏览器中点击API来创建一个：

- 点击UI中的Endpoint链接
- 导航 `/v1/apikeys`
- 点击 `Create`
- 选择`accountId`您要为其创建的密码
- 可选择设置`name`和`description`
- 点击`Show Request`，然后`Send Request`
- 保存`publicValue`并`secretValue`回复

帐号键可以创建新的环境，并且可以通过使用来访问多个环境`/v1/projects/`。 [会员角色](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/#membership-roles)适用于这些密钥，并限制帐户可以看到的环境以及可以采取的行动。

### 作用域

- 大多数资源都是由一个环境所拥有的 `/v1/projects/<project_id>/<resource>`
- 由于环境凭据只能访问一个环境（项目），因此您可以选择跳过该`/project/<project_id>`部分。
- 例如，如果您正在使用项目工作`1a5`使用环境API密钥，`/v1/projects/1a5`是一样的`/v1`，并`/v1/projects/1a5/hosts`是一样的`/v1/hosts`。
- 文档一般参考较短的`/v1/<type>`版本。如果使用“帐号”键，则在相应环境的路径中添加。

### 发出请求

API通常是RESTful的，但是具有几个功能，可以使客户端可以发现的一切的定义，以便可以编写通用客户端，而不必为每种类型的资源编写特定的代码。有关通用API规范的详细信息，[请参阅此处](https://github.com/rancher/api-spec/blob/master/specification.md)。

- 每个类型都有一个Schema描述：
  - 获取此类资源的URL
  - 资源可以拥有的每个字段，以及它们的类型，基本的验证规则，无论是必需的还是可选的。
  - 对这种类型的资源，其输入和输出（也作为模式）可能采取的每一个行动。
  - 允许进行过滤的每个字段
  - 什么HTTP动词方法可用于集合本身或集合中的各个资源。
- 所以理论是你可以加载模式列表，并了解有关API的一切。实际上，API的UI是如何工作的，它不包含Rancher本身特有的代码。获取模式的URL是作为`X-Api-Schemas`标题在每个HTTP响应中发送的。从那里你可以按照`collection`每个模式的链接来知道在哪里列出资源，以及`links`返回资源的其他内容来获取任何其他信息。
- 实际上，您可能只想构造URL字符串。我们强烈建议将此限制在顶层以列出collection（`/v1/<type>`）或获取特定资源（`/v1/<type>/<id>`）。任何比这更深刻的事情在将来的版本中都会有变化。
- 资源之间的关系称为链接。每个资源都包括一个`links`带有链接名称和检索该信息的URL的地图。再次应该`GET`是资源，然后按照`links`地图中的URL ，不要自己构建这些字符串。
- 大多数资源都有一些动作，它们会做某些事情或改变资源的状态。要使用这些，请向地图中的`POST`URL 发送HTTP 以`actions`获取所需的操作。一些操作需要输入或产生输出，有关特定信息，请参阅每种类型的单独文档或模式。
- 要编辑资源，发送一个HTTP `PUT`到`links.self`链接上，你要更改的领域的资源。未知字段和不可编辑的字段将被忽略。
- 要删除资源，请向资源`DELETE`上的`links.self`链接发送HTTP 。请注意，一些资源可能需要在被删除之前被停用，如果您通过ID专门询问删除资源，仍然可以从API检索已删除的资源。
- 要创建新的资源，HTTP `POST`到模式中的集合URL（即是`/v1/<type>`）。

### 过滤

大多数集合可以通过使用HTTP查询参数的通用字段在服务器端进行过滤。该`filters`地图显示您可以筛选哪些字段以及您所做的请求的过滤值。API UI具有设置过滤功能的控件，并向您显示适当的请求。对于简单的“等于”匹配它只是`field=value`。修饰符可以添加到字段名称中，例如`field_gt=42`“field is greater than 42”。有关详细信息，请参阅[API规范](https://github.com/rancher/api-spec/blob/master/specification.md#filtering)。

### 排序

大多数集合可以通过使用HTTP查询参数的公共字段在服务器端进行排序。该`sortLinks`地图显示您可以使用哪些类别，以及通过该URL排序的URL。它还包括有关当前响应按照指定排序的信息。

### 分页

默认情况下，API响应的分页限制为每页100个资源。`limit=1000`查询参数可以增加到最多1000个。`pagination`收集回复中的地图会告诉您是否拥有完整的结果集，如果您没有，则可以链接到下一页。

### 的WebSockets

几个Rancher功能（如容器日志，shell访问和统计信息）使用WebSockets来传输信息。要从API使用这些：

- 按照相应的链接或执行相应的操作
- 响应将包括一个URL（以`ws://`或从`wss://`）开始，一个长`token`字符串。
- 打开一个WebSocket客户端，指向返回的URL。
- 令牌由Rancher服务器签名，并允许容器开启的主机授权请求，因此必须作为HTTP头发送到服务器`Authorization: Bearer <token_string>`。
- 如果您通过网络浏览器发出请求，则无法发送任意的HTTP标头，因此您可以选择将其添加为`token=<token_string>`URL 中的查询参数。