title: JPA自定义类加载数据
date: 2017-07-19 18:10:16
tags:
- jpa
- hibernate
keywords:
- jpa
- 自定义类
description:
---

## 问题场景
当通过JPA查询表数据时，可能只想返回表中的某两个字段数据，这时候Java如何接收数据呢? List<Map>? 还是要其它的呢?

## 解决办法
<script src="https://gist.github.com/thinklife/5748600c35b337a1d503bb7534ff85ff.js"></script>


参考资料
* https://docs.jboss.org/hibernate/orm/3.3/reference/en/html/queryhql.html

