---
title: ubuntu下安装vue-cli后输入vue提示无法找到vue命令解决办法
date: 2018-03-26 09:48:01
tags:
  - 项目问题记录
  - Vue
categories: Web开发
---

在学习完Vue官网的文档后，准备使用vue-cli来做一个实战,`npm install vue-cli -g`安装后Terminal输入vue却提示无法找到命令
<!-- more -->
## 解决方法
建立软链接
```Bash
$ sudo ln -s 你的vue-cli安装包的路径/bin/vue /usr/local/bin/vue 
```
建立成功，则会在`/usr/local/bin/`目录下生成一个叫vue的文件
然后输入
```Bash
$ vue -V
```
正常输出版本号，问题解决
[欢迎访问我的博客了解更多](http://lizheng3401.github.io/2018/03/26/ubuntu下安装vue-cli后输入vue提示无法找到vue命令解决办法/)