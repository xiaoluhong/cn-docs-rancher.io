---
title: Rancher API - account
layout: rancher-api-v2-beta-default-v1.6
version: v1.6
lang: zh
apiVersion: v2-beta
#redirect_from:
#  - /rancher/v1.6/zh/api/v2-beta/api-resources/account/
---

## 帐户


Rancher中的所有资源都由帐户拥有或创建。

### 资源领域

#### 可写字段

| Field          | Type   | Create   | Update | Default | Notes |
| -------------- | ------ | -------- | ------ | ------- | ----- |
| description    | string | Optional | Yes    | -       |       |
| externalId     | string | Optional | Yes    | -       |       |
| externalIdType | string | Optional | Yes    | -       |       |
| name           | string | Optional | Yes    | -       |       |


#### 只读字段

| Field    | Type                                     | Notes                                 |
| -------- | ---------------------------------------- | ------------------------------------- |
| data     | map[json]                                |                                       |
| id       | int                                      | The unique identifier for the account |
| identity | [identity]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/identity/) |                                       |
| version  | string                                   |                                       |


<br>

请阅读有关公[共资源领域的]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/rancher/v1.6/zh/api/v2-beta/common/
)更多信息。这些字段是只读的，适用于几乎每个资源。我们把它们从上面的列表中分离出来。



### 操作
{::options parse_block_html="true" /}
<a id="创建"></a>
<div class="action"><span class="header">Create<span class="headerright">POST:  <code>/v2-beta/projects/${PROJECT_ID}/accounts</code></span></span>
<div class="action-contents"> {% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X POST \
-H 'Content-Type: application/json' \
-d '{
	"description": "string",
	"externalId": "string",
	"externalIdType": "string",
	"kind": "user",
	"name": "string",
	"uuid": "string"
}' 'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/accounts'
{% endhighlight %}
</div></div>
<a id="删除"></a>
<div class="action"><span class="header">Delete<span class="headerright">DELETE:  <code>/v2-beta/projects/${PROJECT_ID}/accounts/${ID}</code></span></span>
<div class="action-contents"> {% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X DELETE \
'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/accounts/${ID}'
{% endhighlight %}
</div></div>
<a id="更新"></a>
<div class="action"><span class="header">Update<span class="headerright">PUT:  <code>/v2-beta/projects/${PROJECT_ID}/accounts/${ID}</code></span></span>
<div class="action-contents"> {% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X PUT \
-H 'Content-Type: application/json' \
-d '{
	"description": "string",
	"externalId": "string",
	"externalIdType": "string",
	"kind": "user",
	"name": "string"
}' 'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/accounts/${ID}'
{% endhighlight %}
</div></div>



### 激活

<div class="action" id="activate">
<span class="header">
activate
<span class="headerright">POST:  <code>/v2-beta/projects/${PROJECT_ID}/accounts/${ID}?action=activate</code></span></span>
<div class="action-contents">

<br>
<span class="输入">
<strong>Input:</strong>此操作没有输入</span>

<br>
{% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X POST \
'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/accounts/${ID}?action=activate'
{% endhighlight %}
<br>
<span class="输出"><strong>Output:</strong> 帐户资源的更新副本帐户资源的更新副本 <a href="{{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/account/">account</a> resource</span>
</div></div>

<div class="action" id="停用">
<span class="header">
deactivate
<span class="headerright">POST:  <code>/v2-beta/projects/${PROJECT_ID}/accounts/${ID}?action=deactivate</code></span></span>
<div class="action-contents">

<br>
<span class="输出：">
<strong>Input:</strong>停用</span>

<br>
{% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X POST \
'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/accounts/${ID}?action=deactivate'
{% endhighlight %}
<br>
<span class="输出："><strong>Output:</strong> 帐户资源的更新副本 <a href="{{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/account/">account</a> resource</span>
</div></div>


