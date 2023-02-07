# `dubbo`是一个高性能的`java rpc`框架。如下图：存在5个部件
+ `Registry`： 注册中心一般是`zookeeper`或者`nacos`，用于存储服务提供者和服务消费者的信息；

+ `Provider`: 服务提供者，在`Container`中启动，启动后会把自身暴露的接口信息以及该主机的ip地址等访问信息上传给`register`注册中心；
+ `Consumer`： 服务消费者，会订阅注册中心的接口信息，注册中心相应接口变更后会通知消费者，消费者获取到服务提供者的地址然后根据地址通过rpc调用接口；
+ `Monitor`： 服务监控，对服务调用以及服务提供者进行监控；
+ `Container`: 拉起`Provider`服务的容器，一般是`jetty`或者`spring`容器；

总体而言dubbo做的事情就是通过注册中心维护服务生产者和服务消费者的信息，消费者调用信息时把服务生产者信息发送给消费者（消费者会对服务生产者信息进行本地缓存）。
然后根据负载均衡策略选择一个可行的生产者地址进行访问，该远程访问流程可以是通过http也可以是通过tcp、udp等传输协议。
服务生产者接收到信息后，通过反射或者代理执行一开始注入的服务，执行函数代码后把结果返回给生产者。中间可以通过filter进行请求的中间层处理，通过netty进行远程通信。

https://github.com/apache/dubbo

https://cn.dubbo.apache.org/zh/docs3-v2/java-sdk/concepts-and-architecture/overall-architecture/
