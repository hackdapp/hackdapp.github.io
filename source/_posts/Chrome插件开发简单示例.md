title: Chrome插件开发简单示例
date: 2015-11-05 22:19:35
tags:
- dev
---
教你如何快速创建一个Chrome Extension
---

## 1、创建一个文件夹用于存放插件信息
    >>> mkdir demo
    >>> cd demo
## 2、创建manifest.json文件    
    {
      "manifest_version": 2,
    
      "name": "Getting started example",
      "description": "This extension shows a Google Image search result for the current page",
      "version": "1.0",
    
      "browser_action": {
        "default_icon": "icon.png",
        "default_popup": "popup.html"
      },
      "permissions": [
        "activeTab",
        "https://ajax.googleapis.com/"
      ]
    }
[下载](https://developer.chrome.com/extensions/examples/tutorials/getstarted/manifest.json)
## 3、创建资源文件
### 3.1、创建插件图标（`icon.png`）
使用此图![default_icon](https://developer.chrome.com/extensions/examples/tutorials/getstarted/icon.png)作为插件图标  

[下载](https://developer.chrome.com/extensions/examples/tutorials/getstarted/icon.png)
### 3.2、创建popup页面
    <!-- Filename: popup.html -->
    <!doctype html>
    <!--
     This page is shown when the extension button is clicked, because the
     "browser_action" field in manifest.json contains the "default_popup" key with
     value "popup.html".
     -->
    <html>
      <head>
        <title>Getting Started Extension's Popup</title>
        <style>
          body {
            font-family: "Segoe UI", "Lucida Grande", Tahoma, sans-serif;
            font-size: 100%;
          }
          #status {
            /* avoid an excessively wide status text */
            white-space: pre;
            text-overflow: ellipsis;
            overflow: hidden;
            max-width: 400px;
          }
        </style>
    
        <!--
          - JavaScript and HTML must be in separate files: see our Content Security
          - Policy documentation[1] for details and explanation.
          -
          - [1]: https://developer.chrome.com/extensions/contentSecurityPolicy
         -->
        <script src="popup.js"></script>
      </head>
      <body>
        <div id="status"></div>
        <img id="image-result" hidden>
      </body>
    </html>
    

[下载popup.html](https://developer.chrome.com/extensions/examples/tutorials/getstarted/popup.html)  

[下载popup.js](https://developer.chrome.com/extensions/examples/tutorials/getstarted/popup.js)


## 4、安装插件
### 4.1 打开chrome浏览器，访问`chrome://extensions`
### 4.2 勾选`Developer mode`
### 4.3 选择`Load unpacked extension...`, 从弹出页面选择之前创建demo文件

到此，即可在chrome插件栏看到我们开发的插件。


