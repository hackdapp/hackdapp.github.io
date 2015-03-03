title: OSGI插件开发示例
date: 2015-03-03 11:42:10
tags: 
- osgi
- 示例
- 原创

categories: 原创

---
### 环境搭建
- eclipse  开发工具
- Felix Framework `OSGI插件运行环境`
*  下载felix  
``` 
wget http://mirror.olnevhost.net/pub/apache//felix/org.apache.felix.main.distribution-4.6.0.tar.gz
```
* 解压文件
```
tar -zxvf org.apache.felix.main.distribution-4.6.0.tar.gz		
```
* 运行felix
```
cd felix-framework-4.6.0
java -jar bin/felix.jar 
```
**显示: "Welcome to Apache Felix Gogo" 表明安装成功.**  
至此,环境搭建完毕.  

### 开发步骤
#### 1. 创建plugin project   
![image](http://7wy48o.com1.z0.glb.clouddn.com/plugin-create.png-blogpic)  
创建成功后，会生成如下目录：  

			SampleOne					//工程名
				├── META-INF	
				│   └── MANIFEST.MF    //plugin配置文件，负现包目录的导入与导出，以及第三方jar的加载
				├── bin					//编译目录
				│   └── sampleone
				│       └── Activator.class
				├── build.properties
				└── src					//源代码目录
 			   		 └── sampleone
       				 └── Activator.java

#### 2. 编辑源码及配置文件：
  - 编辑`Activator.java`  
     此文件为插件装载启动或停止时，调用对应的start及stop方法。可以start方法中执行一些服务注册或初始化工作，以及在stop方法中。    
 ```
public class Activator implements BundleActivator {

	private static BundleContext context;

	static BundleContext getContext() {
		return context;
	}			
	
	public void start(BundleContext bundleContext) throws Exception {
		Activator.context = bundleContext;
		while(true){
			System.out.println("current time:"+new Date());			Thread.sleep(1000);
		}
	}

    public void stop(BundleContext bundleContext) throws Exception {
		Activator.context = null;
	}
}
 ```
- 编辑配置文件  
 ```
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Name: SampleOne
Bundle-SymbolicName: SampleOne
Bundle-Version: 1.0.0.qualifier
Bundle-Activator: sampleone.Activator	//注册初始化加载类，需要扩展实现BundleActivator.非必须配置
Import-Package: org.osgi.framework;version="1.3.0"
Bundle-RequiredExecutionEnvironment: JavaSE-1.7
```  

#### 3. 打包
- 选择打包的工程，右击export
- 在弹出的窗口，选中要打包的工程，填写jar包所要生成的目录
- 点击finish,打包完成.  
注：`打包过程，会在选择目录中生成一个plugins文件夹，而生成的jar就在此文件夹中.`    

### 示例运行
- 将上述生成的jar复制到felix-framework-4.6.0/bundle文件夹中  
- 运行osgi容器
```
java -jar bin/felix.jar
```


