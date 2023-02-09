### Eureka
是类似于Dubbo的能力，Eureka是提供给rest的服务发现、负载均衡的中间件。
通过新建一个或多个服务构成注册中心Eureka Server,后续服务生产者在注册中心中注册接口，
服务消费者从注册中心中获取服务生产者的主机、port、path等信息，通过远程调用（http请求而非rpc）进行接口调用。

和dubbo的区别
	1. dubbo通过zookeeper或者nacos进行注册中心的构建，Eureka更注重搞可用和分区容灾新，所有注册中心服务都是对等的，只要注册中心集群有一个Eureka服务是可用的，则注册中心是可用的，不注重分区的一致性。而zookeeper的集群是不对等的，存在leader和follower，需要leader和一半的follower可用，整个注册中心才是可用的。
	2. dubbo可以通过netty等做远程过程调用，不局限与http请求，可以是tcp、udp等请求，由应用自己构建。
	3. dubbo可以自己实现filter和负载均衡等功能，Eureka负责把所有的服务提供方信息反馈给服务消费者，然后由服务消费者自身来做负载均衡等路由选择。
![image](https://user-images.githubusercontent.com/31442134/217707634-be9e9cd8-4b4b-4d11-b2b8-6dbc2cde74c4.png)
