---
title: FAQS about Rancher Agents/Hosts
layout: rancher-default-v1.6
version: v1.6
lang: en
---

## 关于Rancher代理/主机的常见问题

------

### 如何在Proxy后设置主机？

要支持代理后面的主机，您需要编辑Docker守护程序以指向代理。我们[添加自定义主机页面](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/faqs/agents/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom/#hosts-behind-a-proxy)中列出了详细的说明。

### Rancher代理人无法启动的原因是什么？

#### 加入 `--name rancher-agent`

如果您`docker run .... rancher/agent...`从UI中编辑了添加的命令`--name rancher-agent`，那么Rancher代理将无法启动。Rancher代理在初始运行后启动3个不同的容器。将有1个运行容器和2个停止的容器。容器命名`rancher-agent`和`rancher-agent-state`所需的养牛代理成功地与牧场主服务器连接。具有默认Docker名称的第三个容器可以被删除。

#### 使用克隆的VM

如果您克隆了一个虚拟机并尝试注册克隆的虚拟机，那么它将无法正常工作，并且会在该主机代理日志中发生错误。`ERROR: Please re-register this agent.`养殖者保存的唯一ID `/var/lib/rancher/state`对于克隆的虚拟机将是相同的，并且无法重新注册。

解决方法是在克隆的VM上运行以下命令`rm -rf /var/lib/rancher/state; docker rm -fv rancher-agent; docker rm -fv rancher-agent-state`，一旦完成，您可以再次注册服务器。

### 在哪里可以找到Rancher Agent容器的详细日志？

从v1.6.0开始`docker logs`，在Rancher代理容器上运行将提供一组与代理相关的所有日志。

### 如何检查您的主机注册是否设置正确？

如果您有与Rancher代理连接Rancher服务器的问题，请检查您的主机是否已设置。当您首次尝试在Rancher UI中添加主机时，您必须设置主机注册URL。该URL用于建立从主机到Rancher API服务器的连接。该URL必须可以从您的主机访问。要验证它是否登录到主机并执行curl命令：

```
curl -i <Host Registration URL you set in UI>/v1

```

你应该得到一个json响应。如果认证开启，响应代码应为401.如果认证未打开，则响应代码应为200。

> **注意：**使用正常的HTTP连接和websocket连接（ws：//）。如果此URL指向代理或负载平衡器，请确保它已配置为处理Websocket连接。

### 主机如何确定IP地址，如何更改？如果主机的IP更改（由于重启），该怎么办？

当代理连接到Rancher服务器时，它会自动检测代理的IP。有时候，所选择的IP不是您要使用的IP，还是选择了docker bridge IP，即`172.17.x.x`。

或者，如果您已经在重新启动后注册了主机并且您的主机具有新的IP，那么您的主机在用户界面中的IP将不再匹配。

您可以覆盖`CATTLE_AGENT_IP`设置，并将主机IP设置为您想要的。

当主机IP地址不正确时，容器将无法访问受管网络。要使主机和所有容器进入托管网络，只需通过将新IP指定为环境变量（即步骤4），编辑要[添加到自定义主机](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/faqs/agents/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/hosts/custom)中的命令。在主机上运行编辑的命令。不要停止或删除主机上的现有代理！

```
$ sudo docker run -d -e CATTLE_AGENT_IP = < NEW_HOST_IP > --privileged \
    -v /var/run/docker.sock:/var/run/docker.sock \
    牧师/代理：v0.8.2 http：// SERVER_IP：8080 / v1 / scripts / xxxx
```

### 如果我的主机在Rancher外被删除会怎么样？

如果您的主机在Rancher之外被删除，那么Rancher服务器将继续显示主机，直到它被删除。这些主机最初将处于*重新连接*状态，然后才能进入`Disconnected`状态。您将能够**删除**这些主机以从UI中删除它们。

如果您有将容器部署到`Disconnected`主机上的服务，那么只有在添加了[运行状况检查后](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/faqs/agents/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/health-checks)，它们才会重新安排到其他主机。

### 为什么同一主机多次显示在UI中？

如果您正在使用boot2docker添加主机，则主机无法持久化`var/lib/rancher`，这是Rancher用于存储用于标识主机的必要信息。