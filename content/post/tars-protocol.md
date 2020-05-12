---
title: "tars proto"
tags: ["tars"]
date: 2019-06-25
draft: false
---

tars协议是是IDL语言的一种实现,对于不熟悉IDL的同学来(比如我)可能写IDL的就是第一个迈不出去的坎,基础类型可以参考如下图

![基础类型](https://raw.githubusercontent.com/TarsCloud/Tars/master/docs/images/tars_tarsproto.png)

IDL用module来区分不同模块,如果我模块B依赖模块A中定义的结构,假设模板A,B存在两个单独的文件中, 可以在模板B中通过如下方式引用
```
#include "./modulea.tars"
module b
{
    struct B
    {
        0 optional int id
    };
    
    interface Servant
    {
        int UseOtherModuleStruct(modulea:Struct1 tReq, out B tRsp);
    };
};

```
