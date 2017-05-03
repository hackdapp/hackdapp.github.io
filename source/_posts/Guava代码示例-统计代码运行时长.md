title: 'Guava代码示例: 统计代码运行时长'
date: 2017-01-28 21:52:39
tags:
- Guava代码示例
---
当我们需要业务场景中需要统计某一段代码的运行时长，通常是这样做的

```
long st = new Date().getTime();
doSomething();
long ed = new Date().getTime();
System.out.println("time: " + (ed - st));
```　

而guava提供了更为简单的方式: 
```
Stopwatch stopwatch = Stopwatch.createStarted();
doSomething();
stopwatch.stop();
System.out.println("time: " + stopwatch.elapsed(TimeUnit.MILLISECONDS));
```