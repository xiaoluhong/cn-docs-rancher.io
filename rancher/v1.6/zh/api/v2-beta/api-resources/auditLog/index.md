---
title: Rancher API - auditLog
layout: rancher-api-v2-beta-default-v1.6
version: v1.6
lang: zh
apiVersion: v2-beta
#redirect_from:
#  - /rancher/v1.6/zh/api/v2-beta/api-resources/auditLog/
---

## 审计日志

审计日志提供了一个API请求列表。它记录环境以及API调用。启用访问控制后，还会记录用户。

### 资源领域


#### 只读字段

| Field                     | Type                                     | Notes                                    |
| ------------------------- | ---------------------------------------- | ---------------------------------------- |
| authType                  | enum                                     | The options are `AdminAuth`, `BasicAuth`, `RegistrationToken`, `TokenAccount`, `TokenAuth`. |
| authenticatedAsAccountId  | [account]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/account/) |                                          |
| authenticatedAsIdentityId | [identity]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/identity/) |                                          |
| clientIp                  | string                                   |                                          |
| description               | string                                   |                                          |
| eventType                 | string                                   |                                          |
| id                        | int                                      | The unique identifier for the auditLog   |
| requestObject             | string                                   |                                          |
| resourceId                | int                                      |                                          |
| resourceType              | string                                   |                                          |
| responseCode              | string                                   |                                          |
| responseObject            | string                                   |                                          |


<br>

请阅读有关[公共资源领域的]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/common/). T更多信息。这些字段是只读的，适用于几乎每个资源。我们把它们从上面的列表中分离出来。




