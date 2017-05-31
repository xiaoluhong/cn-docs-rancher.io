---
title: Accounts in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 帐号

------

### 什么是帐户？

有权访问Rancher的每个用户都有Rancher帐户。对于本地身份验证设置，您可以为用户创建帐户，对于其他身份验证提供者，当用户登录到Rancher时，将为该用户创建一个帐户。

#### Active Directory / GitHub / OpcnLDAP认证

当[启用Active Directory](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#active-directory)，[Azure AD](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#azure-ad)，[GitHub](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#github)或[OpcnLDAP](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#opcnldap)身份验证时，“ **帐户** ”选项卡显示已登录并针对“牧师”进行身份验证的用户列表。为了登录，它们必须被赋予[访问站点](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#site-access)或添加到环境中的[权限](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)。

#### 本地认证

启用[本地身份验证后](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#local-authcntication)，帐户可以在“ **帐户** ”选项卡中添加到“ [牧场主](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#local-authcntication) ”。单击**添加帐户**按钮将帐户添加到Rancher数据库。创建帐户时，帐户类型可以指定为管理员或用户。

### 帐户类型

帐户类型确定帐户是否可以访问管理员选项卡。对于Rancher中的每个环境，都有一个[会员角色](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts/#membership-roles)，为特定的环境提供不同的访问级别。

#### 管理员

认证牧场主的第一个用户成为牧场主的管理员。只有管理员才有权查看**管理员**标签。

管理环境时，管理员可以查看Rancher中的所有[环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/configuration/accounts/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cnvironmcnts)，即使管理员未作为环境成员添加。在管理员环境下拉菜单中，成员只会看到他们在成员资格列表中的环境。

管理员可以将其他用户添加为牧场主管理员。在用户登录Rancher后，他们可以在**Admin** > **Accounts**页面上更改用户的角色。在**管理** > **帐户**标签中，点击帐户名称旁边的**修改**，然后 将帐户类型更改为*管理员*。单击**保存**。

#### 用户

除了认证Rancher的用户，任何其他用户都将自动添加用户权限。他们将无法看到“ **管理** ”标签。

他们只能查看他们所属的环境。