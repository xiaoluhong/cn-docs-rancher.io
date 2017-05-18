---
title: Creating Private Catalogs
layout: rancher-default-v1.6
version: v1.6
lang: en
---

#### 创建私人目录

------

Rancher目录服务需要以特定格式构建私有目录，以使目录服务能够将其转换为Rancher。

### 模板文件夹

根据为环境选择的容器编排类型，目录模板显示在Rancher中。

#### 基于编排类型的模板

* _Cattle_ orchestration: Entries in the UI are from the `templates` folder
* _[Swarm]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/swarm/)_ orchestration: Entries in the UI are from the `swarm-templates` folder
* _[Mesos]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/mesos/)_ orchestration: Entries in the UI are from the `mesos-templates` folder

### 基础设施服务模板

可在[环境模板](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/catalog/private-catalog/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments/#what-is-an-environment-template)中启用的[基础架构服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/catalog/private-catalog/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/rancher-services)来自Rancher中启用的任何目录的文件夹。`infra-templates`

这些服务也可从“ **目录** ”选项卡中获得，即使可能无法使用所选的编排类型，您也可以查看所有基础架构服务。建议在环境模板创建期间选择基础架构服务，而不是直接从目录中启动它们。

### 目录结构

```
-- templates (Or any of templates folder)
  |-- cloudflare
  |   |-- 0
  |   |   |-- docker-compose.yml
  |   |   |-- rancher-compose.yml
  |   |-- 1
  |   |   |-- docker-compose.yml
  |   |   |-- rancher-compose.yml
  |   |-- catalogIcon-cloudflare.svg
  |   |-- config.yml
...
```
在主目录中，您将需要一个`templates`文件夹。该`templates`文件夹将包含您所创建的每个目录项的文件夹。我们建议每个目录条目都有一个简单的模板名称作为文件夹名称。

在目录条目文件夹（例如`cloudflare`）中，将为您的目录条目创建的每个版本都有文件夹。第一个版本应该是`0`，并且每个后续版本将是增量值。例如，版本2将在`1`文件夹中。通过提供新的版本文件夹编号，它提供了一种从先前版本的模板升级堆栈的方法。或者，您可以更新`0`文件夹中的模板，只需重新部署该条目。

> **注意：**每个目录条目将需要一个单词，所以请使用`-`而不是空格用于更长的目录名称。您可以使用该`name`部分的空格`config.yml`。

### Rancher目录文件在Rancher目录中显示

在目录条目文件夹中，如何在Rancher目录中显示目录条目的详细信息位于两个文件中。

- 第一个文件`config.yml`包含您的条目的详细信息。

```yaml
name: # Name of the Catalog Entry
description: |
  # Description of the Catalog Entry
version: # Version of the Catalog to be used
category: # Category to be used for searching catalog entries
maintainer: # The maintainer of the catalog entry
license: # The license
projectURL: # A URL related to the catalog entry
```
- 第二个文件是目录条目的图标图像。该文件必须带有前缀`catalogIcon-`。

对于每一个目录条目，将有至少三个项目：`config.yml`，`catalogIcon-entry.svg`，和`0`文件夹，其中包含目录条目的第一个版本。

### 牧场主目录模板

该`docker-compose.yml`和`rancher-compose.yml`是**必需的**文件，以便能够使用牧场主启动服务[牧场主撰写](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/catalog/private-catalog/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services/#adding-services-with-rancher-compose)。这些文件所在的文件夹中的版本号（即内`0`，`1`等）。

本`docker-compose.yml`应该是还可以使用启动文件`docker-compose up`。服务遵循docker-compose格式。

在`rancher-compose.yml`将包含更多的信息，以帮助您自定义目录条目。在该`catalog`部分中，为了使您的目录条目正确解释，将需要一些字段。

`README.md`可以创建一个可选项，它提供了有关如何使用目录服务的冗长描述或注释。

**rancher-compose.yml**

```yaml
version: '2'
catalog:
  name: # Name of the versioned template of the Catalog Entry
  version: # Version of the versioned template of the Catalog Entry
  description: # Description of the versioned template of the Catalog Entry
  minimum_rancher_version: # The minimum version of Rancher that supports the template, v1.0.1 and 1.0.1 are acceptable inputs
  maximum_rancher_version: # The maximum version of Rancher that supports the template, v1.0.1 and 1.0.1 are acceptable inputs
  upgrade_from: # The previous versions that this template can be upgraded from
  questions: #Used to request user input for configuration options
```
因为`upgrade_from`，可以使用三种类型的值。

1. 仅允许从1版本升级： `1.0.0`
2. 如果能够比某个特定版本选择更高或更低：`>=1.0.0.`，`<=2.0.0`
3. 能够定义[一系列版本](https://github.com/blang/semver#ranges)：`>1.0.0 <2.0.0 || >3.0.0`

### 问题在 `rancher-compose.yml`

该`questions`部分`catalog`用于允许用户更改服务的配置选项。该`answers`会内的填充`docker-compose.yml`是推出服务之前。

每个配置选项是该`questions`部分的列表项`rancher-compose.yml`。

```yaml
version: '2'
catalog:
  questions:
    - variable: # A single word that is used to pair the question and answer.
      label: # The "question" to be answered.
      description: | # The description of the question to show the user how to answer the question.
      default: # (Optional) A default value that will be pre-populated into the UI
      required: # (Optional) Whether or not an answer is required. By default, it's considered `false`.
      type: # How the questions are formatted and types of response expected
```
* #### 类型

  该`type`部分控制问题在UI中的格式以及预期的响应类型。

  符合条件的格式有：

  - `string` 用户界面中将显示一个文本框来捕获答案，答案将被格式化为一个字符串。
  - `int`用户界面中将显示一个文本框以捕获答案，答案将被格式化为一个数字。UI将在启动模板之前验证其是否为有效的数字。
  - `boolean`用户界面中将显示一个单选按钮来捕获答案，答案将被格式化为`true`或`false`。如果选择单选按钮，答案将被格式化为`true`。
  - `password` 用户界面中将显示一个文本框来捕获答案，答案将被格式化为一个字符串。
  - `service` 将显示环境中所有服务的下拉列表。
  - `enum`用户界面中将显示一个下拉菜单，下拉菜单中将显示该`options`部分。

```yaml
version: '2'
catalog:
  questions:
    - variable:
      label:
      description: |
      type: enum   
      options: # List of options if using type of `enum`
        - Option 1
        - Option 2
```

* `multiline` 多行文本框将显示在UI中。

```yaml
version: '2'
catalog:
  questions:
    - variable:
      label:
      description: |
      type: multiline
      default: |
        Each line
        would be shown
        on a separate
        line.
```

* `certificate` 在环境中下载所有可用的证书。

```yaml
version: '2'
catalog:
  questions:
    - variable:
      label:
      description: |
      type: certificate
```

### 基于Yeoman的目录生成器

有一个基于[Yeoman ](http://yeoman.io/)[的开源项目](https://github.com/slashgear/generator-rancher-catalog)，可以用于创建空目录条目的模板。