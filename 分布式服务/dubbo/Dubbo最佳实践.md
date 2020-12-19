## 开发分包

 建议将`服务接口`、`服务模型`、`服务异常`等均放在 API 包中，因为服务模型和异常也是 API 的一部分，这样做也符合分包原则：重用发布等价原则(REP)，共同重用原则 (CRP)。 

服务接口尽可能大粒度，每个服务方法应代表一个功能，而不是某功能的一个步骤，否 则将面临分布式事务问题，Dubbo 暂未提供分布式事务支持。

服务接口建议以业务场景为单位划分，并对相近业务做抽象，防止接口数量爆炸。 

不建议使用过于抽象的通用接口，如：Map query(Map)，这样的接口没有明确语义， 会给后期维护带来不便

## 环境隔离与分组 

怎么做多环境的隔离？ 

1、部署多套？ 

2、多注册中心机制 

3、group机制 

4、版本机制**

服务接口增加方法，或服务模型增加字段，可向后兼容，删除方法或删除字段，将不兼 容，枚举类型新增字段也不兼容，需通过变更版本号升级

## 参数配置

 通用参数以 consumer 端为准，如果consumer端没有设置，使用provider数值 建议在 Provider 端配置的 Consumer 端属性有： timeout：方法调用的超时时间 retries：失败重试次数，缺省是 2 2 loadbalance：负载均衡算法 3，缺省是随机 random。 actives：消费者端的最大并发调用限制，即当 Consumer 对一个服务的并发调用到上限后，新 调用会阻塞直到超时，可以配置在方法或服务上。 建议在 Provider 端配置的 Provider 端属性有： threads：服务线程池大小 executes：一个服务提供者并行执行请求上限，即当 Provider 对一个服务的并发调用达到上限 后，新调用会阻塞，此时 Consumer 可能会超时。可以配置在方法或服务上。

## 容器化部署 

注册的IP问题，容器内提供者使用的IP，如果注册到zk，消费者无法访问。 两个解决办法： 1、docker使用宿主机网络 docker xxx -net xxxxx 2、docker参数指定注册的IP和端口, -e DUBBO_IP_TO_REGISTRY — 注册到注册中心的IP地址 DUBBO_PORT_TO_REGISTRY — 注册到注册中心的端口 DUBBO_IP_TO_BIND — 监听IP地址 DUBBO_PORT_TO_BIND — 监听端口

## 运维与监控 

Admin功能较简单，大规模使用需要定制开发，整合自己公司的运维监控系统。 可观测性：tracing、metrics、logging(ELK) - APM(skywalking,pinpoint,cat,,,,) - Promethus+Grafana

## 分布式事务 

柔性事务，SAGA、TCC、AT - Seata - hmily + dubbo 不支持 XA

## 重试与幂等

服务调用失败默认重试2次，如果接口不是幂等的，会造成业务重复处理。 

如何设计幂等接口？ 

1、去重-->(bitmap --> 16M), 100w/s

2、类似乐观锁机制,前一个版本号或时间戳

