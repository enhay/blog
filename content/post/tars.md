---
title: "tars欺萌"
tags: ["tars","rpc"]
date: 2019-05-15
draft: false
---

# tars
  tars是腾讯开源的rpc框架,支持服务治理和多语言
# rpc
  首先了解下什么叫RPC,RPC是指远程服务调用,可以简单理解为从本地调用远程方法,简单流程如下图
  ![operating-system-remote-call-procedure-working](https://ghost-1251180266.cos.ap-beijing.myqcloud.com/tars_20190515_102643.png)
  * 首先客户端调用本地代理方法,本地代理编码数据并通过网络发送给服务端

  * 服务端收到收到数据后对数据解码后调用服务端本地方法

  * 处理后的数据编码后通过网络传输给客户端

  * 客户端收到数据后解码获取结果

  从上述流程图可以看出 RPC 服务主要涉及两点,如何**连接远程服务**和**数据如何传输**
##  通信与寻址
  想要调用服务端方法就要先连接到服务端
  通过提供endpoint(包含主机名ip地址,端口,服务名)信息,通过socket(通常使用tcp)来建立连接通道,并把相关服务注册为socket的handler,为后来解析数据流中的方法参数等信息来调用本地方法做准备.
  对于提供服务发现的rpc框架,其本质也是通过调用服务发现这个中介根据服务名获取服务的endpoint信息

##  通信协议

  当客户端发起一个远程调用时,传输数据中包含方法名和参数等信息,由于底层是通过socket使用二进制传输的,所以需要将数据序列化,同样服务端收到字节流时也需要对数据进行反序列化,
  为了能正常的进行信息交换协同工作,双方需要约定一个通信协议.
  比如可以约定使用文本协议如json来进行通信.然而文本协议虽然兼容性好,易调试(所见即所得),但是安全和性能却不够.想象一下把一个bool转为string后多传输了多少字节.所以从安全和性能考虑通常rpc框架选择使用二进制通信协议序列化.

  tars使用jce协议,jce协议使用[TTLV](https://blog.csdn.net/eddysong9280/article/details/9943969)进行编码.
  TT[L]V在 TTL `<Tag, Length, Value>`三元组编码的基础上改进的 使用`<tag，type，length，value>` 的编码协议, 改进后的TTLV即增加了类型描述,同时对于定长基本数据类型,也省略了不必要的长度描述,另外通过对Type类型描述的约定,可方便的实现跨语言通信.
  如下一个jce文件
  ```
  struct Test
  {
      0 require string name;
      1 require int age;
  }
  ```
  对于这个数据如果使用<Tag, Type, Length, Value>四元组编码(TTLV),数据内存组织大概是这样
  ![无标题](https://ghost-1251180266.cos.ap-beijing.myqcloud.com/tars_20190515_130231.png)

  规则定下后就可以使用代码生成工具来生成相应的结构体的编解码文件.

# tars的服务治理
  tars能实现支持多语言服务治理在于其框架提供了一系列的tars服务,并在相应的库实现了调用逻辑
  tars框架的基础服务包含
  * **tarsAdminRegistry** tars服务管理后台,提供如服务状态查询,日志设置等功能
  * **tarsregistry** 是微服务集群的管理和控制节点，提供服务注册和发现等功能。
  * **tarsnode** 服务节点, 部署在业务节点 实施服务部署和进程管理.
  * **tarstate** 状态服务 接受服务上报的数据,如调用次数,调用时间等.
  * **tarsconfig** 配置服务, 实现配置文件的推送等


# tarsgo
  tarsgo包含的包有
  ### Communicator 
  通信器赋值proxy管理
  ### protocol:
  数据的编解码
  ### transport:
  socket连接和数据收发
  ### tars2go
  jce代码生成器

# tarsgo2taf
  由于taf ->tars 通讯协议并未改变,所以只要知道服务名即可正常调用.

  对于配置文件的解析和框架服务的调用只需要把tars命名空间改为tars即可
  
  而且go同cpp一样编译后为二进制文件,所以在后台部署时选择cpp类型即可由tarsnode启动管理.