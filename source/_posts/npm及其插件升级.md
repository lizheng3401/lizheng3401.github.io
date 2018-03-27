---
title: npm及其插件升级
date: 2018-03-27 23:10:23
tags: 
  - 项目问题记录
  - npm
categories: Web开发
---

今天在新建Vue项目项目的时候，发现Vue在命令行中提示有最新版本，于是学会了如何升级npm及其插件的方法：
<!-- more -->

1. 升级Npm

```bash
npm install -g npm
```

2. 全局升级插件（以Vue-cli为例）

```Bash
npm update vue -g
```

3. 局部升级（项目内部升级）

```Bash
npm update axios --save
```

