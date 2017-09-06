title: 1. Learn The Singleton Pattern
categories: 专栏分享(2017)
tags:
  - hackhook
keywords:
  - hackhook
date: 2017-09-06 11:47:23
---
## 概念定义
>  在应用程序的生命周期内，只允许返回一个实例。比如: 系统配置文件在启动时时只需要加载一次，在以后使用的过程中从此实例中获取即可。

## 代码实现
 单例实例方式可谓是多种多样，下面给大家分享几种实现:

#### 1. 简单方式, 线程不安全 [[Sample Code]](https://gist.github.com/thinklife/d96fd25d9dfb062d6a2d0e40792e6689)
<script src="https://gist.github.com/thinklife/d96fd25d9dfb062d6a2d0e40792e6689.js"></script>
**缺点: 线程不安全，容易多次实例化**

#### 2. 简单方式, 线程安全 [[Sample Code]](https://gist.github.com/thinklife/ae60af3b8f7da277434ee75005bc00f0)
<script src="https://gist.github.com/thinklife/ae60af3b8f7da277434ee75005bc00f0.js"></script>
**缺点:  线程安全，但性能差(初始化只需要执行一次即可，这种方式导致每次调用都要加锁，故效率差)**

#### 3. 双重检查方式 [[Sample Code]](https://gist.github.com/thinklife/85e7fefbf8ca12ec0418aecccee1cb5d)
<script src="https://gist.github.com/thinklife/85e7fefbf8ca12ec0418aecccee1cb5d.js"></script>
**缺点: 在多线程环境下，有可能在实例未完成的情况下调用会出现错误**
那么具体如何理解呢?
> Singleton instance = new Singleton()

JVM在进行上述代码的过程，会进行三件事情: 
1. 分配内存空间
2. 调用实例构造函数初始化类变量
3. 将实例引用指向该内存空间
`乱序执行`就意味着2、3这两步运行顺序不确定，如果3运行在2之前，那么就会出现实例引用不为null但并未初始化的情况，程序中调用就会因初始化数据的不正确引用导致业务逻辑错误。那么是否可以解决此问题呢? 具体解决办法如下:  
<script src="https://gist.github.com/thinklife/4498f6793132653c2f3274208c900606.js"></script>
**将单例模式中的单例实例声明为volatile修饰符**

#### 4.  **推荐** 静态内部类方式 [[Sample Code]](https://gist.github.com/thinklife/77583fa8652ad2d9b73e8d4336b8fe1b)
<script src="https://gist.github.com/thinklife/77583fa8652ad2d9b73e8d4336b8fe1b.js"></script>
**注:  这种方式也是《Effective Java》书上推荐的用法**
volatile的作用就在于告诉jvm禁止指令乱排序

## 适用场景
1. 系统配置工具类. 系统启动时只需解析一次配置文件并缓存到实例变量中，当调用方想要获取某个配置时，只要从该实例变量中获取即可。
2. 缓存工具类.
3. 数据源工具类. 
