---
title: Secrets in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 秘密 - 实验

------

Rancher已经引入了在容器中使用命名秘密的能力。Rancher通过使用本地AES（高级加密标准）密钥或[Vault Transit](https://www.vaultproject.io/docs/secrets/transit/)与加密后端进行接口，以安全地存储Rancher中的值。

### 加密后端配置

默认情况下，Rancher服务器配置为使用本地存储的AES256加密密钥来执行密码加密。这些加密值存储在Rancher服务器使用的MySQL数据库中。

#### 使用Vault Transit

而不是使用本地存储的密钥，Rancher可以配置为使用[Vault Transit](https://www.vaultproject.io/docs/secrets/transit/)执行加密。

##### 使用Vault Transit安装Rancher服务器

在安装Rancher Server之前，需要使用Vault Transit完成几个先决条件。

1. 将Vault运行后端挂载到将运行Rancher服务器的主机上

2. 使用Vault CLI或API，创建一个名为的新加密密钥 `rancher`

3. 使用Vault CLI或API，创建可以使用`rancher`密钥加密/解密的保管库访问令牌

   - 此令牌必须使用Rancher服务器的策略来使用以下Vault Transit端点。在`<KEY>`此列表中是`rancher`已创建的关键。

     ```
     path "transit/random/*" {
       capabilities = ["create", "update"]
     }

     path "transit/hmac/*" {
       capabilities = ["create", "update"]
     }

     path "transit/encrypt/rancher" {
       capabilities = ["create", "update"]
     }

     path "transit/decrypt/rancher" {
       capabilities = ["create", "update"]
     }

     path "transit/verify/rancher/*" {
       capabilities = ["create", "update", "read"]
     }

     path "transit/keys/*" {
       capabilities = ["deny"]
     }

     path "sys/*" {
       capabilities = ["deny"]
     }

     ```

4. 启动Rancher服务器并将环境变量添加到命令以连接到Vault。

   ```
   $ docker run -d --restart = unless-stopped -p 8080：8080 \
      -e VAULT_ADDR = https：// < VAULT_SERVER > -e VAULT_TOKEN = < TOKEN_FOR_VAULT_ACCCESS >牧场主/服务器
   ```

   > **注意：**验证您是否正在运行所需的[Rancher服务器标记](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/secrets/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/installing-rancher/installing-server/#rancher-server-tags)。

5. 一旦Rancher服务器启动，您将需要更新`service-backend`Rancher中的设置。在**管理** - > **设置** - > **高级设置下**，找到该`secrets.backend`值。默认情况下，它将`localkey`作为值。您可以将其编辑为值`vault`。

> **注意：**目前，Rancher不支持在加密后端类型之间切换。

### 创造秘密

在环境级别创建和限定秘密，这意味着在环境中只能有一个具有相同名称的秘密。同一环境中的任何容器都可以共享相同的秘密。例如，数据库密码，即`db_password`可以在数据库容器和Wordpress容器中使用。

> **注意：** Rancher CLI目前不支持为Rancher添加秘密。

去**基础设施** - > **秘密**。提供**名称**和**秘密价值**并**保存**秘密。

创建秘密后，**无法**编辑或更新密码。如果您需要更改密码的现有值，唯一的方法是删除该密码。在更新了一个秘密后，使用秘密的任何服务都将需要重新启动并使用更新的秘密。

### 删除秘密

在UI中，秘密可以从Rancher中删除，但是它不会从使用秘密的任何容器或使用秘密运行容器的主机上删除秘密（即文件）。

### 启用容器中的秘密

为了消费容器中的**秘密**，**Rancher Secrets**服务将需要启动。该服务可以通过将其添加到[环境模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/secrets/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/#what-is-an-environment-template)中进行部署，以便将其部署在所有[环境中，](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/secrets/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)或者直接从[Rancher目录](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/secrets/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/catalog)启动。如果要将此服务添加到现有环境中，请导航到**Catalog** - > **Library**，然后搜索**Rancher Secrets**条目。没有启动此目录条目，您将只能创建秘密，但无法在容器中使用它们。

### 向服务/容器添加秘密

在服务/容器创建过程中，秘密可以添加到**Secrets**选项卡下的服务/容器中。

1. 选择**添加秘密**
2. 在Rancher中创建的可用秘密列表可在下拉列表中找到。选择您要使用的秘密。
3. （可选）为密码文件输入不同的文件名。默认情况下，它将使用文件名的秘密名称。
4. （可选）如果需要修改默认文件模式，请单击“自**定义文件所有权和权限”**链接。用户ID，组ID和文件模式（八进制）可以更新。默认情况下，用户ID为`0`组ID `0`，文件模式为`0444`。
5. 单击**创建**。

当将秘密添加到容器时，将将秘密写入tmpfs卷，这可以从容器和主机访问。

- 容器内部：体积安装在`/run/secrets/`。
- 在使用秘密运行容器的主机上：该卷被安装在`/var/lib/rancher/volumes/rancher-secrets/`。

> **注意：** Rancher CLI目前不支持在容器中使用秘密。

### Docker Hub图像

Docker在许多官方存储库中提供了支持，以通过文件传递秘密。要利用这个，附加`_FILE`到环境变量名称和值将是`/run/secrets/NAME>`。当容器启动时，文件中的值将被分配给环境变量。

例如，启动MySQL容器时，可以将环境变量设置为：

```
-e MYSQL_ROOT_PASSWORD_FILE=/run/secrets/db_password

```

该`MYSQL_ROOT_PASSWORD`环境变量将使用值从文件。

### 已知的漏洞

#### 妥协的牧场服务器容器

存储在Rancher中的秘密包含与CI系统相同的信任水平，如Travis CI和Drone。由于加密密钥直接存储在Rancher服务器容器中，所以Rancher服务器的任何妥协都应该被视为秘密数据的妥协。Rancher将在以后的版本中努力减轻这种情况。

> **注意：**如果您正在使用保险库进行加密，请创建一个策略，限制Rancher服务器使用的令牌的访问。

#### 被妥协的主机

如果主机受到威胁，则可以访问主机上所有容器的秘密。攻击者无法列出或请求放置在主机上的其他秘密。

#### 容器访问

如果用户有权访问能够执行到容器中，则可以通过容器中存储的卷来访问容器的秘密。容器可以通过以下方法访问：

- UI访问通过“Exec Shell”
- 牧场CLI
- 搬运工人