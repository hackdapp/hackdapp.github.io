title: 180407|30分钟hyperledger-fabric搭建指南
categories: 专栏分享(2017)
tags:
  - hackhook
keywords:
  - hackhook
date: 2018-04-06 09:02:10
---
# 30分钟hyperledger-fabric搭建指南
本教程会带领大家在amazon的ec2实例机器上进行hyperledger-fabric的`环境搭建`以及`fabric-sample示例的合约安装与方法调用`。

为了保证大家在安装过程中的流畅性，特意对比了本地系统、阿里云、aws三种环境下的安装注程，综合对比后发现amazon机器实例不仅安装效率高、问题少，最关键的是**12个月免费使用优惠**，可以减少一些非fabric相关问题的干扰。

## 前期准备
### 创建 AWS 账户
![](http://cdn.hackhook.com/2018-04-05-AWS%20Console%20-%20Signup.png)
整个注册过程，需要用到手机号、邮箱、信用卡。因注册过程较为简单，就不在此赘述。

**注册链接**: [https://portal.aws.amazon.com/billing/signup#/start](https://portal.aws.amazon.com/billing/signup#/start)
### 创建机器实例
#### 1. 注册成功后，进入控制台界面
![](http://cdn.hackhook.com/2018-04-05-us-west-2.console.aws.amazon.com_console_home_region-us-west-2.png)
#### 2. 点击`服务`, 在展开的列表中选择`EC2实例`
![](http://cdn.hackhook.com/2018-04-05-120548.jpg)
#### 3. 点击`启动实例`
![](http://cdn.hackhook.com/2018-04-05-122457.jpg)
#### 4. 选择要创建的系统类型，选择`ubuntu 16.04`版本系统
![](http://cdn.hackhook.com/2018-04-05-123111.jpg)
#### 5. 选择`实例类型`，点击`审核和启动`
![](http://cdn.hackhook.com/2018-04-05-123639.jpg)
#### 6. 进入实例启动检查页面，`配置安全组`
![](http://cdn.hackhook.com/2018-04-05-130137.jpg)
![](http://cdn.hackhook.com/2018-04-05-130612.jpg)
![](http://cdn.hackhook.com/2018-04-05-130742.jpg)
#### 7. `生成访问密钥`并下载
![](http://cdn.hackhook.com/2018-04-05-132047.jpg)
注：**备份好生成的密钥文件**，后面会用此文件进行远程ssh访问。
#### 8. 查看机器实例`公网IP`
![](http://cdn.hackhook.com/2018-04-05-133037.jpg)
![](http://cdn.hackhook.com/2018-04-05-133332.jpg)

### 远程连接EC2实例(SecureCRT)
#### 1. 打开SecureCRT软件
![](http://cdn.hackhook.com/2018-04-05-134138.jpg)
#### 2. 配置连接
![](http://cdn.hackhook.com/2018-04-05-133935.jpg)
#### 3. 连接远程服务器
![](http://cdn.hackhook.com/2018-04-05-134439.jpg)

## 环境搭建
### 1. 安装go环境
#### A. 下载go安装包
```
> wget https://storage.googleapis.com/golang/go1.9.linux-amd64.tar.gz
```
#### B. 解压包
```
> sudo tar -C /usr/local -xzf go1.9.linux-amd64.tar.gz
```
#### C. 配置环境变量

```
# 编辑配置文件
> vi ~/.profile

# 在文件末尾加入以下内容:
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

# 加载环境变量
> source ~/.profile
```
 

#### D. 验证配置

```
> go version
go version go1.9 linux/amd64
```
显示`go version go1.9 linux/amd64`表示配置成功.

### 2. 安装docker及docker-compose

#### 安装docker

```
# 更新包
> sudo apt-get update
> sudo apt-get upgrade

# 安装docker
> sudo apt-get install -y docker.io

# 启动docker以及开机启动设置
> sudo systemctl start docker
> sudo systemctl enable docker

# 修改当前用户所属用户组
> sudo usermod -aG docker ubuntu 
注意：一定要**注销并重新登录**配置才生效。

# 验证docker是否安装成功
> docker version

Client:
 Version:      1.13.1
 API version:  1.26
 Go version:   go1.6.2
 Git commit:   092cba3
 Built:        Thu Nov  2 20:40:23 2017
 OS/Arch:      linux/amd64

Server:
 Version:      1.13.1
 API version:  1.26 (minimum version 1.12)
 Go version:   go1.6.2
 Git commit:   092cba3
 Built:        Thu Nov  2 20:40:23 2017
 OS/Arch:      linux/amd64
 Experimental: false

```
如能正确显示以上内容，则表明安装成功.

#### 安装docker-compose
Docker-compose是支持通过模板脚本批量创建Docker容器的一个组件。

```
# 安装docker-compose
> sudo apt-get install -y docker-compose

# 验证是否安装成功
> docker-compose version

docker-compose version 1.8.0, build unknown
docker-py version: 1.9.0
CPython version: 2.7.12
OpenSSL version: OpenSSL 1.0.2g  1 Mar 2016
```

### 3. 安装fabric
#### 创建工程目录并进入该目录

```
> mkdir -p ~/go/src/github.com/hyperledger && cd "$_"
```

#### 下载`Hyperledger Fabric Samples`

```
> git clone https://github.com/hyperledger/fabric-samples.git
```

#### 拉取Fabric docker依赖镜像

```
> cd ~/go/src/github.com/hyperledger/fabric-samples
> ./scripts/fabric-preload.sh
```

#### 启动基础网络: orderer/peer/cli/chaincode
该基础网络主要包含：
- orderer容器：用于运行排序节点; 
- peer容器：用于运行普通的节点;
- cli容器：用于运行客户端工具;
- chaincode容器： 用于运行智能合约
`docker-compose-simple.yaml`配置文件主要配置了以上四个容器的启动参数

```
> cd ~/go/src/github.com/hyperledger/fabric-samples/chaincode-docker-devmode
> docker-compose -f docker-compose-simple.yaml up -d

Creating orderer
Creating peer
Creating chaincode
Creating cli
```

启动容器之后，可以通过以下命令查看已启动服务: 

```
> docker ps

CONTAINER ID        IMAGE                        COMMAND                  CREATED              STATUS              PORTS                                            NAMES
5203bb0d7aaa        hyperledger/fabric-ccenv     "/bin/bash -c 'sle..."   About a minute ago   Up About a minute                                                    chaincode
78bb278b7ac0        hyperledger/fabric-tools     "/bin/bash -c ./sc..."   About a minute ago   Up About a minute                                                    cli
707df74a599b        hyperledger/fabric-peer      "peer node start -..."   About a minute ago   Up About a minute   0.0.0.0:7051->7051/tcp, 0.0.0.0:7053->7053/tcp   peer
7f30502675f5        hyperledger/fabric-orderer   "orderer"                About a minute ago   Up About a minute   0.0.0.0:7050->7050/tcp                           orderer
```

#### 编译合约并注册合约
##### 1. 进入chaincode命令窗口

```
> docker exec -it chaincode bash
#命令窗口会进入以下状态: 
root@5203bb0d7aaa:/opt/gopath/src/chaincode>
```

##### 2. 编译合约文件

```
root@5203bb0d7aaa:/opt/gopath/src/chaincode> cd chaincode_example02/go
root@5203bb0d7aaa:/opt/gopath/src/chaincode> go build
```

##### 3. 注册合约

```
root@5203bb0d7aaa:/opt/gopath/src/chaincode> CORE_PEER_ADDRESS=peer:7051 CORE_CHAINCODE_ID_NAME=mycc:0 ./go

2018-04-06 02:52:23.128 UTC [shim] SetupChaincodeLogging -> INFO 001 Chaincode log level not provided; defaulting to: INFO
2018-04-06 02:52:23.128 UTC [shim] SetupChaincodeLogging -> INFO 002 Chaincode (build level: ) starting up ...
# 保持该窗口不要动，后续本窗口会打印合约调用时所输出的日志文件
```

***注：保持当前窗口不要动，另开一个ssh连接窗口***

#### 安装合约(新开ssh窗口)

##### 1. 进入**cli容器窗口**

```
> docker exec -it cli bash
root@78bb278b7ac0:/opt/gopath/src/chaincodedev>
```

##### 2. 安装合约

```
root@78bb278b7ac0:/opt/gopath/src/chaincodedev> peer chaincode install -p chaincodedev/chaincode/chaincode_example02/go -n mycc -v 0

2018-04-06 03:05:25.856 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-04-06 03:05:25.856 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-04-06 03:05:25.856 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-04-06 03:05:25.856 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-04-06 03:05:25.910 UTC [golang-platform] getCodeFromFS -> DEBU 005 getCodeFromFS chaincodedev/chaincode/chaincode_example02/go
2018-04-06 03:05:26.061 UTC [golang-platform] func1 -> DEBU 006 Discarding GOROOT package fmt
2018-04-06 03:05:26.061 UTC [golang-platform] func1 -> DEBU 007 Discarding provided package github.com/hyperledger/fabric/core/chaincode/shim
2018-04-06 03:05:26.061 UTC [golang-platform] func1 -> DEBU 008 Discarding provided package github.com/hyperledger/fabric/protos/peer
2018-04-06 03:05:26.062 UTC [golang-platform] func1 -> DEBU 009 Discarding GOROOT package strconv
2018-04-06 03:05:26.062 UTC [golang-platform] GetDeploymentPayload -> DEBU 00a done
2018-04-06 03:05:26.064 UTC [msp/identity] Sign -> DEBU 00b Sign: plaintext: 0AAD070A5B08031A0B08F6C79BD60510...2EE3DF010000FFFF2684EE73001C0000 
2018-04-06 03:05:26.064 UTC [msp/identity] Sign -> DEBU 00c Sign: digest: CD77F89CECDF1935F58C06498D9F92F6D21537074322EA02582FDBD66B930C17 
2018-04-06 03:05:26.072 UTC [chaincodeCmd] install -> DEBU 00d Installed remotely response:<status:200 payload:"OK" > 
2018-04-06 03:05:26.072 UTC [main] main -> INFO 00e Exiting.....
```

#### 3. 合约方法调用
##### 1) 方法调用: 初始化合约

```
#a帐户与b帐户分别初始化为: 10000/20000
root@78bb278b7ac0:/opt/gopath/src/chaincodedev# peer chaincode instantiate -n mycc -v 0 -c'{"Args":["init","a","10000","b","20000"]}' -C myc

2018-04-06 03:06:35.331 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-04-06 03:06:35.331 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-04-06 03:06:35.332 UTC [msp/identity] Sign -> DEBU 003 Sign: plaintext: 0AAE070A5C08011A0C08BBC89BD60510...436F6E666967426C6F636B0A036D7963 
2018-04-06 03:06:35.332 UTC [msp/identity] Sign -> DEBU 004 Sign: digest: F829A254295F3414E6BDA6A82D8BE68DEF6454E5C188EA39F8E351DAB978F39E 
......日志文件太长，删掉部分。
2018-04-06 03:06:35.358 UTC [chaincodeCmd] InitCmdFactory -> INFO 09e Get chain(myc) orderer endpoint: orderer:7050
2018-04-06 03:06:35.378 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 09f Using default escc
2018-04-06 03:06:35.378 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 0a0 Using default vscc
2018-04-06 03:06:35.379 UTC [msp/identity] Sign -> DEBU 0a1 Sign: plaintext: 0AB3070A6108031A0C08BBC89BD60510...30300A000A04657363630A0476736363 
2018-04-06 03:06:35.379 UTC [msp/identity] Sign -> DEBU 0a2 Sign: digest: FC2A829D28311280117CBBA89F11DBEC4513EBEF7382CADBFA0C97801CF5D7B9 
2018-04-06 03:06:35.396 UTC [msp/identity] Sign -> DEBU 0a3 Sign: plaintext: 0AB3070A6108031A0C08BBC89BD60510...46F9365DC192E252C3D372FBD9F18FFD 
2018-04-06 03:06:35.396 UTC [msp/identity] Sign -> DEBU 0a4 Sign: digest: 48003437C42BF212E6CC57A5F96B041624F4DAF4397E60A56C01D1D0AC8AE36A 
2018-04-06 03:06:35.403 UTC [main] main -> INFO 0a5 Exiting.....

# 查询a帐户
root@78bb278b7ac0:/opt/gopath/src/chaincodedev# peer chaincode query -n mycc -c '{"Args":["query","a"]}' -C myc

2018-04-06 03:10:53.415 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-04-06 03:10:53.415 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-04-06 03:10:53.415 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-04-06 03:10:53.416 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-04-06 03:10:53.416 UTC [msp/identity] Sign -> DEBU 005 Sign: plaintext: 0AB3070A6108031A0C08BDCA9BD60510...6D7963631A0A0A0571756572790A0161 
2018-04-06 03:10:53.416 UTC [msp/identity] Sign -> DEBU 006 Sign: digest: 37D0A0ABA2A96041A6EF843251271558425ADDF69025809F0779B9B16491C25C 
Query Result: 10000 #注：a帐户初始值
2018-04-06 03:10:53.428 UTC [main] main -> INFO 007 Exiting.....

# 查询a帐户
root@78bb278b7ac0:/opt/gopath/src/chaincodedev# peer chaincode query -n mycc -c '{"Args":["query","b"]}' -C myc

2018-04-06 03:13:22.778 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-04-06 03:13:22.778 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-04-06 03:13:22.778 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-04-06 03:13:22.778 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-04-06 03:13:22.779 UTC [msp/identity] Sign -> DEBU 005 Sign: plaintext: 0AB3070A6108031A0C08D2CB9BD60510...6D7963631A0A0A0571756572790A0162 
2018-04-06 03:13:22.779 UTC [msp/identity] Sign -> DEBU 006 Sign: digest: 64DDC455282EC189BF10432AC0FD92457A2F47EBD2987D3325BF7DC37FD457BA 
Query Result: 20000 #注：b帐户初始值
2018-04-06 03:13:22.791 UTC [main] main -> INFO 007 Exiting.....
```

##### 2) 方法调用: 转帐

```
# invoke to move 10 from a to b
root@78bb278b7ac0:/opt/gopath/src/chaincodedev# peer chaincode invoke -n mycc -c '{"Args":["invoke","a","b","10"]}' -C myc

2018-04-06 03:17:30.923 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-04-06 03:17:30.923 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-04-06 03:17:30.923 UTC [msp/identity] Sign -> DEBU 003 Sign: plaintext: 0AAE070A5C08011A0C08CACD9BD60510...436F6E666967426C6F636B0A036D7963 
2018-04-06 03:17:30.923 UTC [msp/identity] Sign -> DEBU 004 Sign: digest: 0308DC9AE35B3C4EC779057BFFEF94531159D3DB29CB3B1031F3933376976FCC 
......
2018-04-06 03:17:30.990 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> DEBU 0a5 ESCC invoke result: version:1 response:<status:200 message:"OK" > payload:"\n \365\215l\210\337\206\344s\323S8X\344\265\366e\245\253t\334\244\361\263fQ\267k\233\310p\302\257\022[\nI\022\024\n\004lscc\022\014\n\n\n\004mycc\022\002\010\001\0221\n\004mycc\022)\n\007\n\001a\022\002\010\001\n\007\n\001b\022\002\010\001\032\t\n\001a\032\0049990\032\n\n\001b\032\00520010\032\003\010\310\001\"\t\022\004mycc\032\0010" endorsement:<endorser:"\n\007DEFAULT\022\244\006-----BEGIN -----\nMIICNjCCAd2gAwIBAgIRAMnf9/dmV9RvCCVw9pZQUfUwCgYIKoZIzj0EAwIwgYEx\nCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1TYW4g\nRnJhbmNpc2NvMRkwFwYDVQQKExBvcmcxLmV4YW1wbGUuY29tMQwwCgYDVQQLEwND\nT1AxHDAaBgNVBAMTE2NhLm9yZzEuZXhhbXBsZS5jb20wHhcNMTcxMTEyMTM0MTEx\nWhcNMjcxMTEwMTM0MTExWjBpMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZv\ncm5pYTEWMBQGA1UEBxMNU2FuIEZyYW5jaXNjbzEMMAoGA1UECxMDQ09QMR8wHQYD\nVQQDExZwZWVyMC5vcmcxLmV4YW1wbGUuY29tMFkwEwYHKoZIzj0CAQYIKoZIzj0D\nAQcDQgAEZ8S4V71OBJpyMIVZdwYdFXAckItrpvSrCf0HQg40WW9XSoOOO76I+Umf\nEkmTlIJXP7/AyRRSRU38oI8Ivtu4M6NNMEswDgYDVR0PAQH/BAQDAgeAMAwGA1Ud\nEwEB/wQCMAAwKwYDVR0jBCQwIoAginORIhnPEFZUhXm6eWBkm7K7Zc8R4/z7LW4H\nossDlCswCgYIKoZIzj0EAwIDRwAwRAIgVikIUZzgfuFsGLQHWJUVJCU7pDaETkaz\nPzFgsCiLxUACICgzJYlW7nvZxP7b6tbeu3t8mrhMXQs956mD4+BoKuNI\n-----END -----\n" signature:"0E\002!\000\201\300bN\030\326\264\352\335bT\237\252\020|\010\t\024#\217(l`\215\267\231k\r%\215\365{\002 #4\357\000\234\034\3560\327\343\2375\277\364\036\207\372\264\237\344\333\032\215\240;\360\334\202:\325\341\365" > 
2018-04-06 03:17:30.990 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> INFO 0a6 Chaincode invoke successful. result: status:200 
2018-04-06 03:17:30.991 UTC [main] main -> INFO 0a7 Exiting.....
```

##### 3) 方法调用: 查询
###### 查询帐户a

```
root@78bb278b7ac0:/opt/gopath/src/chaincodedev# peer chaincode query -n mycc -c '{"Args":["query","a"]}' -C myc

2018-04-06 03:20:39.812 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-04-06 03:20:39.812 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-04-06 03:20:39.812 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-04-06 03:20:39.812 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-04-06 03:20:39.813 UTC [msp/identity] Sign -> DEBU 005 Sign: plaintext: 0AB3070A6108031A0C0887CF9BD60510...6D7963631A0A0A0571756572790A0161 
2018-04-06 03:20:39.813 UTC [msp/identity] Sign -> DEBU 006 Sign: digest: EB5D8C5E7DBE18348D884CFF60E6860F5787C0BFA73B774D579BD96565A1B70F 
Query Result: 9990
2018-04-06 03:20:39.825 UTC [main] main -> INFO 007 Exiting.....
```

###### 查询帐户b

```
root@78bb278b7ac0:/opt/gopath/src/chaincodedev# peer chaincode query -n mycc -c '{"Args":["query","b"]}' -C myc 

2018-04-06 03:22:14.208 UTC [msp] GetLocalMSP -> DEBU 001 Returning existing local MSP
2018-04-06 03:22:14.208 UTC [msp] GetDefaultSigningIdentity -> DEBU 002 Obtaining default signing identity
2018-04-06 03:22:14.208 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2018-04-06 03:22:14.208 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2018-04-06 03:22:14.209 UTC [msp/identity] Sign -> DEBU 005 Sign: plaintext: 0AB2070A6008031A0B08E6CF9BD60510...6D7963631A0A0A0571756572790A0162 
2018-04-06 03:22:14.209 UTC [msp/identity] Sign -> DEBU 006 Sign: digest: 06950383F096D62E19B50C664D31B7C47D85CB12812CB30BDCB86F613ADAA315 
Query Result: 20010
2018-04-06 03:22:14.221 UTC [main] main -> INFO 007 Exiting.....
```

-----
到此，整个fabric的安装部署以及合约的测试过程就算完成了。

`注：在安装过程如果出现任何问题，欢迎加我微信(wechat:rushking2009)打扰。`

同时欢迎关注以下我的个人公众号，了解最新动态。
![](http://cdn.hackhook.com/2018-03-27-164508.png)
-----
**参考资源**
1. [hyperledger-fabric官方文档](http://hyperledger-fabric.readthedocs.io/en/latest/)
2. [Hyperledger Fabric 1.0 从零开始（一）](http://www.cnblogs.com/aberic/p/7527831.html)
3. [深入理解Fabric环境搭建的详细过程](https://www.cnblogs.com/studyzy/p/7451276.html)
4. [区块链之Hyperledger（超级账本）Fabric v1.0 的环境搭建（超详细教程）](https://blog.csdn.net/so5418418/article/details/78355868)


