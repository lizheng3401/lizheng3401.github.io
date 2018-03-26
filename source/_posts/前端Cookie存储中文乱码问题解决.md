---
title: 前端Cookie存储中文乱码问题解决
date: 2018-03-24 22:17:20
tags: 
  - 项目问题记录
  - Cookie
categories: Web开发
---
前端使用Cookie在存储用户名等中文字符串时候，当从Cookie中取出时，会出现乱码
<!--more-->

## 问题解释

Cookie在存储中文时会将其转换为16进制，故而当从Cookie中取出时，取出的实际上时16进制的字符串

## 问题解决

利用`unescape()`可以将Cookie中取出的字符串转换为对应的中文字符

[欢迎访问我的博客了解更多](http://lizheng3401.github.io/2018/03/24/前端Cookie存储中文乱码问题解决/)