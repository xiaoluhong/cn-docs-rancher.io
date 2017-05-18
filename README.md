# Rancher Labs 文档

此README文件包括有关Rancher和Rancher OS的文档项目的信息。

## Rancher

Rancher是一个开源项目，为Docker在生产中提供了一个完整的平台。它提供基础架构服务，如多主机网络，全局和本地负载平衡以及卷快照。它集成了Docker Machine和Docker Swarm等本地Docker管理功能。它提供了丰富的用户体验，使得管理员能够在大规模生产中操作Docker。

## Rancher OS

操作系统完全由Docker容器组成。RancherOS中的所有内容都是由Docker管理的容器。这包括系统服务，如udev和syslog。RancherOS仅包含运行Docker所需的最少量的软件。

## Rancher Labs Documentation website

Rancher文档可在http://docs.rancher.com/rancher/获得。

随着Rancher已经走出GA，我们加入了版本控制。默认的Rancher文档站点始终是指最新版本。我们将分支被认为是GA的具体版本的Rancher，也将被标记为rancher/server:stable。

目前，我们已经添加了对每个社区请求的中文版文档站点的支持。目前，Rancher不会积极翻译文档网站，但我们欢迎PRs。

Rancher操作系统文档可从http://docs.rancher.com/os/获取。

## 为Rancher Labs文档项目做出贡献

### 关于Rancher Labs文档站点

Rancher Labs文档托管在GitHub Pages上，并通过使用Jekyll在线发布，Jekyll是一个易于浏览的静态网站生成器。有关如何设置Jekyll的更多详细信息，我们建议您阅读https://help.github.com/articles/using-jekyll-with-pages/。如果您使用Windows，我们强烈建议您按照http://jekyllrb.com/docs/windows/中的说明进行操作。

有关编辑.md文件（Markdown）的信息，请参阅https://daringfireball.net/projects/markdown/syntax。

### 设置Git环境

在您的浏览器中，导航到https://github.com/rancher/rancher.github.io。

通过点击右上角的“叉”按钮将该仓库叉上。为您生成存储库的一个fork。在您的分支页面的右侧，在“HTTPS克隆URL”下，通过单击“复制到剪贴板”图标复制URL。

在您的计算机上，按照以下步骤设置本地存储库以开始处理文档：

```shell
git clone https://github.com/YOUR_ACCOUNT/rancher.github.io.git
cd rancher.github.io
git remote add upstream https://github.com/rancher/rancher.github.io
git checkout master
git fetch upstream
git merge upstream/master
```

### 更新文件

我们建议您创建一个新的分支来更新文档文件，并且不要干扰主分支，而不是从中更改upstream/master。例如，您dev在计算机上创建一个分支，在本地进行文档更改。该dev分支将是您的本地存储库，然后将其推送到您在GitHub上的分支存储库，您将在其中创建一个Pull请求，以将更改提交到官方文档中。

每当您想要对文档做出贡献并且只跟踪该分支中的拉动请求的更改时，创建一个新的分支是一种健康的做法。

```shell
git checkout -b dev
```

该参数-b dev创建一个名为的新分支dev。现在您可以对文档进行必要的更改。

```shell
git add .
git commit -a -m "commit message for your changes"
```

您可以选择在计算机上运行Jekyll本地，以便能够看到修改的最终结果，并写入它们。为此，请使用以下命令。您可以参考Jekyll的官方网站了解更多详情。

```shell
jekyll serve
```

### 将上游/主机并入您的本地分支（dev）

在本地存储库中维护最新的主分支。将每日更改从upstream/master（官方文档资料库）合并到本地存储库中。

在您开始处理功能之前，以及在作为拉取请求提交更改之前，请确保完成此活动。

您还可以在处理更改时定期执行此过程，以确保您正在处理最新版本的文档。

```shell
# 检查你的本地“主”分支。
git checkout master

# 将本地存储库与'upstream / master'同步，以便您具有所有最新的更改。
git fetch upstream

# 将最新的“上游/主机”更改合并到本地“主”分支中以使其更新。
git merge upstream/master

# 查看你的本地开发分支（例如：'dev'）。
git checkout dev

# 将最新的更改拉取到您的本地开发分支。
git pull master
```

### 在GitHub上提出请求

重要：upstream/master在执行以下操作之前，确保已经合并到您的开发分支。

在对文档进行必要的更改并准备提交文档之后，请在GitHub上创建一个Pull请求。您可以将更改推送到您的分支库（通常称为origin），然后启动拉取请求。

```
git push origin master
git push origin dev
```

现在按照以下步骤在GitHub上启动Pull请求。

1、浏览浏览器到您的分支库：https：//github.com/YOUR_ACCOUNT/rancher.github.io.git。
2、点击分叉存储库上侧的比较和拉取请求按钮。
3、输入您所做更改的明确说明。
4、单击发送拉请求。

如果您被要求修改您的建议的更改，请在您的dev分支上本地进行更改并推送更改。拉请求将自动更新。

### 清理本地存储库

dev变更完成后，您不再需要分支upstream/master。如果要进行其他文档更改，请使用新的分支重新启动该过程。

```
git checkout master
git branch -D dev
git push origin :dev
```
