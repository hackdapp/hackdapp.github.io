title: SpringCloud组件服务简介
categories: SpringCloud
tags:
  - hackhook
keywords:
  - hackhook
  - SpringCloud
  - 组件服务
  - spring
date: 2017-09-13 16:46:07
---
## Spring Cloud Config  
> Centralized external configuration management backed by a git repository. The configuration resources map directly to Spring `Environment` but could be used by non-Spring applications if desired.

## Spring Cloud Bus  
> An event bus for linking services and service instances together with distributed messaging. Useful for propagating state changes across a cluster (e.g. config change events).

## Spring Cloud Netflix  
> Integration with various Netflix OSS components (Eureka, Hystrix, Zuul, Archaius, etc.).

针对多种Netflix组件提供的开发工具包，其中包括Eureka、Hystrix、Zuul、Archaius等。
- Netflix Eureka：云端负载均衡，一个基于 REST 的服务，用于定位服务，以实现云端的负载均衡和中间层服务器的故障转移。
- Netflix Hystrix：容错管理工具，旨在通过控制服务和第三方库的节点,从而对延迟和故障提供更强大的容错能力。
- Netflix Zuul：边缘服务工具，是提供动态路由，监控，弹性，安全等的边缘服务。
- Netflix Archaius：配置管理API，包含一系列配置管理API，提供动态类型化属性、线程安全配置操作、轮询框架、回调机制等功能。

## Spring Cloud for Cloud Foundry：通过Oauth2协议绑定服务到CloudFoundry，CloudFoundry是VMware推出的开源PaaS云平台。
## Spring Cloud Sleuth：日志收集工具包，封装了Dapper,Zipkin和HTrace操作。
## Spring Cloud Data Flow：大数据操作工具，通过命令行方式操作数据流。
## Spring Cloud Security：安全工具包，为你的应用程序添加安全控制，主要是指OAuth2。
## Spring Cloud Consul：封装了Consul操作，consul是一个服务发现与配置工具，与Docker容器可以无缝集成。
## Spring Cloud Zookeeper：操作Zookeeper的工具包，用于使用zookeeper方式的服务注册和发现。
## Spring Cloud Stream：数据流操作开发包，封装了与Redis,Rabbit、Kafka等发送接收消息。
## Spring Cloud CLI：基于 Spring Boot CLI，可以让你以命令行方式快速建立云组件。