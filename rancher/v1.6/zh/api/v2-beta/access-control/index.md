---
title: Setting up Access Control using the API
layout: rancher-api-v2-beta-default-v1.6
version: v1.6
lang: en
apiVersion: v2-beta
---

## 使用API设置访问控制

------

Rancher在API中有两种认证方式。我们的原始认证方法具有自己的顶级API类型，用于每个身份验证提供程序的特定配置。
还有一个[身份验证服务作为单独的微服务运行，该服务](https://github.com/rancher/rancher-auth-service/wiki)具有可用于许多身份验证提供者的一般配置。

### 认证提供商使用顶级API

- 活动目录： `/v2-beta/ldapconfig`
- Azure AD :( `/v2-beta/azureadconfig`这是一个web服务可用的Azure，与实际的Active Directory无关）
- OpenLDAP的： `/v2-beta/openldapconfig`
- Local Rancher DB： `/v2-beta/localauthconfig`

### 验证提供商使用验证服务

- 公共与企业Github： `/v1-auth/config`
- Shibboleth（SAML）： `v1-auth/config`

### 访问模式

身份验证提供商可能拥有许多用户（即全局，用于公共GitHub），因此可能希望将访问权限限制为其包含的有效用户的子集。在`accessMode`和`allowedIdentities`参数中的每个驱动器进行控制。

#### 提供商+访问模式可用性：

| 提供商         | 配置URL                      | 配置架构            | 无限制      | 受限       | 需要       |
| ----------- | -------------------------- | --------------- | -------- | -------- | -------- |
| 活动目录        | `/v2-beta/ldapconfig`      | ldapconfig      | 1.0      | 1.1 DEV5 | 1.1 DEV5 |
| Azure AD    | `/v2-beta/azureadconfig`   | azureadconfig   | 1.1 DEV5 | 未来       | 未来       |
| OpenLDAP的   | `/v2-beta/openldapconfig`  | openldapconfig  | 1.0      | 未来       | 未来       |
| GitHub的*    | `/v1-auth/config`          | 配置              | 1.0      | 1.0      | 1.1 DEV5 |
| 当地牧场主       | `/v2-beta/localauthconfig` | localauthconfig | 1.0      | N / A ** | N / A ** |
| Shibboleth的 | `/v1-auth/config`          | 配置              | 1.2      | 1.2      | 1.2      |

*：GitHub提供商支持公共GitHub和私有GitHub Enterprise安装。**：用户在Rancher本地认证中直接定义和管理，因此不需要限制/必需。

#### 访问模式：

- ```
  unrestricted
  ```

  - 验证提供商中的任何有效用户都可以登录。

- ```
  restricted
  ```

  - 具体的一组用户/组`allowedIdentities`可以登录。
  - 另外，作为环境成员添加的任何人（在UI `project`中，在API中）都可以登录。

- ```
  required
  ```

  - 严格的具体一组用户/组`allowedIdentities`可以登录。
  - 环境成员也必须在`allowedIdentities`列表中登录。

### 启用访问控制

Rancher UI执行一个三步骤的过程，以安全地启用访问控制。如果您正在自动化，并确保配置正确，您可以跳过前两个请求并直接进入[启用](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/access-control/index.md#enabling)。

#### 配置所需的提供商

为所需的提供者生成完整的配置对象`enabled: false`。

根据您的提供商，您可以将其作为`POST /v2-beta/<desired provider config>`或的主体提交`POST /v1-auth/config`。

#### 测试访问令牌的生成

`POST /v2-beta/token` `{code: "<code string for provider>"}`

有关详细信息，请参阅[下面的生成授权令牌](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/api/v2-beta/access-control/index.md#generating-an-auth-token)。如果令牌生成失败，则配置有问题，如果`enabled`设置为可能，则可能已被锁定`true`。

#### 启用

将配置对象重新提交给`POST /v2-beta/<desired provider config>`或`POST /v1-auth/config`，此时与`enabled:true`。

##### 生成授权令牌

`POST /v2-beta/token {code: "<code string for provider>"}`

对于GitHub，代码字符串是从GitHub Oauth重定向发回的值。对于其他提供程序，字符串之间用冒号分隔用户凭证（ie `<username>:<password>`）。

如果认证成功，将返回16小时的令牌，并且配置正常。这可以作为`Authorization: Bearer <token>`头发送以验证未来的请求。

### 查找身份

可以使用`/v2-beta/identities`端点查找用户，组和/或组织。对于大多数提供商，`GET /v2-beta/identities`将返回经过身份验证的用户所属的组或组织。可以搜索其他名称`GET /v2-beta/identities?name=<NAME>`。

### 管理允许的身份

身份最低限度由一个`externalIdType`，它标识用户/组/组织是什么提供商和资源类型，`externalId`哪个是该资源的特定标识符。应该查找身份（如上所述），而不是手动生成。例如，在GitHub `externalId`中，用户/组织的user_id（通常不是众所周知的）。

在受支持的提供程序配置中，有一个`allowedIdentities`数组，其中包含“受限制”和“必需”的允许用户/组/组织列表`accessMode`。要更新列表，请`POST /v2-beta/<configured provider config>`使用新列表。配置的秘密部分（例如服务帐户密码）可以保留为空，以保持其当前值。

### 获取当前启用的访问控制提供程序

`GET /v2-beta/token`没有发送授权信息将返回配置的提供程序以及使用它所需的公共信息。例如，Github a `redirectUrl`包含协议和Enterprise `hostname`，如果配置，将返回。

### 禁用访问控制

`POST /v2-beta/<enabled provider config>`或`POST /v1-auth/config`与`enabled: false`

### 更改访问控制提供程序

一般我们不推荐这个。一旦访问控制被禁用，它可以为不同的提供者重新启用。当切换提供商时，Rancher无法知道谁将什么帐户与什么帐户相关联，所以`externalId`/ `externalIdType`不会在现有帐户上进行修改。现有帐户和环境将无法再访问。这可以通过遍历每个帐户（即`/v2-beta/accounts?kind=admin`和`/v2-beta/accounts?kind=user`）和编辑这些字段在数据库或API中手动更正。