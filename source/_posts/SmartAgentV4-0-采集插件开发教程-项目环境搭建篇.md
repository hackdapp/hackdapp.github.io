title: SmartAgentV4.0-采集插件开发教程-项目环境搭建篇
date: 2015-03-04 11:56:24
tags:
---
此章节主要用于介绍整个项目的构建以及新项目环境的创建、配置、编译、打包等。

   
#### 分级目录:  

1. **项目环境安装**
2. **创建新项目**
3. **编码**
4. **测试**
5. **打包**

--- 

#### 项目环境安装  
	
* 克隆项目工程
	
		git clone https://github.com/JianKongBao/apm-agent.git -b 0.4
		
* 初始化项目工程	
		
		cd apm-agent
		gradle eclipse


#### 创建新项目

1. 创建项目
	
			mkdir agent-plugin-sample-discover
			cd agent-plugin-sample-discover
			mkdir -p src/{main,test}/{java,resource}  <!-- 生成源文件目录 -->
		
2. 配置项目配置  
创建好项目工程目录后，为工程添加gradle配置文件  

	文件名：`build.gradle`
		
	```
	apply plugin: 'osgi'                                                        
  
	sourceSets {
      	 main {
      	     java { srcDir 'src/main/java' }
      	     resources { srcDir 'src/main/resources' }
     	 }
     	 test {
           java { srcDir 'src/test/java' }
           resources { srcDir 'src/test/resources' }
     	 }
	}
	
	dependencies {
		compile 'com.cloudwise.smartagent:agent-plugin-api:1.0'
		compile 'org.osgi:org.osgi.core:6.0.0'
	}
 
    jar {
        manifest {
        	instruction 'Bundle-Activator','com.cloudwise.smartagent.plugin.sample.discover.Activator'
        	instruction 'Import-Package',
				'com.cloudwise.smartagent.plugin.api',
				'com.cloudwise.smartagent.plugin.api.exception',
				'com.cloudwise.smartagent.plugin.api.model',
				'org.osgi.framework;version="1.3.0"'
        	instruction 'Export-Package',''
        }
    }
	
	```
3. 初始化agent-plugin-sample-discover项目工程
	
		gradle agent-plugin-sample-discover:eclipse
		
#### 编码

- 编写plugin-api接口的实现类（这里只演示IDiscover的接口实现，ICollect的接口实现如法泡制即可。）

	```
	package com.cloudwise.smartagent.plugin.sample.discover;

	import com.cloudwise.smartagent.plugin.api.ADiscover;
	import com.cloudwise.smartagent.plugin.api.IDiscover;
	import com.cloudwise.smartagent.plugin.api.model.ServiceInfo;

	public class SampleDiscover extends ADiscover implements IDiscover{

	
	public SampleDiscover() {
		super("10012", "Sample", 10012);
	}

	@Override
	protected ServiceInfo fetchData() throws Exception {
		ServiceInfo service = newInstanceServiceInfo();
		service.addAttache("info-id", "001");
		service.addAttache("info-conf", "d://sample/config/conf.xml");
		service.addAttache("info-port", "9080");
		service.addAttache("info-installpath", "d://sample/");
		return service;
	}

	}
```

- 注册实现

	```
	 
	public class Activator implements BundleActivator {

		private static BundleContext context;

		static BundleContext getContext() {
			return context;
		}
		private IDiscover discover ;

	  	public void start(BundleContext bundleContext) throws Exception {
			discover = new SampleDiscover();
			Dictionary dic = new Properties();
			dic.put("id", discover.id());
			dic.put("name", discover.name());
			dic.put("group", IDiscover.class.getName());
			bundleContext.registerService(IDiscover.class, discover, dic);
		}

 
		public void stop(BundleContext bundleContext) throws Exception {
			List<ServiceReference> list = new ArrayList(bundleContext.getServiceReferences(IDiscover.class, "(id="+ discover.id() + ")"));
			if (list != null && list.size() == 1) {
				bundleContext.ungetService(list.get(0));
			}
			Activator.context = null;
		}

	}

	```
	
#### 打包
打开命令窗口，切换到主目录下，执行

	gradle agent-plugin-sample-discover:build

命令执行完成后，会在agent-plugin-sample-discover/build/libs目录下生成一个jar文件。


---
到此，SampleDiscover插件就实现结束了，只需要发布到SmartAgentFramework中就可以运行了。