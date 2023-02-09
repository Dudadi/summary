# Spring cloud config
https://www.springcloud.cc/spring-cloud-config.html

Spring cloud config 功能提供分布式配置功能
根据环境和变量名获取对应的分布式系统的配置属性；
以git为例，设置公共的配置代码仓，然后在微服务中设置代码仓的git地址以及登录的用户名和密码（需要加密），然后微服务在读取配置项时，从git代码仓中读取。实现分布式配置

其余功能： 加解密、登录、ssh访问配置文件服务器（eg: git）、监听配置属性变化
