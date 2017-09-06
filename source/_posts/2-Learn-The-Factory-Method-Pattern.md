title: 2. Learn The Factory Method Pattern
categories: 专栏分享(2017)
tags:
  - hackhook
keywords:
  - hackhook
date: 2017-09-06 12:44:49
---
## 概念定义

> 用于创建或实例化某个具体类型服务, 并将调用方与具体实现隔离. 比如, 在业务系统中，我们通常都会用到日志系统，而其中LoggerFactory就是此模式的一个典型

## 类图
![](http://7wy48o.com1.z0.glb.clouddn.com/2017-09-06-043139.jpg)

## 代码实现
1. Logger 接口定义
<script src="https://gist.github.com/thinklife/1bddaa555bb7780d0351e60e2de7ba0f.js"></script>
2. Logger 具体实现类
<script src="https://gist.github.com/thinklife/21198c505a3d2ad2bf728983fc1dbe41.js"></script>
3. 抽象工厂接口定义类
<script src="https://gist.github.com/thinklife/ec1138f1747d0bf5421d0e05c989e3ac.js"></script>
4. 抽象工厂具体实现类
<script src="https://gist.github.com/thinklife/7da14c3e220e3f240e8fe1b8880d2414.js"></script>
5. 客户端调用类
<script src="https://gist.github.com/thinklife/a38a0c02ed2641fdff8fe967dd390859.js"></script>

## 适用场景
1. 日志工厂类, 为整个系统提供一个独立日志服务
2. 缓存工厂类, 为业务系统提供一个可复用的业务存储组件.
