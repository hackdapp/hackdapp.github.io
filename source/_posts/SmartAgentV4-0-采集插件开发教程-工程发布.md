title: SmartAgentV4.0-采集插件开发教程-工程发布
date: 2015-03-04 16:20:41
tags:
---
### 工程发布

在确认上续代码都已完成后，只需发布各工程jar到运行容器中，即可完成采集器的整体构建。

-  构建整个工程

	gradle build
	
-  复制所有工程jar

		cp agent-\*/build/libs/\*.jar framework/build/SmartAgent/plugins
	
-	运行容器

		java -jar bin/framework-1.0.jar 

-	web访问
	
		http://localhost:8080/smartagent/discover