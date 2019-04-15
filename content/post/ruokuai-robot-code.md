---
title: "使用若快解决爬虫验证码问题"
tags: ["爬虫"]
date: 2019-04-16
draft: true
---
使用node写爬虫除了axios+cheerio这种外,我更喜欢puppeteer这种headless
浏览器,因为这样你只要关注你关心的内容在哪个dom节点即可,不用操心ajax, token cookie等等. 在表单和验证码处理方面也更友好.~~小声bb我对headless翻译中无头十分不满意,我更喜欢无界这个翻译~~
爬虫处理验证码很麻烦,接入打码平台无异是最简单的手段,下面以若快为例讲下如何处理hdcmct的登陆验证码

[](https://github.com/enhay/pt/blob/master/lib/ruokuai.js)