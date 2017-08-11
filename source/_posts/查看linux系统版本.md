title: 查看linux系统版本
categories: linux
tags:
  - hackhook
  - linux命令
keywords:
  - hackhook
  - linux版本
  - linux内核
  - linux版本号
date: 2017-08-10 11:34:25
---
# 1. Finding your kernel release

		uname -r
  
![](http://7wy48o.com1.z0.glb.clouddn.com/2017-08-10-034604.jpg)
# 2. Finding your distribution release

		lsb_release -a
  
![](http://7wy48o.com1.z0.glb.clouddn.com/2017-08-10-034708.jpg)
# 3. Finding a specific package release number

		dpkg -l openssl
![](http://7wy48o.com1.z0.glb.clouddn.com/2017-08-10-034840.jpg)