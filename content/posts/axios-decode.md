---
title: "axios 中文乱码"
tags: ["爬虫"]
date: 2019-04-15
draft: false
---

axios和cheerio配合做爬虫时有时会遇到中文乱码问题
这是由于axios解析请求结果默认使用json格式和uft8编码
```javascript
responseType: 'json',
responseEncoding: 'utf8'
```
有一些网站由于非utf8格式所以会导致中文乱码.
其中一些网站可以根据请求头去调整编码格式,这种只需在axios中添加header `Content-Type:text/html;charset=UTF8`即可解决

另一部分网站并不鸟你的请求头,这种就需要做转码处理.
可以通过 `response headers`的`content-type`或者`document.charset`来确认网站编码格式,然后转为utf8即可.

```javascript
const iconv = require('iconv-lite'); //转码模板
const response = await axios({
    method: 'get',
    responseType: 'arraybuffer', // 设定返回数据的格式
    url:''
  });
  // 国内大部分老站都是 `GBK`
  const data = iconv.decode(response.data, 'GBK');
```



