---
title: "targo源码分析"
tags: ["golang"]
date: 2020-05-13
draft: true
---
Tarsgo是tars项目的go语言的基本包
### 关键概念 
Servant 服务接口定义
TarsServer tars服务端连接管理 监听与处理客户端请求
TarsClient tars客户端连接管理器 管理tcp连接和发送请求
AdapterProxy 连接适配器管理 tarsclient相关信息
EndpointManager 创建和管理AdapterProxy,选取合适的adapterproxy
Communicator 根据配置初始化EndpointManager

### 啥是服务
  servantHandler 实现dispatch接口, 分发请求
  Servant


### 项目结构
  tars包含client和server两部分 入口分别为 communicator的 stringtoproxy application run 和  

  `adapter` --适配器 服务节点, 请求状态的管理
  `admin` -- 接收和处理服务治理中心的消息
  `application` --框架入口 解析配置并初始化服务连接包括初始tars框架相关的服务 状态上报等...
  `communicator` -- 通讯器 每个服务对应一个communicator 根据配置初始化并管理adapter
  `endpointManager` -- 节点管理 一个服务可能有多个服务节点并通过set区分 endPointManager筛选和维护客户端链接的服务节点 set区分 异常过滤
  `filter` -- 过滤器 pre在Invoke前调用, 比如为所有请求设置context ip post在invoke后调用 比如统一处理 RetCode非0日志,
  `httpserver` -- tarsgo支持通过http方式调用tars服务
  `message` -- 上报数据?
  `nodef` -- 节点信息? 初始化communicatior 的基础数据
  `notifyf` -- 数据上报服务
  `propertyf` --节点信息上报
  `rconfig` -- 后台下发配置管理
  `servant` -- 服务proxy实例
  `tarsprotocol` -- client 底层通讯管理者 服务者
### tars流程
// 先看模板生成了什么东西

tarsGo中包含client和server的部分, 两者均通过communicator进行通讯管理

服务端
```
 // 
	imp := new(servant.BadgeServantImp)
	app := new(PGC.BadgeServant)
	app.AddServant(imp, BadgeServantObj")
	tars.Run()
```
 1-3行 初始化一个BadgeServant,绑定实现并制定服务名
 addServant 首先将服务实现与服务名绑定添加到服务队列中, 
 通过服务名查找服务配置 创建一个新的TarsServer保存所有服务相关的配置
 保存到服务列表中
 接着执行tars.run() 
 tars.run 首先将tars框架服务治理相关的服务通过addservant方式创建tarsserver并保存到服务队列中
 遍历服务队列根据服务协议启动不同的相关服务, 
 通过 tcphandler和udbhandler的listen方法 监听 handleConn 方法处理数据并最终调用server的invoke方法

客户端

客户端与服务端调用底层逻辑一致, 都是创建创建tarsserver并通过 tcphandler处理请求
不同的是由于服务治理, 客户端可能需要通过服务名去获取服务端配置并与多个服务端进行通讯,因此多出了
上层封装tars.Communicator 通讯器负责管理一组 endpoint 的socket通讯

Communicator

数据传输

tars rpc实现



请求处理


服务端
 run-> addServant->addServantCommon->NewTarsProtocol
 run-> mainloop(启动服务communicator) 启动tars框架client(admin repost status)

tars go 服务部分

根据jce文件生成 Servant模板文件 servant实现 dispatch接口对内部逻辑分发 
基于jce生成Servant模板文件 模板文件包含interface 自己实现
模板文件通过interface对接口实现进行约束, tars内部管理中抽象为dispatch接口 添加到 objRunList 进行统一管理

addServantCommon

tars run 执行stringtoproxy 解析配置 初始化上报服务 