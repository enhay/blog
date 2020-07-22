---
title: "使用athens搭建GOPROXY"
tags: ["goproxy"]
date: 2019-05-09
draft: false
---

## GOPROXY
GOPROXY是允许用户控制从哪里下载go modules 对于国内用户来说第一大好处就是
可以设置一个统一的代理服务来解决包无法下载的问题,其次,如果和gitlab ci/cd流程结合进行线上打包,一个内网的modules仓库远比外网速度要快很多.下面使用athens演示如何使用docker搭建一个本地代理
### docker部署athens
首先假设你有本地代理http://127.0.0.1:8118
```bash
export ATHENS_STORAGE=/data/anthens #你喜欢就好
docker run -d -v $ATHENS_STORAGE:/var/lib/athens \
   -e ATHENS_DISK_STORAGE_ROOT=/var/lib/athens \
   -e ATHENS_STORAGE_TYPE=disk \
   --name athens-proxy \
   --restart always \
   -p 3001:3000 \
   --env HTTP_PROXY="http://127.0.0.1:8118" \ #让这个container链接走本地代理
   gomods/athens:latest

```
最后设置环境变量即可
```bash
export GOPROXY="http://127.0.0.1:3001"
```
当然对于公司开发来发来说最好是找OPS内网部署一套再做个域名解析岂不美哉!