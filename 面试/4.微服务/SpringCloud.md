# 微服务

![image-20240907165747258](https://gitee.com/w-d1107/typora-image/raw/master/image-20240907165747258.png)

---

# Spring Cloud

![image-20240907170003259](https://gitee.com/w-d1107/typora-image/raw/master/image-20240907170003259.png)

## 常见组件

> - 基础的内容考察
> - 回答原则: 简单的问题<font color='red'>**不能**</font>答错(一道面试题就能淘汰一个人)新手和老手都要注意

![image-20240907170323334](https://gitee.com/w-d1107/typora-image/raw/master/image-20240907170323334.png)

- Eureka/Nacos: 注册中心 *Nacos同时也是配置中心*
- Ribbon: 负载均衡
- Feign: 远程调用
- Hystrix/Sentinel: 服务熔断
- Zuul/Gateway: 网关

## 注册中心

**问: 服务注册和发现是什么意思? Spring Cloud如何实现服务注册发现?**

> - 微服务中必须要使用的组件, 考察我们使用微服务的程度
> - 注册中心的核心作用是: 服务注册和发现
> - 常见的注册中心: <font color='red'>eureka</font>、<font color='red'>Nacos</font>、zookeeper

<font color='red'>**我做过的某个为服务项目, 使用到了某个注册中心**</font>

### Eureka的作用

![image-20240907171558558](https://gitee.com/w-d1107/typora-image/raw/master/image-20240907171558558.png)

假设此时服务提供者有一台服务宕机了, 怎么办呢

![image-20240908093928560](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908093928560.png)

服务提供者的每一台服务都要定期向注册中心发送心跳, 证明当前服务是一个健康的实例, 假设有一台实例(比如8083)一直没有发送心跳, 注册中心90秒没有收到心跳, 就认为当前某一台实例宕机了, 此时注册中心就要从服务列表中将这个实例删除, 相对应的, 服务消费者再去拉取服务时, 也拉取不到了

### 面试官: 服务注册和发现是什么意思? Spring Cloud如何实现服务注册发现?

答: 

- 我们当时项目采用的是Eureka作为注册中心, 这个也是Spring Cloud体系中的一个核心组件
- **服务注册**: 服务提供者需要把自己的信息注册到Eureka, 由Eureka来保存这些信息, 比如服务名称、ip、端口等等
- **服务发现**: 消费者向Eureka拉取服务列表信息, 如果服务提供者有集群, 则消费者会利用负载均衡算法, 选择一个发起调用
- **服务监控**: 服务提供者会每隔30秒向Eureka发送心跳, 报告健康状态, 如果Eureka服务90秒没接收到心跳, 就会从Eureka中剔除

### Nacos的工作流程

一般情况下: 

![image-20240908102210863](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908102210863.png)

我们也可以将实例配置为永久实例(非临时实例)

![image-20240908102245514](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908102245514.png)

此时, Nacos的工作流程:

![image-20240908102304901](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908102304901.png)

### 面试官: 我看你之前也用过Nacos、你能说下Nacos与Eureka的区别吗?

- Nacos与Eureka的共同点(注册中心)
  1. 都支持服务注册和服务拉取
  2. 都支持服务提供者心跳方式做健康监测
- Nacos与Eureka的区别(注册中心)
  1. Nacos支持服务端主动监测提供者状态: 临时实例采用心跳模式, 非临时实例采用主动检测模式
  2. 临时实例心跳不正常会被剔除, 非临时实例则不会被剔除
  3. Nacos支持服务列表变更的消息推送模式, 服务列表更新更及时
  4. Nacos集群默认采用AP方式, 当集群中存在非临时实例时, 采用CP方式; Eureka采用AP方式
- Nacos还支持了配置中心, Eureka则只有注册中心, 也是选择使用Nacos的一个重要原因



## 注册中心详解

本章主要学习Nacos中的一些特性和原理，以及与Eureka的功能对比。

### 环境隔离

企业实际开发中，往往会搭建多个运行环境，例如：

- 开发环境
- 测试环境
- 预发布环境
- 生产环境

这些不同环境之间的服务和数据之间需要隔离。

还有的企业中，会开发多个项目，共享nacos集群。此时，这些项目之间也需要把服务和数据隔离。

因此，Nacos提供了基于`namespace`的环境隔离功能。具体的隔离层次如图所示：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670992-31.png)

说明：

- Nacos中可以配置多个`namespace`，相互之间完全隔离。默认的`namespace`名为`public`
- `namespace`下还可以继续分组，也就是group ，相互隔离。 默认的group是`DEFAULT_GROUP`
- `group`之下就是服务和配置了

#### 创建namespace

nacos提供了一个默认的`namespace`，叫做`public`：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670992-32.png)

默认所有的服务和配置都属于这个`namespace`，当然我们也可以自己创建新的`namespace`：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670992-33.png)

然后填写表单：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670992-34.png)

添加完成后，可以在页面看到我们新建的`namespace`，并且Nacos为我们自动生成了一个命名空间id：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670992-35.png)

我们切换到配置列表页，你会发现`dev`这个命名空间下没有任何配置：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670992-36.png)

因为之前我们添加的所有配置都在`public`下：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670992-37.png)

#### 微服务配置namespace

默认情况下，所有的微服务注册发现、配置管理都是走`public`这个命名空间。如果要指定命名空间则需要修改`application.yml`文件。

比如，我们修改`item-service`服务的bootstrap.yml文件，添加服务发现配置，指定其`namespace`：

```YAML
spring:
  application:
    name: item-service # 服务名称
  profiles:
    active: dev
  cloud:
    nacos:
      server-addr: 192.168.11.128 # nacos地址
      config:
      	namespace: ... # 设置namespace, 必须用id
      	file-extension: yaml # 配置管理
      	shared-configs: 
      		- dataId: ...
      		...
      discovery: # 服务发现配置
        namespace: 8c468c63-b650-48da-a632-311c75e6d235 # 设置namespace，必须用id
      # 。。。略
```

启动`item-service`，查看服务列表，会发现`item-service`出现在`dev`下：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670993-38.png)

而其它服务则出现在`public`下：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670993-39.png)

此时访问`http://localhost:8082/doc.html`，基于`swagger`做测试：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670993-40.png)

会发现查询结果中缺少商品的最新价格信息。

我们查看服务运行日志：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670993-41.png)

会发现`cart-service`服务在远程调用`item-service`时，并没有找到可用的实例。这证明不同namespace之间确实是相互隔离的，不可访问。

当我们把`namespace`切换回`public`，或者统一都是以`dev`时访问恢复正常。

### 分级模型

在一些大型应用中，同一个服务可以部署很多实例。而这些实例可能分布在全国各地的不同机房。由于存在地域差异，网络传输的速度会有很大不同，因此在做服务治理时需要区分不同机房的实例。

例如item-service，我们可以部署3个实例：

- 127.0.0.1:8081
- 127.0.0.1:8082
- 127.0.0.1:8083

假如这些实例分布在不同机房，例如：

- 127.0.0.1:8081，在上海机房
- 127.0.0.1:8082，在上海机房
- 127.0.0.1:8083，在杭州机房

Nacos中提供了集群（`cluster`）的概念，来对应不同机房。也就是说，一个服务（`service`）下可以有很多集群（`cluster`），而一个集群（`cluster`）中下又可以包含很多实例（`instance`）。

如图：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670993-42.png)

因此，结合我们上一节学习的`namespace`命名空间的知识，任何一个微服务的实例在注册到Nacos时，都会生成以下几个信息，用来确认当前实例的身份，从外到内依次是：

- `namespace`：命名空间
- `group`：分组
- `service`：服务名
- `cluster`：集群
- `instance`：实例，包含ip和端口

这就是nacos中的服务分级模型。

在Nacos内部会有一个服务实例的注册表，是基于Map实现的，其结构与分级模型的对应关系如下：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670993-43.png)

查看nacos控制台，会发现默认情况下所有服务的集群都是default：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670993-44.png)

如果我们要修改服务所在集群，只需要修改`bootstrap.yml`即可：

```YAML
spring:
  cloud:
    nacos:
      discovery:
        cluster-name: BJ # 集群名称，自定义
```

我们修改`item-service`的`bootstrap.yml`，然后重新创建一个实例：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670993-45.png)

再次查看nacos：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312670993-46.png)

发现8084这个新的实例确实属于`BJ`这个集群了。

### Eureka

Eureka是Netflix公司开源的一个服务注册中心组件，早期版本的SpringCloud都是使用Eureka作为注册中心。由于Eureka和Nacos的starter中提供的功能都是基于SpringCloudCommon规范，因此两者使用起来差别不大。

课前资料中提供了一个Eureka的demo：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312689124-79.png)

我们可以用idea打开查看一下：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312689125-80.png)

结构说明：

- `eureka-server`：Eureka的服务端，也就是注册中心。没错，Eureka服务端要自己创建项目
- `order-service`：订单服务，是一个服务调用者，查询订单的时候要查询用户
- `user-service`：用户服务，是一个服务提供者，对外暴露查询用户的接口

启动以后，访问`localhost:10086`即可查看到Eureka的控制台，相对于Nacos来说简陋了很多：

![img](https://gitee.com/w-d1107/typora-image/raw/master/1722312689125-81.png)

微服务引入Eureka的方式也极其简单，分两步：

- 引入`eureka-client`依赖
- 配置`eureka`地址

接下来就是编写OpenFeign的客户端了，怎么样？是不是跟Nacos用起来基本一致。

### Eureka和Nacos对比

Eureka和Nacos都能起到注册中心的作用，用法基本类似。但还是有一些区别的，例如：

> - Nacos支持配置管理，而Eureka则不支持。

而且服务注册发现上也有区别，我们来做一个实验：

我们停止`user-service`服务，然后观察Eureka控制台，你会发现很长一段时间过去后，Eureka服务依然没有察觉`user-service`的异常状态。

这与Eureka的健康检测机制有关。在Eureka中，健康检测的原理如下：

> - 微服务启动时注册信息到Eureka，这点与Nacos一致。
> - 微服务每隔30秒向Eureka发送心跳请求，报告自己的健康状态。Nacos中默认是5秒一次。
> - Eureka如果90秒未收到心跳，则认为服务疑似故障，可能被剔除。Nacos中则是15秒超时，30秒剔除。
> - Eureka如果发现超过85%比例的服务都心跳异常，会认为是自己的网络异常，暂停剔除服务的功能。
> - Eureka每隔60秒执行一次服务检测和清理任务；Nacos是每隔5秒执行一次。

综上，你会发现Eureka是尽量不剔除服务，避免“误杀”，宁可放过一千，也不错杀一个。这就导致当服务真的出现故障时，迟迟不会被剔除，给服务的调用者带来困扰。

不仅如此，当Eureka发现服务宕机并从服务列表中剔除以后，并不会将服务列表的变更消息推送给所有微服务。而是等待微服务自己来拉取时发现服务列表的变化。而微服务每隔30秒才会去Eureka更新一次服务列表，进一步推迟了服务宕机时被发现的时间。

而Nacos中微服务除了自己定时去Nacos中拉取服务列表以外，Nacos还会在服务列表变更时主动推送最新的服务列表给所有的订阅者。

**综上，Eureka和Nacos的相似点有**：

> - 都支持服务注册发现功能
> - 都有基于心跳的健康监测功能
> - 都支持集群，集群间数据同步默认是AP模式，即最全高可用性

**Eureka和Nacos的区别有：**

> - Eureka的心跳是30秒一次，Nacos则是5秒一次
> - Eureka如果90秒未收到心跳，则认为服务疑似故障，可能被剔除。Nacos中则是15秒超时，30秒剔除。
> - Eureka每隔60秒执行一次服务检测和清理任务；Nacos是每隔5秒执行一次。
> - Eureka只能等微服务自己每隔30秒更新一次服务列表；Nacos即有定时更新，也有在服务变更时的广播推送
> - Eureka仅有注册中心功能，而Nacos同时支持注册中心、配置管理
> - Eureka和Nacos都支持集群，而且默认都是AP模式, Nacos支持CP
> - Nacos可以通过配置使服务成为永久实例, 从而开启Nacos主动监测功能, 但是永久实例宕机后不会被剔除, 所以一般不使用
> - **Eureka圣母, Nacos杀伐果断**

### 面试题

> - **面试官: 服务注册发现的基本流程是怎样的？**
>   - 服务启动时就会注册自己的服务信息（服务名、IP、端口）到注册中心
>   - 调用者可以从注册中心订阅想要的服务，获取服务对应的实例列表（1个服务可能多实例部署）
>
> - **面试官: Eureka和Nacos有哪些区别？**
>   1. Nacos支持服务端主动检测提供者状态: 临时实例采用心跳模式,非临时实例采用主动检测模式
>   2. 临时实例心跳不正常会被剔除,非临时实例则不会被剔除
>
>   3. Nacos支持服务列表变更的消息推送模式,服务列表更新更及时
>
>   4. Nacos集群默认采用`AP`方式,但也支持`CP`(存在非临时实例时);Eureka采用`AP`方式
>
>   5. Nacos支持配置中心, Eureka只有注册中心
>
> - **面试官: Nacos的分级存储模型是什么意思？**
>   -  任何一个微服务的实例在注册到Nacos时，都会生成以下几个信息，用来确认当前实例的身份，从外到内依次是：
>
>      - `namespace`：命名空间
>
>      - `group`：分组
>
>      - `service`：服务名
>
>      - `cluster`：集群
>
>      - `instance`：实例，包含ip和端口

## 负载均衡

- 负载均衡Ribbon, 发起远程调用Feign就会使用Ribbon
- Ribbon负载均衡策略有哪些?
- 如果想自定义负载均衡策略如何实现?

### Ribbon负载均衡流程

![image-20240908104813254](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908104813254.png)

### 负载均衡策略

- **`RoundRobinRule`: 简单轮询服务列表来选择服务器**
- **`WeightedResponseTimeRule`: 按照权重来选择服务器, 响应时间越长, 权重越小**
- **`RandomRule`: 随机选择一个可用的服务器**
- `BestAvailableRule`: 忽略哪些短路的服务器, 并选择<font color='red'>并发数较低</font>的服务器
- `RetryRule`: 重试机制的选择逻辑(还是轮询方式选择服务, 但是如果服务宕机或失效, 那么它就会按照指定的时间不断地重试去获取服务)
- `AvailabilityFilteringRule`: 可用性敏感策略, 先过滤非健康的, 再选择连接数较小的实例
- **`ZoneAvoidanceRule`(默认): 以区域可用的服务器为基础进行服务器的选择. 使用`Zone`对服务器进行分类, 这个`Zone`可以理解为一个机房、一个机架等. 而后再对`Zone`内的多个服务做轮询**
  - 解释: 当前服务可能会有很多, 那么机房可能也有很多, 如果北京上海等地, 假设调用方也在北京, 那么会优先选择北京的机房的服务器进行连接(就近原则). 如果没有机房这个概念, 则还是使用轮询策略

### 自定义负载均衡策略

<font color='red'>可以自己创建类实现`IRule`接口, 然后再通过配置类或者配置文件配置即可, 通过定义`IRule`实现可以修改负载均衡规则, 有两种方式:</font>

![image-20240908110952553](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908110952553.png)

### 面试官: 你们项目负载均衡如何实现的?

微服务的负载均衡主要使用了一个组件Ribbon, 比如, 我们在使用Feign远程调用的过程中, 底层的负载均衡就是使用了Ribbon

### 面试官: Ribbon的负载均衡策略有哪些? 

- `RoundRobinRule`: 简单轮询服务列表来选择服务器
- `WeightedResponseTimeRule`: 按照权重来选择服务器, 响应时间越长, 权重越小
- `RandomRule`: 随机选择一个可用的服务器
- `ZoneAvoidanceRule`: 区域敏感策略, 以区域可用的服务器为基础进行服务器的选择. 使用`Zone`对服务器进行分类, 这个`Zone`可以理解为一个机房、一个机架等. 而后再对`Zone`内的多个服务做轮询(默认)

### 面试官: 如果想自定义负载均衡策略如何实现?

提供了两种方式:

1. 创建类实现`IRule`接口, 可以指定负载均衡策略(全局)
2. 在客户端的配置文件中, 可以配置某一个服务调用的负载均衡策略(局部)

## 服务雪崩、熔断降级

- 什么是服务雪崩?
- 熔断降级(解决)
- 限流(预防)

![image-20240908111949802](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908111949802.png)

### 服务降级

服务降级是服务自我保护的一种方式, 或者保护下游服务的一种方式, 用于确保服务不会受请求突增影响变得不可用, 确保服务不会崩溃

![image-20240908112300289](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908112300289.png)

![image-20240908112318580](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908112318580.png)

如果降级太多, 则会触发<font color='red'>熔断机制</font>.

### 服务熔断

Hystrix熔断机制, 用于监测微服务调用情况, 默认是关闭的, 如果需要开启需要在引导类上添加注解: `@EnableCircuitBreaker`. 如果检测到<font color='red'>10秒内请求的失败率超过50%</font>, 就触发熔断机制. 之后<font color='red'>每隔5秒</font>重新尝试请求微服务, 如果微服务不能响应, 继续走熔断机制. 如果微服务可达, 则关闭熔断机制, 恢复正常请求

![image-20240908112636609](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908112636609.png)

### 面试官: 什么是服务雪崩, 怎么解决这个问题? 

- 服务雪崩: 一个服务失败, 导致整条链路的服务都失败的情形
- 服务降级: 服务自我保护的一种方式, 或者保护下游服务的一种方式, 用于确保服务不会受请求突增影响变得不可用, 确保服务不会崩溃, <font color='red'>一般在实际开发中与Feign接口整合, 编写降级逻辑</font>
- 服务熔断: 默认关闭, 需要手动打开, 如果检测到<font color='red'>10秒内请求的失败率超过50%</font>, 就触发熔断机制. 之后<font color='red'>每隔5秒重新尝试请求</font>微服务, 如果微服务不能响应, 继续走熔断机制. 如果微服务可达, 则关闭熔断机制, 恢复正常请求

## 微服务的监控

**为什么需要监控?**

![image-20240908121319593](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908121319593.png)

- **问题定位**

  假如:

  ![image-20240908121529330](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908121529330.png)

  PC端发出请求, 请求经过网关->服务A–>服务H–>服务K–>MySQL

  此时服务K宕机了或出现了问题, 该如何快速定位到出问题的微服务呢?

- **性能分析**

  还是上个请求链路, 在这个请求链路中, 只要有一个服务响应慢就会导致整条链路的时间变久. 比如还是服务K慢, 该如何快速定位到慢的接口呢

- **服务关系**

  当微服务变多了之后, 再依靠人工处理维护服务之间的关系显然是不实际的

- **服务告警**

  假设还是服务K出了问题, 我们能否及时的知道服务K出现问题呢

**常见的服务监控工具**:

- SpringBoot-admin 比较简单, 功能单一, 一般不采用
- prometheus + Grafana 企业中常用, 但是搭建复杂
- zipkin 代码耦合
- skywalking 相对优秀

zipkin和Skywalking都是链路追踪工具, 也可以提供一些监控的功能

### Skywalking

一个分布式系统的应用程序性能监控工具(<font color='red'>A</font>pplication <font color='red'>P</font>erformance <font color='red'>M</font>anagment), 提供了完善的链路追踪能力, Apache的顶级项目(前华为产品经理吴晟主导开源)

![image-20240908123009855](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908123009855.png)

- 服务(Service): 业务资源应用系统(微服务)
- 端点(endpoint): 应用系统对外暴露的功能接口(接口)
- 实例(instance): 物理机

**仪表盘**

![image-20240908153948022](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908153948022.png)

我们进入**追踪**页面:

![image-20240908154550300](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908154550300.png)

同时, 这个页面还可以展示更多的信息, 比如点击MySQL的信息后:

![image-20240908154731119](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908154731119.png)

会展示当前的查询语句

我们进入**拓扑图**页面:

![image-20240908154908209](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908154908209.png)

这里会展示所有微服务之间的关系, 标红代表当前微服务不太健康

**服务告警:**

![image-20240908155210304](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908155210304.png)

**告警规则:**

在配置文件中预先定义的告警规则总结如下:

1. 在过去10分钟的3分钟内服务平均响应时间超过1秒达3次
2. 在过去10分钟内服务成功率低于80%达2次
3. 在过去10分钟内服务90%响应时间低于1秒达3次
4. 在过去10分钟内服务的响应时间超过1秒达2次
5. 在过去十分钟内端点的响应时间超过1秒达2次

告警信息不仅仅在这个页面展示, 同时可以针对性的发送邮件、短信, 包括对接企业微信或钉钉快速的通知负责人

### 面试官: 你们的微服务是怎么监控的?

我们项目中采用的Skywalking进行监控的

1. Skywalking主要可以监控接口、服务、物理实例的一些状态. 特别是在压测的时候可以看到众多服务中哪些服务和接口比较慢, 我们可以针对性的分析和优化.
2. 我们还在Skywalking设置了告警规则, 特别是在项目上线以后, 如果报错, 我们分别设置了可以给相关负责人发短信和发邮件, 第一时间知道项目的bug情况, 第一时间修复

### [项目链路追踪+Skywalking介绍安装部署使用]( http://t.csdnimg.cn/ROui2)

# 业务问题

![image-20240908162257518](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908162257518.png)

## 微服务限流

### 为什么要限流?

1. 并发的确大(突发流量)
2. 防止用户恶意刷接口

![image-20240908162428930](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908162428930.png)

### 限流的实现方式

- **Tomcat**: 可以设置最大连接数

  ![image-20240908162612205](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908162612205.png)

- **Nginx**: 漏桶算法

- **网关**: 令牌桶算法

- **自定义拦截器**

### Nginx限流

#### 控制速率(突发流量)

##### 漏桶算法原理

![image-20240908163249027](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908163249027.png)

##### 配置

![image-20240908163200029](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908163200029.png)

- 语法: `limit_req_zone key zone rate`
- `key`: 定义限流对象, `binary_remote_addr`就是一种`key`, 基于客户端ip(用户)限流
- `zone`: 定义共享存储区来存储访问信息, 10m可以存储16w个ip地址访问信息
- `rate`: 最大访问速率, `rate=10r/s` 表示每秒最多10个请求
- `burst=20`: 相当于桶的大小
- `nodelay`: 快速处理

#### 控制并发连接数

![image-20240908163852117](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908163852117.png)

- `limit_conn perip 20`: 对应的`key`是`$binary_remote_addr`, 表示限制单个ip同时最多能持有20个连接.
- `limit_conn perserver 100`: 对应的`key`是`$server_name`, 表示虚拟主机(server)同时能处理并发连接的总数.

### 网关限流

#### 令牌桶算法

![image-20240908164306547](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908164306547.png)

#### 配置

yml配置文件中, 微服务路由设置添加局部过滤器`RequestRateLimiter`

![image-20240908164222114](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908164222114.png)

- `key-resolver`: 定义限流对象(ip、路径、参数), 需代码实现, 使用`spel`表达式获取
- `replenishRate`: 令牌桶每秒填充平均速率
- `urstCapacity`: 令牌桶总容量

### 面试官: 你们项目中有没有做过限流? 怎么做的?

1. 先来介绍业务, 什么情况下去做限流, 需要说明QPS具体多少

   - 我们当时有一个活动, 到了假期就会抢购优惠券, QPS最高可以达到2000, 平均10-50之间, 为了应对突发流量, 需要做限流

   - 常规限流, 为了防止恶意攻击, 保护系统正常运行, 我们当时系统能够承受最大的QPS是**(压测结果)

2. Nginx限流

   - 控制速率(突发流量): 使用的漏桶算法来实现过滤, 让请求以固定的速率处理请求, 可以应对突发流量
   - 控制并发数, 限制单个ip的连接数喝并发连接的总数

3. 网关限流

   - 在Spring Cloud Gateway中支持局部过滤器`RequestRateLimiter`来做限流, 使用的是令牌桶算法
   - 可以根据ip或路径进行限流, 可以设置每秒平均填充速率, 和令牌桶总容量

### 面试官: 限流常见的算法有哪些?

- 漏桶算法
- 令牌桶算法

## 分布式系统理论

### CAP和BASE

- 分布式事务方案的指导
- 分布式系统设计方向
- 根据业务指导使用正确的技术选择

#### CAP定理

1998年, 加州大学的计算机科学家Eric Brewer提出, 分布式系统有三个指标:

- `Consistency`(一致性)
- `Availability`(可用性)
- `Partition tolerance`(分区容错性)

![image-20240908165751732](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908165751732.png)

##### `Consistency`

`Consistency`(一致性): 用户访问分布式系统中的任意节点, 得到的数据必须一致

![image-20240908165903876](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908165903876.png)

##### `Availability`

`Availability`(可用性): 用户访问集群中的任意健康节点, 必须能得到响应, 而不是超时或拒绝

![image-20240908170029365](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908170029365.png)

##### `Partition tolerance`

`Partition`(分区): 因为网络故障或其他原因导致分布式系统中的部分节点与其他节点失去连接, 形成独立分区.

`tolerance`(容错): 在集群出现分区时, 整个系统也要持续对外提供服务 

![image-20240908170209948](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908170209948.png)

##### 结论

- 分布式系统节点之间肯定是需要网络连接的, <font color='red'>分区(P)是必然存在的</font>
- 如果保证访问的高可用性(A), 可以持续对完提供服务, 但不能保证数据的强一致性 –> `AP`
- 如果保证访问的数据强一致性(C), 就要放弃高可用性 –> `CP`

#### BASE理论

BASE理论是对CAP理论的一种解决思路, 包含三个思想:

- **<font color='red'>B</font>asically <font color='red'>A</font>vailable (基本可用)**: 分布式系统在出现故障时, 允许损失部分可用性, 即保证核心可用.
- **<font color='red'>S</font>oft State (软状态)**: 在一定时间内, 允许出现中间状态, 比如临时的不一致状态
- **<font color='red'>E</font>ventually Consistent (最终一致性)**: 虽然无法保证强一致性, 但是在软状态结束后, 最终达到数据一致

![image-20240908171031578](https://gitee.com/w-d1107/typora-image/raw/master/image-20240908171031578.png)

#### 面试官: 解释一下CAP和BASE

- **CAP定理(一致性、可用性、分区容错性)**
  1. 分布式系统节点通过网络连接, 一定会出现分区问题(P)
  2. 当分区出现时, 系统的一致性(C)和可用性(A)就无法同时满足
- **BASE理论**
  1. 基本可用
  2. 软状态
  3. 最终一致
- **解决分布式事务的思想和模型**
  1. 最终一致思想: 各分支事务分别执行并提交, 如果有不一致的情况, 再想办法恢复数据(AP)
  2. 强一致思想: 各分支事务执行完业务不要提交, 等待彼此结果, 而后统一提交或回滚(CP)

## 分布式事务解决方案

- 简历上写的是微服务项目
- Seata框架(XA、AT、TCC)
- MQ

### Seata架构

Seata事务管理中有三个重要的角色:

- **TC(Transaction Coordinator) - 事务协调者:** 维护全局和分支事务的状态, 协调全局事务提交或回滚.
- **TM(Transaction Manager) - 事务管理器:** 定义全局事务的范围、开始全局事务、提交或回滚全局事务
- **RM(Resource Manager) - 资源管理器:** 管理分支事务处理的资源, 与TC交谈以注册分支事务和报告分支事务的状态, 并驱动分支事务提交或回滚.

![image-20240909105920288](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909105920288.png)

### Seata的XA模式(CP)

RM一阶段的工作:

1. 注册分支事务到TC
2. 执行分支业务SQL但不提交
3. 报告执行状态到TC

TC二阶段的工作:

- TC检测各分支事务执行状态
  - 如果都成功, 通知所有RM提交事务
  - 如果有失败, 通知所有RM回滚事务

RM二阶段的工作:

- 接收TC命令, 提交或回滚事务.

![image-20240909110241550](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909110241550.png)

### AT模式原理(AP)

AT模式同样是分阶段提交的事务模型, 不过却弥补了XA模型中资源锁定周期过长的缺陷.

RM一阶段的工作:

- 注册分支事务
- <font color='red'>记录`undo-log`(数据快照)</font>
- 执行业务SQL并<font color='red'>提交</font>
- 报告事务状态

RM阶段二提交时的工作

- 删除`undo-log`即可

RM阶段二回滚时的工作:

- 根据`undo-log`恢复数据到更新前

![image-20240909110652065](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909110652065.png)

### TCC模式原理

1. <font color='red'>T</font>ry: 资源的检测和预留;
2. <font color='red'>C</font>onfirm: 完成资源操作业务; 要求Try成功Confirm一定要能成功.
3. <font color='red'>C</font>ancel: 预留资源释放, 可以理解为Try的反向操作

![image-20240909111026287](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909111026287.png)

缺点: 需要用代码实现, 有耦合

### MQ分布式事务

![image-20240909111330125](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909111330125.png)

### 面试官: 你们采用哪种分布式事务解决方案?

- 简历上写的微服务, 只要是发生了多个服务之间的<font color='red'>写操作</font>, 都需要进行分布式事务控制
- 描述项目中采用的哪种方案(Seata | MQ)
  1. Seata的XA模式, CP, 需要互相等待各个分支事务提交, 可以保证强一致性, 性能差
  2. Seata的AT模式, AP, 底层使用`undo log`实现, 性能好
  3. Seata的TCC模式, AP, 性能较好, 不过需要人工编码实现
  4. MQ模式实现分布式事务, 在A服务写数据的时候, 需要在同一个事务内发送消息到另一个事务, 异步, 性能最好

## 分布式事务的接口幂等性

幂等: 多次调用方法或者接口不会改变业务状态, 可以保证重复调用的结果和单词调用的结果一致. 

需要幂等场景:

- 用户重复点击(网络波动)
- MQ消息重复
- 应用使用失败或超时重试机制

### 接口幂等

基于`RESTful API`的角度对部分常见类型请求的幂等性特点进行分析

| 请求方式 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| GET      | 查询操作，天然幂等                                           |
| POST     | 新增操作，请求一次与请求多次造成的结果不同，<font color='red'>不是幂等的</font> |
| PUT      | 更新操作，如果是以绝对值更新，则是幂等的。如果是通过增量的方式更新，则<font color='red'>不是幂等的</font> |
| DELETE   | 删除操作，根据唯一值删除，是幂等的                           |

```MySQL
update t_item set money = 500 where id = 1; 幂等
update t_item set money = money + 500 where id = 1; 不是幂等
```

解决幂等性方案:

- 数据库唯一索引: 解决新增问题
- token + redis: 解决新增和修改问题
- 分布式锁: 解决新增和修改问题

### token + redis

创建商品、提交订单、转账、支付等操作

![image-20240909112924138](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909112924138.png)

![image-20240909113049164](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909113049164.png)

### 分布式锁

![image-20240909113201015](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909113201015.png)

- 快速失败(抢不到锁的线程)
- 控制锁的粒度

### 面试官: 分布式事务的接口幂等性如何设计?

- 幂等: 多次调用方法或者接口不会改变业务状态, 可以<font color='red'>保证重复调用的结果和单词调用的结果一致</font>
- 如果是新增数据, 可以使用数据库的唯一索引
- 如果是新增或修改数据
  - 分布式锁, 性能较低
  - 使用token + redis来实现, 性能较好
    - 第一次请求, 生成一个唯一token存入redis, 返回给前端
    - 第二次请求, 业务处理, 携带之前的token, 到redis进行验证, 如果存在, 可以执行业务, 删除token; 如果不存在, 在直接返回, 不处理业务

## 分布式任务调度

首先, 还是要描述当时是什么场景用了任务调度

**xxl-job解决的问题**

- 解决集群任务的重复执行问题
- `cron`表达式定义灵活
- 定时任务失败了, 重试和统计
- 任务量大, 分片执行

常见问题:

1. xxl-job路由策略有哪些?
2. xxl-job任务执行失败怎么解决?
3. 如果有大数据量的任务同时都需要执行, 怎么解决?

### xxl-job路由策略有哪些?

1. `FIRST`(第一个): 固定选择第一台机器;
2. `LAST`(最后一个): 固定选择最后一台机器;
3. <font color='red'>**`ROUND`(轮询);**</font>
4. `RANDOM`(随机): 随机选择在线的机器;
5. `CONSISTENT_HASH`(一致性HASH): 每个任务按照HASH算法固定选择某一台机器, 且所有任务均匀散列在不同机器上;
6. `LEAST_FREQUENTLY_USED`(最不经常使用): 使用频率最低的机器优先被选举;
7. `LEAST_RECENTLY_USED`(最近最久未使用): 最久未使用的机器优先被选举;
8. <font color='red'>**`FAILOVER`(故障转移): 按照顺序依次进行心跳检测, 第一个心跳检测成功的机器选定为目标执行器并发起调度;**</font>
9. `BUSYOVER`(忙碌转移): 按照顺序依次进行空闲检测, 第一个空闲检测成功的机器选定为目标执行器并发起调度;
10. <font color='red'>**`SHARDING_BROADCAST`(分片广播): 广播触发对应集群中所有机器执行一次任务, 同时系统自动传递分片参数; 可根据分片参数开发分片任务**</font>

### xxl-job任务执行失败怎么解决?

故障转移 + 失败重试, 查看日志分析 —> 邮件告警

日志:

![image-20240909114855515](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909114855515.png)

邮件告警:

![](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909114926520.png)

需要在代码配置文件中设置开启邮件告警:

![image-20240909115109059](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909115109059.png)

### 如果有大数据量的任务同时都需要执行, 怎么解决?

执行器集群部署时, 任务路由策略选择<font color='red'>分片广播</font>情况下, <font color='red'>一次任务</font>调度将会广播触发对应集群中所有执行器执行一次任务

![image-20240909115310547](https://gitee.com/w-d1107/typora-image/raw/master/image-20240909115310547.png)

分片参数:

- `index`: 当前分片序号(从0开始), 执行器集群列表中当前执行器的序号;
- `total`: 总分片数, 执行器集群的总机器数量;

```java
@XxlJob("shadingSample")
public void shardingJobHandler() throws Exception {
    // 分片参数
    int shardIndex = XxlJobHelper.getShardIndex();
    int shardTotal = XxlJobHelper.getShardTotal();
    XxlJobHelper.log("分片参数：当前分片序号 = {}, 总分片数 = {}", shardIndex, shardTotal);
    // 业务逻辑
    List<Integer> list = getList();
    for (Integer integer : list) {
        if(integer % shardTotal == shardIndex){
            System.out.println("第"+shardIndex+"分片执行，执行数据为："+integer);
        }
    }
}
```

### 面试官: xxl-job路由策略有哪些?

xxl-job提供了很多的路由策略, 我们平时用的较多就是: 轮询、故障转移、分片广播.

### 面试官: xxl-job任务执行失败怎么解决?

- 路由策略选择故障转移, 使用健康的实力来执行任务
- 设置重试次数
- 查看日志 + 邮件告警来通知相关负责人解决

### 面试官: 如果有大数据量的任务同时都需要执行, 怎么解决?

- 让多个实例一块去执行(部署集群), 路由策略<font color='red'>分片广播</font>
- 在任务执行的代码中可以获取分片总数和当前分片, 按照取模的方式分摊到各个实例执行

# 微服务面试题

>**面试官：**Spring Cloud 5大组件有哪些？
>
>**候选人：**
>
>早期我们一般认为的Spring Cloud五大组件是 
>
>- Eureka   : 注册中心
>- Ribbon  : 负载均衡
>- Feign     : 远程调用
>- Hystrix :  服务熔断
>- Zuul/Gateway  : 网关
>
>随着SpringCloudAlibba在国内兴起 , 我们项目中使用了一些阿里巴巴的组件 
>
>- 注册中心/配置中心 Nacos
>
>- 负载均衡 Ribbon
>
>- 服务调用 Feign
>
>- 服务保护 sentinel
>
>- 服务网关 Gateway
>
>**面试官：**服务注册和发现是什么意思？Spring Cloud 如何实现服务注册发现？
>
>**候选人：**
>
>我理解的是主要三块大功能，分别是服务注册 、服务发现、服务状态监控
>
>我们当时项目采用的eureka作为注册中心，这个也是spring cloud体系中的一个核心组件
>
>**服务注册**：服务提供者需要把自己的信息注册到eureka，由eureka来保存这些信息，比如服务名称、ip、端口等等
>
>**服务发现**：消费者向eureka拉取服务列表信息，如果服务提供者有集群，则消费者会利用负载均衡算法，选择一个发起调用
>
>**服务监控**：服务提供者会每隔30秒向eureka发送心跳，报告健康状态，如果eureka服务90秒没接收到心跳，从eureka中剔除
>
>**面试官：**我看你之前也用过nacos、你能说下nacos与eureka的区别？
>
>**候选人：**
>
>我们当时xx项目就是采用的nacos作为注册中心，选择nacos还要一个重要原因就是它支持配置中心，不过nacos作为注册中心，也比eureka要方便好用一些，主要相同不同点在于几点：
>
>- 共同点
>
>Nacos与eureka都支持服务注册和服务拉取，都支持服务提供者心跳方式做健康检测
>
>- Nacos与Eureka的区别
>
>①Nacos支持服务端主动检测提供者状态：临时实例采用心跳模式，非临时实例采用主动检测模式
>
>②临时实例心跳不正常会被剔除，非临时实例则不会被剔除
>
>③Nacos支持服务列表变更的消息推送模式，服务列表更新更及时
>
>④Nacos集群默认采用AP方式，当集群中存在非临时实例时，采用CP模式；Eureka采用AP方式
>
>**面试官：**你们项目负载均衡如何实现的 ? 
>
>**候选人：**
>
>是这样~~
>
>在服务调用过程中的负载均衡一般使用SpringCloud的Ribbon 组件实现  , Feign的底层已经自动集成了Ribbon  , 使用起来非常简单
>
>当发起远程调用时，ribbon先从注册中心拉取服务地址列表，然后按照一定的路由策略选择一个发起远程调用，一般的调用策略是轮询
>
>**面试官：**Ribbon负载均衡策略有哪些 ? 
>
>**候选人：**
>
>我想想啊，有很多种，我记得几个：
>
>- RoundRobinRule：简单轮询服务列表来选择服务器
>
>- WeightedResponseTimeRule：按照权重来选择服务器，响应时间越长，权重越小
>
>- RandomRule：随机选择一个可用的服务器
>
>- ZoneAvoidanceRule：区域敏感策略，以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房、一个机架等。而后再对Zone内的多个服务做轮询(默认)
>
>**面试官：**如果想自定义负载均衡策略如何实现 ? 
>
>**候选人：**
>
>提供了两种方式：
>
>1，创建类实现IRule接口，可以指定负载均衡策略，这个是全局的，对所有的远程调用都起作用
>
>2，在客户端的配置文件中，可以配置某一个服务调用的负载均衡策略，只是对配置的这个服务生效远程调用
>
>**面试官：**什么是服务雪崩，怎么解决这个问题？
>
>**候选人：**
>
>
>
>服务雪崩是指一个服务失败，导致整条链路的服务都失败的情形，一般我们在项目解决的话就是两种方案，第一个是服务降级，第二个是服务熔断，如果流量太大的话，可以考虑限流
>
>服务降级：服务自我保护的一种方式，或者保护下游服务的一种方式，用于确保服务不会受请求突增影响变得不可用，确保服务不会崩溃，一般在实际开发中与feign接口整合，编写降级逻辑
>
>服务熔断：默认关闭，需要手动打开，如果检测到 10 秒内请求的失败率超过 50%，就触发熔断机制。之后每隔 5 秒重新尝试请求微服务，如果微服务不能响应，继续走熔断机制。如果微服务可达，则关闭熔断机制，恢复正常请求
>
>**面试官：**你们的微服务是怎么监控的？
>
>**候选人：**
>
>我们项目中采用的skywalking进行监控的
>
>1，skywalking主要可以监控接口、服务、物理实例的一些状态。特别是在压测的时候可以看到众多服务中哪些服务和接口比较慢，我们可以针对性的分析和优化。
>
>2，我们还在skywalking设置了告警规则，特别是在项目上线以后，如果报错，我们分别设置了可以给相关负责人发短信和发邮件，第一时间知道项目的bug情况，第一时间修复
>
>**面试官：**你们项目中有没有做过限流 ? 怎么做的 ?
>
>**候选人：**
>
>我当时做的xx项目，采用就是微服务的架构，因为xx因为，应该会有突发流量，最大QPS可以达到2000，但是服务支撑不住，我们项目都通过压测最多可以支撑1200QPS。因为我们平时的QPS也就不到100，为了解决这些突发流量，所以采用了限流。
>
>【版本1】
>
>我们当时采用的nginx限流操作，nginx使用的漏桶算法来实现过滤，让请求以固定的速率处理请求，可以应对突发流量，我们控制的速率是按照ip进行限流，限制的流量是每秒20
>
>【版本2】
>
>我们当时采用的是spring cloud gateway中支持局部过滤器RequestRateLimiter来做限流，使用的是令牌桶算法，可以根据ip或路径进行限流，可以设置每秒填充平均速率，和令牌桶总容量
>
>**面试官：**限流常见的算法有哪些呢？
>
>**候选人：**
>
>比较常见的限流算法有漏桶算法和令牌桶算法
>
>漏桶算法是把请求存入到桶中，以固定速率从桶中流出，可以让我们的服务做到绝对的平均，起到很好的限流效果
>
>令牌桶算法在桶中存储的是令牌，按照一定的速率生成令牌，每个请求都要先申请令牌，申请到令牌以后才能正常请求，也可以起到很好的限流作用
>
>它们的区别是，漏桶和令牌桶都可以处理突发流量，其中漏桶可以做到绝对的平滑，令牌桶有可能会产生突发大量请求的情况，一般nginx限流采用的漏桶，spring cloud gateway中可以支持令牌桶算法
>
>**面试官**：什么是CAP理论？
>
>**候选人**：
>
>CAP主要是在分布式项目下的一个理论。包含了三项，一致性、可用性、分区容错性
>
>- 一致性(Consistency)是指更新操作成功并返回客户端完成后，所有节点在同一时间的数据完全一致(强一致性)，不能存在中间状态。
>
>- 可用性(Availability) 是指系统提供的服务必须一直处于可用的状态，对于用户的每一个操作请求总是能够在有限的时间内返回结果。
>
>- 分区容错性(Partition tolerance) 是指分布式系统在遇到任何网络分区故障时，仍然需要能够保证对外提供满足一致性和可用性的服务，除非是整个网络环境都发生了故障。
>
>**面试官**：为什么分布式系统中无法同时保证一致性和可用性？
>
>**候选人**：
>
>嗯，是这样的~~
>
>首先一个前提，对于分布式系统而言，分区容错性是一个最基本的要求，因此基本上我们在设计分布式系统的时候只能从一致性（C）和可用性（A）之间进行取舍。
>
>如果保证了一致性（C）：对于节点N1和N2，当往N1里写数据时，N2上的操作必须被暂停，只有当N1同步数据到N2时才能对N2进行读写请求，在N2被暂停操作期间客户端提交的请求会收到失败或超时。显然，这与可用性是相悖的。
>
>如果保证了可用性（A）：那就不能暂停N2的读写操作，但同时N1在写数据的话，这就违背了一致性的要求。
>
>**面试官**：什么是BASE理论？
>
>**候选人**：
>
>嗯，这个也是CAP分布式系统设计理论
>
>BASE是CAP理论中AP方案的延伸，核心思想是即使无法做到强一致性（StrongConsistency，CAP的一致性就是强一致性），但应用可以采用适合的方式达到最终一致性（Eventual Consitency）。它的思想包含三方面：
>
>1、Basically Available（基本可用）：基本可用是指分布式系统在出现不可预知的故障的时候，允许损失部分可用性，但不等于系统不可用。
>
>2、Soft state（软状态）：即是指允许系统中的数据存在中间状态，并认为该中间状态的存在不会影响系统的整体可用性，即允许系统在不同节点的数据副本之间进行数据同步的过程存在延时。
>
>3、Eventually consistent（最终一致性）：强调系统中所有的数据副本，在经过一段时间的同步后，最终能够达到一个一致的状态。其本质是需要系统保证最终数据能够达到一致，而不需要实时保证系统数据的强一致性。
>
>**面试官：**你们采用哪种分布式事务解决方案？
>
>**候选人：**
>
>我们当时是xx项目，主要使用到的seata的at模式解决的分布式事务
>
>seata的AT模型分为两个阶段：
>
>1、阶段一RM的工作：① 注册分支事务  ② 记录undo-log（数据快照）③ 执行业务sql并提交 ④报告事务状态
>
>2、阶段二提交时RM的工作：删除undo-log即可
>
>3、阶段二回滚时RM的工作：根据undo-log恢复数据到更新前
>
>at模式牺牲了一致性，保证了可用性，不过，它保证的是最终一致性
>
>**面试官：**分布式服务的接口幂等性如何设计？
>
>**候选人：**
>
>嗯，我们当时有一个xx项目的下单操作，采用的token+redis实现的，流程是这样的
>
>第一次请求，也就是用户打开了商品详情页面，我们会发起一个请求，在后台生成一个唯一token存入redis，key就是用户的id，value就是这个token，同时把这个token返回前端
>
>第二次请求，当用户点击了下单操作会后，会携带之前的token，后台先到redis进行验证，如果存在token，可以执行业务，同时删除token；如果不存在，则直接返回，不处理业务，就保证了同一个token只处理一次业务，就保证了幂等性
>
>**面试官：**xxl-job路由策略有哪些？
>
>**候选人：**
>
>xxl-job提供了很多的路由策略，我们平时用的较多就是：轮询、故障转移、分片广播…
>
>**面试官：**xxl-job任务执行失败怎么解决？
>
>**候选人：**
>
>有这么几个操作
>
>第一：路由策略选择故障转移，优先使用健康的实例来执行任务
>
>第二，如果还有失败的，我们在创建任务时，可以设置重试次数
>
>第三，如果还有失败的，就可以查看日志或者配置邮件告警来通知相关负责人解决
>
>**面试官：**如果有大数据量的任务同时都需要执行，怎么解决？
>
>**候选人：**
>
>我们会让部署多个实例，共同去执行这些批量的任务，其中任务的路由策略是分片广播
>
>在任务执行的代码中可以获取分片总数和当前分片，按照取模的方式分摊到各个实例执行就可以了
