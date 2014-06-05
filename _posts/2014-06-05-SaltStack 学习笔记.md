---
layout: post
title: SaltStack 学习笔记
category: linux 管理工具
---

之前在培训机构学习了`Puppet`, 但是由于 `Puppet` 是使用 `Ruby` 语言开发的，所以学习的时候发现资源文件的配置管理等非常麻烦，语法等等也不习惯，所以对精研 `Puppet` 一直提不起兴趣。希望能有一款使用我熟悉的语言开发的，方便安装和配置的管理软件。

之后有一天我突然发现了`SaltStack`。粗粗查看了下文档，我就被她方便的安装，便利的资源文件的配置（使用`YAML`语法）等优点征服了...


#### 安装 `SaltStack`

安装`SaltStack`非常方便， 在 `CentOS6.5` 上只需要先安装 `epel`软件库。之后就可以方便的使用`yum`方式安装了。

- 在主控端安装`salt-master`
- 在代理端安装`salt-minion`

即可

#### 配置启动 `SaltStack`

- `master` 端的配置文件位置： `/etc/salt/master`
    默认情况下无需修改配置

- 启动`master`服务： 
        service salt-master start
        chkconfig salt-master on //开机启动服务
    
- 启动`minion`服务：
        service salt-minion start
        chkconfig salt-minion on //开机启动服务

#### 监控端和代理端互相通信

1. 首先要保证代理端能解析到服务端，不论是使用`DNS`服务或`hosts`文件都可以。对于代理端来说，只需要知道监控端的地址。监控端默认名字是`salt`。所以只要让代理端能解析`salt`到监控端即可。

2. 当代理端的服务启动之后，在监控端执行 `salt-key -L` 来查看当前等待连接的客户端的`hostname`。之后使用`salt-key -A`来接受所有的客户端请求。

3. 成功连接之后使用`salt ‘×’ test.ping` 来测试监控端的连接是否正常。
