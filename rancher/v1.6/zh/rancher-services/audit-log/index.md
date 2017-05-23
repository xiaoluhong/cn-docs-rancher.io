---
title: Audit Logging in Rancher
layout: rancher-default-v1.6
version: v1.6
lang: cn
---

## 审核记录

------

只有[管理员](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/cn/rancher-services/audit-log/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/configuration/access-control/#admin)才能访问审核日志。审核日志可以在**管理** - > **审核日志中**找到。

Rancher的审核日志是不同事件类型的汇编。

- 任何前缀`api`都是调用我们的API。事件类型将记录API操作，谁执行操作以及API的调用方式（即通过UI，通过API密钥）。
- **没有**前缀的任何其他事件都是`api`Rancher服务器正在执行的事件。例如，在协调服务的容器期间，可以创建将记录`instance.create`事件的实例。