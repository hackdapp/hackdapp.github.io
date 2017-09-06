title: 3. Learn The Abstract Factory Pattern
categories: 专栏分享(2017)
tags:
  - hackhook
keywords:
  - hackhook
date: 2017-09-06 13:06:44
---
## 概念定义
> 通过委托类来创建或实例化多个不同类型服务, 并将调用方与具体实现隔离. 比如: 不同的汽车生产厂商生产不同类型的汽车

## 类图
![](http://7wy48o.com1.z0.glb.clouddn.com/2017-09-06-045236.jpg)

## 代码实现
1.  多种车型接口定义
<script src="https://gist.github.com/thinklife/7b1925679db52f2d13899e8ad32a4d02.js"></script>
<script src="https://gist.github.com/thinklife/7639b0ae7082c04808020af10e60f0d1.js"></script>
2.  多种具体车型实现
<script src="https://gist.github.com/thinklife/7bc95b95d6a690a552b43c947646a84c.js"></script>
<script src="https://gist.github.com/thinklife/e887fc2cd226f17347518bc6ea30bae9.js"></script>
3.  不同厂商实现
<script src="https://gist.github.com/thinklife/0df49cf9815b60fd78908101f736176b.js"></script>
<script src="https://gist.github.com/thinklife/66ef59f32498d93150507afd9580d11d.js"></script>
4. 客户端调用
<script src="https://gist.github.com/thinklife/0d4c34be3edede83701199c4f53a22c2.js"></script>

## 适用场景
1. 不同汽车生商厂家生产不同车型
2. 数据库方言