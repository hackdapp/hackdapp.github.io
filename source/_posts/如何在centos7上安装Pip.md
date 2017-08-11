title: 如何在centos7上安装Pip
categories: 待分类
tags:
  - hackhook
keywords:
  - hackhook
date: 2017-08-08 14:32:10
---

## 方式一: `Install Pip with Yum`
##### Step #1: Add the EPEL Repository  

		yum -y update

##### Step #2: The Installation

		yum -y install python-pip

## 方式二: ` Install Pip with Curl and Python`
##### Step #1: donwload file: ‘get-pip.py’

	curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py”

##### Step #2: The Installation
	
	python get-pip.py
  
----
Wrap-up: Verify The Installation  
	
	pip -V