title: 如何解决”ERROR Process failed:layout/_partial/.DS_Store”
date: 2016-12-13 00:52:09
tags:
- hexo问题
---
# 问题
![2016-12-13_00:53:25.jpg](http://7wy48o.com1.z0.glb.clouddn.com/2016-12-13_00:53:25.jpg)
# 解决方法
删除hexo安装目录下的所有”.DS_Store”文件
```
cd hexo安装目录
sudo find ./ -name ".DS_Store" -depth -exec rm {} \;
```
# 其它资料
## 什么是.DS_Store
这种文件存在于众多的OS文件目录中，Finder自动隐藏它们，所以用户不会感到其存在。.DS_Store几乎存在于每个文件当中。它是Finder记录客户定制文件夹显示方式的一种元数据文件（metadata）用于控制一个文件夹的显示方式（列表、图标、分栏和CoverFlow）和背景图标等。
## 相关设置
1. 禁止.DS_Store生成    

		> defaults write com.apple.desktopservices DSDontWriteNetworkStores true

2. 恢复.DS_Store生成  
			
		> defaults delete com.apple.desktopservices DSDontWriteNetworkStores