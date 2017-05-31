---
title: Internal DNS Service in Cattle Environments
layout: rancher-default-v1.6
version: v1.6
lang: zh
---

## 内部DNS服务

------

在Rancher内，我们拥有自己的内部DNS服务，允许一个[牛环境](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/internal-dns-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/environments)中的所有服务都能够解决环境中的任何其他服务。

堆栈中的所有服务都可以解析，`<service_name>`并且不需要在服务之间设置服务链接。创建服务时，您可以定义**服务链接**以将服务链接在一起。对于不同堆栈的任何服务，您都可以解决`<service_name>.<stack_name>`而不是仅仅是`<service_name>`。如果您想以不同的名称解析服务，则可以设置服务链接，以便服务可以由服务别名解析。

### 通过链接设置服务别名

在UI中，[添加服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/internal-dns-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services/#adding-services-in-the-ui)时，展开**服务链接**部分，选择服务，并提供别名。

如果您使用Rancher Compose [添加服务](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/internal-dns-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/services/#adding-services-with-rancher-compose)，`docker-compose.yml`则会使用`links`或`external_links`指令。

```
版本：' 2 '
服务：
   service1：
     image：wordpress 
    ＃如果其他服务是在相同的堆栈
    链接：
     ＃ <service_name>：<service_alias> 
    - service2 ：mysql 
    ＃如果其他服务是在不同的堆栈
    external_links：
     ＃ < stackname> / <service_name>：<service_alias> 
    - 默认/ service3：mysql
```

### 边条和链接

启动服务时，您可能需要一直在同一台主机上一起启动服务。特定用例包括尝试使用`volumes_from`或`net`从其他服务使用时。当创建一个[sidekick关系](https://github.com/rancher/rancher.github.io/blob/master/rancher/v1.6/en/cattle/internal-dns-service/%7B%7Bsite.baseurl%7D%7D/rancher/%7B%7Bpage.version%7D%7D/%7B%7Bpage.lang%7D%7D/cattle/adding-services/#sidekick-services)时，这些服务可以通过他们的名字自动地相互解析。我们目前不支持通过sidekick服务中的links / external_links创建服务别名。

当创建一个副作用关系时，总是有一个主要的服务和一个服务。它们一起被认为是单个启动配置。此启动配置将作为一组容器部署到主机上，1个来自主服务，1个从每个sidekick定义。在启动配置中的任何服务中，您可以按其名称解析主要和副本。对于启动配置之外的任何服务，主服务可以通过名称解析，但是sidekick服务只能解析`<sidekick_name>.<primary_service_name>`。

### 集装箱名称

所有容器都可以通过其名称全局解析，因为每个服务的容器名称在每个环境中都是唯一的。没有必要附加服务名称或堆栈名称。

### 例子

#### 平铺服务在同一堆栈

如果您执行到容器的shell，则可以通过服务名称ping同一堆栈中的其他服务。

在我们的例子中，有一个名为堆栈`stackA`有两个服务，`foo`和`bar`。

执行服务中的一个容器后`foo`，可以ping该`bar`服务。

```
$ ping吧
PING bar.stacka.rancher.internal（10.42.xx）58（84）个字节的数据。
来自10.42.xx的64字节：icmp_seq = 1 ttl = 62 time = 1.63 ms
来自10.42.xx的64字节：icmp_seq = 2 ttl = 62 time = 1.13 ms
从10.42.xx的64字节：icmp_seq = 3 ttl = 62时间= 1.07毫秒
```

#### Pinging服务在不同的堆栈

对于不同堆栈的服务，您可以使用不同的堆栈来ping服务`<service_name>.<stack_name>`。

在我们的示例中，我们有一个称为的栈`stackA`，它包含一个被称为服务`foo`的栈，我们也有一个叫做栈的栈`stackB`，它包含一个叫做的服务`bar`。

如果我们执行服务中的一个容器`foo`，您可以ping该`bar`服务`bar.stackb`。

```
$ ping bar.stackb
PING bar.stackb（10.42.xx）56（84）个字节的数据。
来自10.42.xx的64个字节：icmp_seq = 1 ttl = 62 time = 1.43 ms
来自10.42.xx的64字节：icmp_seq = 2 ttl = 62 time = 1.15 ms
来自10.42.xx的64字节：icmp_seq = 3 ttl = 62 time = 1.27 ms
```

#### Pinging Sidekick服务

根据您从哪个服务ping，您可以通过任一`<sidekick_name>`或两者来达成一个配偶服务`<sidekick_name>.<primary_service_name>`。

在我们的示例中，我们有一个名为stack的栈`stackA`，它包含一个叫做`foo`sidekick `bar`和一个叫做service的服务`hello`。我们还有一个堆栈`stackB`，它包含一个服务`world`。

如果我们执行服务中的一个容器`foo`，您可以`bar`直接通过其名称ping 服务。

```
＃在“foo”服务中的一个容器内，“bar”是一个副作用。
$ ping吧
PING bar.foo.stacka.rancher.internal（10.42.xx）56（84）个字节的数据。
从10.42.xx的64字节：icmp_seq = 1 ttl = 64时间= 0.060毫秒
来自10.42.xx的64字节：icmp_seq = 2 ttl = 64 time = 0.111 ms
来自10.42.xx的64字节：icmp_seq = 3 ttl = 64 time = 0.114 ms
```

如果我们执行到`hello`服务中的一个容器，这是同一个堆栈，您可以ping `foo`服务`foo`和`bar`sidekick服务`bar.foo`。

```
＃里面在`hello`服务，这是不是服务/搭档服务的一部分的容器中的一个
＃平主要服务（即富）
$ ping foo
PING foo.stacka.rancher.internal（10.42.xx）56（84）个字节的数据。
从10.42.xx的64字节：icmp_seq = 1 ttl = 62时间= 1.04毫秒
来自10.42.xx的64字节：icmp_seq = 2 ttl = 62 time = 1.40 ms
从10.42.xx的64字节：icmp_seq = 3 ttl = 62时间= 1.07毫秒
＃平的搭档服务（即条）
$ ping bar.foo
PING bar.foo（10.42.xx）56（84）个字节的数据。
来自10.42.xx的64字节：icmp_seq = 1 ttl = 62 time = 1.01 ms
来自10.42.xx的64字节：icmp_seq = 2 ttl = 62 time = 1.12 ms
从10.42.xx的64字节：icmp_seq = 3 ttl = 62时间= 1.05毫秒
```

如果我们执行到`world`服务中的一个容器（位于不同的堆栈中），则可以`foo`通过该服务`foo.stacka`和`bar`sidekick服务ping 服务`bar.foo.stacka`。

```
＃里面在`world`服务，这是在不同的堆叠中的容器中的一个
＃平主要服务（即富）
$ ping foo.stacka
PING foo.stacka（10.42.xx）56（84）个字节的数据。
从10.42.xx的64字节：icmp_seq = 1 ttl = 62时间= 1.13毫秒
从10.42.xx的64字节：icmp_seq = 2 ttl = 62时间= 1.05毫秒
从10.42.xx的64字节：icmp_seq = 3 ttl = 62时间= 1.29毫秒
＃平的搭档服务（即条）
$ ping bar.foo.stacka
PING bar.foo.stacka（10.42.xx）56（84）个字节的数据。
从10.42.xx的64字节：icmp_seq = 1 ttl = 62时间= 1.23毫秒
来自10.42.xx的64个字节：icmp_seq = 2 ttl = 62 time = 1.00 ms
来自10.42.xx的64个字节：icmp_seq = 3 ttl = 62 time = 0.994 ms
```

#### 平仓集装箱名称

在任何容器中，您可以通过其名称在环境中ping另一个容器，无论它们在不同的堆栈或服务中。

在我们的示例中，我们有一个调用的堆栈`stackA`，它包含一个名为的服务`foo`。我们还有另外一个堆栈`stackB`，它包含一个叫做的服务`bar`。容器的名称是`<stack_name>-<service_name>-<number>`。

如果我们执行服务中的一个容器`foo`，您可以ping服务中的容器`bar`。



```bash
$ ping stackB-bar-1
PING stackB-bar-1.rancher.internal (10.42.x.x): 56 data bytes
64 bytes from 10.42.x.x: icmp_seq=1 ttl=62 time=1.994 ms
64 bytes from 10.42.x.x: icmp_seq=2 ttl=62 time=1.090 ms
64 bytes from 10.42.x.x: icmp_seq=3 ttl=62 time=1.100 ms
```
