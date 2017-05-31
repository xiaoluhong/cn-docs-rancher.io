---
title: Rancher API - agent
layout: rancher-api-v2-beta-default-v1.6
version: v1.6
lang: zh
apiVersion: v2-beta
#redirect_from:
#  - /rancher/v1.6/zh/api/v2-beta/api-resources/agent/
---

## 代理



### 资源领域

#### 可写字段

| Field       | Type   | Create   | Update | Default | Notes |
| ----------- | ------ | -------- | ------ | ------- | ----- |
| description | string | Optional | Yes    | -       |       |
| name        | string | Optional | Yes    | -       |       |


#### 只读字段

| Field         | Type      | Notes                               |
| ------------- | --------- | ----------------------------------- |
| data          | map[json] |                                     |
| id            | int       | The unique identifier for the agent |
| managedConfig | boolean   |                                     |
| uri           | string    |                                     |


<br>

请阅读有关 [公共资源领域的]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/common/). 更多信息。这些字段是只读的，适用于几乎每个资源。我们把它们从上面的列表中分离出来。




### 操作

<div class="action" id="activate">
<span class="header">
activate
<span class="headerright">POST:  <code>/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=activate</code></span></span>
<div class="action-contents">

<br>
<span class="input">
<strong>Input:</strong>This action has no inputs</span>

<br>
{% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X POST \
'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=activate'
{% endhighlight %}
<br>
<span class="output"><strong>Output:</strong> An updated copy of the <a href="{{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/agent/">agent</a> resource</span>
</div></div>

<div class="action" id="deactivate">
<span class="header">
deactivate
<span class="headerright">POST:  <code>/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=deactivate</code></span></span>
<div class="action-contents">

<br>
<span class="input">
<strong>Input:</strong>This action has no inputs</span>

<br>
{% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X POST \
'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=deactivate'
{% endhighlight %}
<br>
<span class="output"><strong>Output:</strong> An updated copy of the <a href="{{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/agent/">agent</a> resource</span>
</div></div>

<div class="action" id="disconnect">
<span class="header">
disconnect
<span class="headerright">POST:  <code>/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=disconnect</code></span></span>
<div class="action-contents">

<br>
<span class="input">
<strong>Input:</strong>This action has no inputs</span>

<br>
{% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X POST \
'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=disconnect'
{% endhighlight %}
<br>
<span class="output"><strong>Output:</strong> An updated copy of the <a href="{{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/agent/">agent</a> resource</span>
</div></div>

<div class="action" id="finishreconnect">
<span class="header">
finishreconnect
<span class="headerright">POST:  <code>/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=finishreconnect</code></span></span>
<div class="action-contents">

<br>
<span class="input">
<strong>Input:</strong>This action has no inputs</span>

<br>
{% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X POST \
'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=finishreconnect'
{% endhighlight %}
<br>
<span class="output"><strong>Output:</strong> An updated copy of the <a href="{{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/agent/">agent</a> resource</span>
</div></div>

<div class="action" id="reconnect">
<span class="header">
reconnect
<span class="headerright">POST:  <code>/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=reconnect</code></span></span>
<div class="action-contents">

<br>
<span class="input">
<strong>Input:</strong>This action has no inputs</span>

<br>
{% highlight json %}
curl -u "${RANCHER_ACCESS_KEY}:${RANCHER_SECRET_KEY}" \
-X POST \
'http://${RANCHER_URL}:8080/v2-beta/projects/${PROJECT_ID}/agents/${ID}?action=reconnect'
{% endhighlight %}
<br>
<span class="output"><strong>Output:</strong> An updated copy of the <a href="{{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/api/{{page.apiVersion}}/api-resources/agent/">agent</a> resource</span>
</div></div>


