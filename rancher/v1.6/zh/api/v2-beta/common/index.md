---
title: API Common Resource Fields
layout: rancher-api-v2-beta-default-v1.6
version: v1.6
lang: zh
apiVersion: v2-beta
---

## 公共资源字段

------

在API中，资源视图中已经隐藏了最常见且仅在资源上读取的资源字段。我们将其从资源字段列表中删除，以便用户更容易查看资源字段列表。


| Field                 | Type                                     | Notes                                    |
| --------------------- | ---------------------------------------- | ---------------------------------------- |
| accountId             | [account]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/account/) | The unique identifier for the associated account |
| created[TS]           | date                                     | The date/time when the resource was initially created. |
| id                    | string                                   | The unique identifier for the resource.  |
| kind                  | string                                   | A more specific sub-type for the resource. |
| removeTime            | date                                     | The date/time when the resource was removed, or null if not removed. |
| removed               | bool                                     | True if a resource has been removed.  Removed resources won't be returned in collection lists but can be individually retrieved by ID. |
| transitioning         | enum                                     | The transitioning state of the resource:  `yes`, `no`, or `error` |
| transitioningMessage  | string                                   | A message relating to the transition that is happening, or the error that occurred |
| transitioningProgress | int                                      | An estimate of far along the transition is, from 0-100. |
| uuid                  | string                                   | A universally unique identifier for the resource.  This will always be unique across Rancher installations. |
