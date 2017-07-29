title: TextExpander在Mac上有时失效无法展开
categories: Mac应用
tags:
  - hackhook
  - 问题集锦
keywords:
  - hackhook
  - textexpander问题
  - textexpander
date: 2017-07-28 12:37:52
---
## 问题
在Mac环境下，使用textexpander时有时会无法展开。
# 原因
在中文环境下，使用**`缩写`**是无法展开的
## 解决办法
1. 打开脚本编辑器(Automator)
2. 创建脚本  
new->workflow->搜索框中检索’Run AppleScript’, 双击->粘入以下代码      
![](http://7wy48o.com1.z0.glb.clouddn.com/2017-07-28-051720.jpg)
3. 运行脚本
  
TextExpander: [下载地址](https://itunes.apple.com/us/app/textexpander-3-+-custom-keyboard/id917416298?mt=8)