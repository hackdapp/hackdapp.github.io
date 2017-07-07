title: Linux环境下如何定位程序目录
date: 2016-11-11 15:02:05
tags:
- Linux
---

1. 定位程序pid
lsof -i TCP:80
ps -ef | grep nginx

2. 定位目录
ll /proc/{pid}/exe