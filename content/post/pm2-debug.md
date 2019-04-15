---
title: "pm2配置启动文件"
tags: ["pm2"]
date: 2019-04-11
draft: false
---

pm2启动项目可以使用命令行模式如 `pm2 start app.js --watch`
或者是提供一个启动文件 文件可以是json或者node模板的.js文件
json格式的文件名没有要求,但是js文件要使用.config.js结尾
不然pm2会把这个文件当成入口脚本,会不断重启,日志上只能看到
```
App [appname:pid] exited with code [0] via signal [SIGINT]
```
具体配置文件规则可以看下面这段源码
```javascript
/**
 * Check if filename is a configuration file
 * @param {string} filename
 * @return {mixed} null if not conf file, json or yaml if conf
 */
Common.isConfigFile = function (filename) {
  if (typeof (filename) !== 'string')
    return null;
  if (filename.indexOf('.json') !== -1)
    return 'json';
  if (filename.indexOf('.yml') > -1 || filename.indexOf('.yaml') > -1)
    return 'yaml';
  if (filename.indexOf('.config.js') !== -1)
    return 'js';
  if (filename.indexOf('.config.mjs') !== -1)
    return 'mjs';
  return null;
};
```

