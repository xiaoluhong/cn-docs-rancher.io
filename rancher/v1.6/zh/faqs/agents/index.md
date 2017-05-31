---
title: FAQS about Rancher Agents/Hosts
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 关于Rancher agent/host的常见问题

------

### 如何在Proxy后设置host？

要支持 agent后面的host，您需要编辑Docker守护程序以指向 agent。我们[添加自定义host页面]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//hosts/custom/#hosts-behind-a-proxy)中列出了详细的说明。

### Rancher agent无法启动的原因是什么？

#### 加入 `--name rancher-agent`

如果您`docker run .... rancher/agent...`从UI中编辑了添加的命令`--name rancher-agent`，那么Rancher agent将无法启动。Rancher agent在初始运行后启动3个不同的容器。将有1个运行容器和2个停止的容器。对于Rancher agent成功地与Rancher服务器连接，名为`rancher-agent`和`rancher-agent-state`  的两个容器是必须的。具有默认Docker名称的第三个容器可以被删除。

#### 使用克隆的VM

如果您克隆了一个虚拟机并尝试注册该克隆的虚拟机，那么它将无法正常工作，并且会在该host agent日志中发生错误。`ERROR: Please re-register this agent.` rancher 保存在 `/var/lib/rancher/state`下的unique ID，对于克隆的虚拟机将是相同的，并且无法重新注册。

解决方法是在克隆的VM上运行以下命令`rm -rf /var/lib/rancher/state; docker rm -fv rancher-agent; docker rm -fv rancher-agent-state`，一旦完成，您可以再次注册服务器。

### 在哪里可以找到Rancher Agent容器的详细日志？

从v1.6.0开始`docker logs`，在Rancher agent容器上运行将提供一组与 agent相关的所有日志。

### 如何检查您的host注册是否设置正确？

如果您有Rancher agent连接Rancher服务器的问题，请检查您的host是否已设置。当您首次尝试在Rancher UI中添加host时，您必须设置host注册URL。该URL用于建立从host到Rancher API服务器的连接。该URL必须可以从您的host访问。要验证它是否登录到host并执行curl命令：

```
curl -i <Host Registration URL you set in UI>/v1
```

你应该得到一个json响应。如果认证开启，响应代码应为401.如果认证未打开，则响应代码应为200。

> **注意：**使用正常的HTTP连接和websocket连接（ws://）。如果此URL指向 agent或负载平衡器，请确保它已配置为处理Websocket连接。

### host如何确定IP地址，如何更改？如果host的IP更改（由于重启），该怎么办？

当 agent连接到Rancher服务器时，它会自动检测 agent的IP。有时候，所选择的IP不是您要使用的IP，还是选择了docker bridge IP，即`172.17.x.x`。

或者，如果您已经在重新启动后注册了host并且您的host具有新的IP，那么您的host在用户界面中的IP将不再匹配。

您可以覆盖`CATTLE_AgenT_IP`设置，并将hostIP设置为您想要的。

当host IP地址不正确时，容器将无法访问受管网络。要使host和所有容器进入托管网络，只需通过将新IP指定为环境变量（即步骤4），编辑要[添加到自定义host]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//hosts/custom)中的命令。在host上运行编辑的命令。不要停止或删除host上的现有 agent！

```
sudo docker run -d -e CATTLE_AGENT_IP=<NEW_HOST_IP> --privileged \
    -v /var/run/docker.sock:/var/run/docker.sock \
    rancher/agent:v0.8.2 http://SERVER_IP:8080/v1/scripts/xxxx
```

### 如果我的host在Rancher外被删除会怎么样？

如果您的host在Rancher之外被删除，那么Rancher服务器将继续显示host，直到它被删除。这些host最初将处于*重新连接*状态，然后才能进入`Disconnected`状态。您将能够**删除**这些host以从UI中删除它们。

如果您有将容器部署到`Disconnected`host上的服务，那么只有在添加了[运行状况检查后]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}//cattle/health-checks)，它们才会重新安排到其他host。

### 为什么同一host多次显示在UI中？

如果您正在使用boot2docker添加host，则host无法持久化`var/lib/rancher`，这是Rancher用于存储用于标识host的必要信息。