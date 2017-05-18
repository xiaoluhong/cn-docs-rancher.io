---
title: Access Control in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
redirect_from:
  - /rancher/latest/zh/configuration/access-control/
---

## 访问控制

------

### 什么是访问控制？

访问控制是Rancher如何限制对Rancher实例具有访问权限的用户。默认情况下，未配置访问控制。这意味着任何拥有Rancher实例IP地址的人都可以使用它并访问API。您的Rancher实例向公众开放！强力建议您在启动Rancher后立即配置Access Control。启用访问控制后，您可以与您分享您的Rancher实例。在访问实例之前，将需要它们进行身份验证。只有那些具有实例的有效API密钥的API才能访问该API。

与Rancher进行认证的第一个帐户将成为该帐户的**管理员**。有关详细信息，请参阅[管理员的权限](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#admin)。

### 启用访问控制

在**管理员**选项卡中，单击**访问控制**。

验证您的Rancher实例后，Access Control将被视为启用。启用访问控制后，您将能够管理不同的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)，并与不同的人分享。

当启用访问控制时，API被锁定，并且要求作为用户进行身份验证，或者使用[API密钥](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/api-keys)进行访问。

#### 活动目录

选择**Active Directory**图标。如果要使用TLS使用Active Directory，请确保已[使用适当的证书启动Rancher服务器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#ldap)。通过单击**验证**填写部分并验证Rancher 。启用Active Directory后，您将自动以已验证的用户名身份登录。您也将作为牧场主的管理员登录。

##### 用户搜索库与组搜索库

配置Active Directory配置时，您将需要为用户输入搜索库。此搜索库允许Rancher搜索设置在您的Active Directory中的用户。如果您的用户和组位于搜索库中，则**只需**填写用户的搜索库，但如果您的组位于不同的搜索库，则可以将其他搜索库放在该`Group Search Base`字段中。此字段专用于搜索组，不是必需的。

#### Azure AD

选择**Azure AD**图标。通过点击**使用Azure**进行**身份验证，**填写部分并验证Rancher 。启用Active Directory后，您将自动以已验证的用户名身份登录。您也将作为牧场主的管理员登录。

#### GitHub上

选择**GitHub**图标，并按照UI中的说明将Rancher注册为GitHub应用程序。点击**使用GitHub进行身份验证后**，访问控制将被启用，您将自动使用GitHub登录凭据和Rancher管理员登录到Rancher。

#### 本地认证

本地身份验证允许您创建自己的一组存储在Rancher数据库中的帐户。

选择**本地**图标。通过提供**登录用户名**，**全名**和**密码**创建管理员用户。单击**启用本地验证**以**启用本地身份验证**。通过单击此按钮，管理员将被创建并保存在数据库中。您刚刚创建的管理员帐户自动登录到Rancher实例。

#### OpenLDAP的

选择**OpenLDAP**图标。如果要使用TLS使用OpenLDAP，请确保已[使用适当证书启动Rancher服务器](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#ldap)。通过单击**验证**填写部分并验证Rancher 。当启用OpenLDAP时，您将自动以已验证的用户名身份登录。您也将作为牧场主的管理员登录。

##### 用户搜索库与组搜索库

配置Active Directory配置时，您将需要为用户输入搜索库。此搜索库允许Rancher搜索设置在您的Active Directory中的用户。如果您的用户和组位于搜索库中，则**只需**填写用户的搜索库，但如果您的组位于不同的搜索库，则可以将其他搜索库放在该`Group Search Base`字段中。此字段专用于搜索组，不是必需的。

#### Shibboleth的

选择**Shibboleth**图标。填写Shibboleth帐户的配置，**保存**访问控制正在工作的信息和**测试**。

使用Shibboleth，有一些已知的问题，您应该注意，如果您正在配置来验证它。

- 没有搜索或查找支持。在用户添加时，必须输入正确的用户ID才能访问。
- 将用户添加到[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)时，不支持组ID，除非启用访问控制的管理员是该组的成员。

### 网站访问

根据您的身份验证类型，Rancher提供不同级别的站点访问。

#### Active Directory / GitHub / Shibboleth

如果您已通过AD或GitHub进行身份验证，则将有3个选项可用。

- **允许任何有效的用户** - GitHub或Active Directory中的任何用户都可以访问您的Rancher实例。这**不**推荐用于GitHub，因为它将是GitHub中的任何用户！
- **允许环境成员以及授权用户和组织** - 任何环境成员或所有者的用户也可以访问Rancher实例以及添加到*授权用户和组织*列表中的任何用户。
- **限制仅对授权用户和组织的访问权限** - 只有添加到*授权用户和组织的用户*才能访问Rancher实例。即使用户已添加到环境中，他们将无法访问，除非他们**也**加入到*授权用户和组织*部分。

任何具有Rancher实例[权限的用户](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/accounts/#users)都将获得[用户](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/accounts/#users)权限。他们将无法查看“ **管理** ”标签。您将明确需要将其帐户更改为[管理员帐户](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/accounts/#admin)。

为了使用户能够查看不同的[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)，它们需要由环境[所有者](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/#owners)添加到环境中。

#### Azure AD / OpenLDAP

对于Azure AD和OpenLDAP，作为安装程序成员的任何用户都可以访问Rancher站点。

#### 本地认证

启用本地身份验证后，管理员可以通过访问**管理员** > **帐户**选项卡来创建其他管理员/用户。单击**添加帐户**并填写您要添加的帐户的详细信息。您可以选择其帐户类型作为**管理员**或**用户**。管理员可以查看“ **管理** ”标签，而Rancher实例的用户将无法看到该选项卡。

创建帐户后，可以将admin / user添加到任何[环境中](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)。

### 帐户类型

帐户类型确定帐户是否可以访问管理员选项卡。对于Rancher中的每个环境，都有一个[会员角色](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/#membership-roles)，为特定的环境提供不同的访问级别。

#### 管理员

认证牧场主的第一个用户成为牧场主的管理员。只有管理员才有权查看**管理员**标签。

管理环境时，管理员可以查看Rancher中的所有[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/configuration/access-control/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)，即使管理员未作为环境成员添加。在管理员环境下拉菜单中，成员只会看到他们在成员资格列表中的环境。

管理员可以将其他用户添加为牧场主管理员。在用户登录Rancher后，他们可以在**Admin** > **Accounts**页面上更改用户的角色。在**管理** > **帐户**标签中，点击帐户名称旁边的**修改**，然后 将帐户类型更改为*管理员*。单击**保存**。

#### 用户

除了认证Rancher的用户，任何其他用户都将自动添加用户权限。他们将无法看到“ **管理** ”标签。

他们只能查看他们所属的环境。

### 禁用访问控制

如果您决定不再需要访问控制，请单击**禁用访问控制**按钮。这将使您的Rancher实例向公众开放，任何人都可以访问您的API。这是**不**推荐的。

### 配置会话超时

默认情况下，会话令牌在创建后16小时到期。如果您的需求太长，可以更新会话令牌的到期时间。

1. 在**管理** - > **设置** - > **高级设置下**，点击**我明白我可以通过更改高级设置来打破事情**。
2. 找到**api.auth.jwt.token.expiry**设置，然后单击编辑图标。
3. 更新超时会话值，然后单击**保存**。值以毫秒为单位。