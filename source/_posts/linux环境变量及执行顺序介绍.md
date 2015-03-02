title: linux环境变量及执行顺序介绍
date: 2015-03-02 16:09:25
tags:
 - linux
 - env
 - 随笔 
categories: linux
---
1. /etc/environment   　#系统环境变量,`系统启动时装载`
2. /etc/profile		#所有用户的环境变量,`用户登陆时装载`
3. ~/.bash_profile	#存在就执行,且后续４,５配置文件不加载
4. ~/.bash_login	#存在就执行,且后续５配置文件不加载
5. ~/.profile		#存在就行.
> 注:每次用户登录时,都会从２~５的顺序加载,有则加载并停止后续加载

<!-- more -->
６.~/.bashrc		#用户登录成功后,每启动一个terminal就会执行一次加载
