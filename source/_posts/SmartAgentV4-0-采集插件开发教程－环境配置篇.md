title: SmartAgentV4.0-采集插件开发教程－环境配置篇
date: 2015-03-04 10:24:52
tags:
---
本章节主要介绍开发过程中应用到的软件以及如何进行安装配置.
### 软件环境配置
- Eclipse	`开发软件`
- Apache Maven	`项目构建工具`，在此处主要是给gradle提供本地maven存储库，便于项目的快速构建
- Gradle  `项目构建工具`
- Github  `项目版本控制`
 
--- 

#### Eclipse安装  
`注：如果本地已安装eclipse,则检查其是否有plugin-in插件，有则可跳过此安装`

* [下载](http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/indigo/SR2/eclipse-jee-indigo-SR2-linux-gtk-x86_64.tar.gz)

		wget http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/indigo/SR2/eclipse-jee-indigo-SR2-linux-gtk-x86_64.tar.gz

* 安装  

		tar -zxvf eclipse-jee-indigo-SR2-linux-gtk-x86_64.tar.gz
	 
* 验证  
解压完成后，进入eclipse目录，运行eclipse出现欢迎界面，则表明安装完成。


#### Apache Maven安装

* [下载](http://mirrors.cnnic.cn/apache/maven/maven-3/3.0.5/binaries/apache-maven-3.0.5-bin.tar.gz)

		wget http://mirrors.cnnic.cn/apache/maven/maven-3/3.0.5/binaries/apache-maven-3.0.5-bin.tar.gz

* 安装并配置环境变量  
  	 - 解压
	
		tar -zxvf apache-maven-3.0.5-bin.tar.gz
		
	 - 添加环境变量  
	 	1) 编辑配置文件
	
			vim ~/.bash_profile
	
		2) 在文件中添加如下内容：
	
			export MAVEN_HOME=/usr/local/apache-maven-3.0.5
			export PATH=$PATH:$MAVEN_HOME/bin
		
		3) 打开命令窗口，执行如下命令，使环境变量生效。
			
			source ~/.bash_profile

* 验证  
	打开命令窗口，执行

		mvn -version
		
	如能正常显示版本信息,表明安装成功.


#### Gradle安装

* [下载](https://services.gradle.org/distributions/gradle-2.2-bin.zip)

		wget https://services.gradle.org/distributions/gradle-2.2-bin.zip

* 安装并配置环境变量 

	- 解压	
	
			unzip gradle-2.2-bin.zip
	
	- 添加环境变量
	
		1) 编辑配置文件
		
			vim ~/.bash_profile
			
		2) 在文件中添加如下内容
		
			export GRADLE_HOME=/usr/local/gradle-2.2
			export PATH=$PATH:$GRADLE_HOME/bin
			
		3) 命令窗口中运行如下命令，使其生效

			source ~/.bash_profile

* 验证  

	打开命令窗口，执行命令：

		gradle -version
		
	如能正常显示版本信息，则表明安装配置成功。



#### Github安装

* 下载

		apt-get install git
		
* 配置Git缺省配置

		git config --global user.name "YOUR NAME"
		git config --global user.email "YOUR EMAIL ADDRESS"

* 验证  
	打开命令窗口，执行命令：
	
		git -version
		
	如能正常显示版本信息，则表明安装配置成功。
 