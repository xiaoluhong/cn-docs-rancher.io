---
title: Rancher API - account
layout: rancher-api-v1-default-v1.6
version: v1.6
lang: zh
apiVersion: v1
#redirect_from:
#  - /rancher/v1.6/zh/api/v1/api-resources/account/
---

## 帐户

在Rancher中所有资源都是通过一个帐户拥有或创建的。

### 资源域	

#### 可写域

| Field          | Type   | Create   | Update | Default | Notes |
| -------------- | ------ | -------- | ------ | ------- | ----- |
| description    | string | Optional | Yes    | -       |       |
| externalId     | string | Optional | Yes    | -       |       |
| externalIdType | string | Optional | Yes    | -       |       |
| name           | string | Optional | Yes    | -       |       |


#### 只读域

| Field    | Type                                     | Notes                                 |
| -------- | ---------------------------------------- | ------------------------------------- |
| data     | map[json]                                |                                       |
| id       | int                                      | The unique identifier for the account |
| identity | [identity]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/identity/) |                                       |




请阅读更多有关[公共资源领域]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}//common/)。这些字段只适用于读取几乎所有资源。我们从上面的列表隔离他们。

### 操作
{::options parse_block_html="true" /}

CreatePOST: `/v1/projects/${PROJECT_ID}/accounts`

{% highlight json %} curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \ -X POST \ -H 'Content-Type: application/json' \ -d '{ "description": "string", "externalId": "string", "externalIdType": "string", "kind": "user", "name": "string", "uuid": "string" }' 'http://${RANCHER_URL}:8080/v1/projects/${PROJECT_ID}/accounts' {% endhighlight %}

DeleteDELETE: `/v1/projects/${PROJECT_ID}/accounts/${ID}`

{% highlight json %} curl -u "${RANCHER_ACCESS_KEY}:​${RANCHER_SECRET_KEY}" \ -X DELETE \ 'http://${RANCHER_URL}:8080/v1/projects/​${PROJECT_ID}/accounts/${ID}' {% endhighlight %}

UpdatePUT: `/v1/projects/${PROJECT_ID}/accounts/${ID}`

{% highlight json %} curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \ -X PUT \ -H 'Content-Type: application/json' \ -d '{ "description": "string", "externalId": "string", "externalIdType": "string", "kind": "user", "name": "string" }' 'http://${RANCHER_URL}:8080/v1/projects/${PROJECT_ID}/accounts/${ID}' {% endhighlight %}



### Actions

activate POST: `/v1/projects/${PROJECT_ID}/accounts/${ID}?action=activate`

**Input:**This action has no inputs 
{% highlight json %} curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \ -X POST \ 'http://${RANCHER_URL}:8080/v1/projects/${PROJECT_ID}/accounts/${ID}?action=activate' {% endhighlight %} 
**Output:** An updated copy of the [account](http://git.oschina.net/hongxl/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/api-resources/account/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/%7B%7Bpage.apiVersion%7D%7D/api-resources/account/) resource

deactivate POST: `/v1/projects/${PROJECT_ID}/accounts/${ID}?action=deactivate`

**Input:**This action has no inputs 
{% highlight json %} curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \ -X POST \ 'http://${RANCHER_URL}:8080/v1/projects/${PROJECT_ID}/accounts/${ID}?action=deactivate' {% endhighlight %} 
**Output:** An updated copy of the [account](http://git.oschina.net/hongxl/rancher.github.io/blob/master/rancher/v1.6/en/api/v1/api-resources/account/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/api/%7B%7Bpage.apiVersion%7D%7D/api-resources/account/) resource


