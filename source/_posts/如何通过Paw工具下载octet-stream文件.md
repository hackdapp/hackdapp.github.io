title: 如何通过Paw工具下载octet-stream文件
categories: 技术问题
tags:
  - hackhook
  - paw
keywords:
  - hackhook
  - paw
  - postman
  - application/octet-stream
  - 接口文件导出
date: 2017-08-18 12:31:39
---
# 问题
用Paw或postman调用一个post接口且返回数据是application/octet-stream; charset=UTF-8的时，如何进行本地保存呢？
![](http://7wy48o.com1.z0.glb.clouddn.com/2017-08-18-043604.jpg)

# 解决办法
1. 打开Paw Preferences，在Response栏目下选中`Show Hex Response Tab`
![](http://7wy48o.com1.z0.glb.clouddn.com/2017-08-18-044136.jpg)  
2. 调用下载文件的接口，在响应区域选中Resonse->hex, 最后选择File->ExportResponse->ResponseBody
![](http://7wy48o.com1.z0.glb.clouddn.com/2017-08-18-063419.jpg)

参考资料:
1. https://stackoverflow.com/questions/42282589/how-does-paw-handles-download-application-octet-stream