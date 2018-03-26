---
title: 利用Hexo+Github搭建个人博客
date: 2018-03-25 14:41:04
tags:
  - 项目问题记录
  - Hexo
categories: Web开发
---
本示例都是在Windows10环境下配置
<!--more -->

## 环境配置

1.Node.js
2.Git

## 创建过程

### 创建仓库

登陆Github网站，新建一个仓库，仓库名使用 `你的GitHub帐户名/github.io`

### 本地创建hexo的Blog项目

首先利用npm安装hexo插件：
```Bash
npm install hexo -g
```
然后创建一个放置你的博客项目的文件夹，这里以Blog为文件夹名称，CD进入该文件夹后，进行hexo项目的初始化
```Bash
hexo init
npm install
```
待所有依赖安装完成之后，Blog下会生成如下的目录结构
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes

- _config.yml
  这是你的博客的整个项目的站点的配置文件
- package.json
  这是你的博客项目的整个的node依赖文件，记录了依赖有关的npm包的信息
- scaffolds
  这个是博客文章的模板
- source
  这个是存放各类博客的文件夹，其中`_drafts`表示草稿，`_posts`是发布到站点的文章
- theme
  这个是整个站点的主题文件夹
### 本地运行查看你的个人网站

在Blog文件夹下启动hexo自带的服务器，然后浏览器中访问http://localHost:4000查看博客效果
```Bash
hexo s --debug
```

### 新建文章

在Blog目录下输入如下命令，hexo会在`source/_posts`目录下生成对应的Markdown文件
```Bash
hexo new "你的博客题目"
```
然后刷新浏览器即可看到对应的文章

### 将你的hexo项目发布至Github进行部署

在_config.yml中找到如下配置项：
```yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: 
  repository: 
  branch: 
```
由于我们使用的是github，故而对应项如下填写(注意':'符号后必须有空格)
```yml
type: git
repository: git@github.com:你的GitHub帐户名/你的GitHub帐户名.github.io.git
branch: master
```
正确填写配置之后，利用npm安装hexo-deployer-git插件，该插件可以将你的就可以将自己的博客项目上传至Github上。
```Bash
npm install hexo-deployer-git --save
```
安装完成之后，在Blog目录下输入如下命令上传至Github，
```Bash
hexo d -g
```
Github会自动帮你进行各种配置，然后就可以访问`http://你的GitHub帐户名.github.io`查看的你的博客了，注意当上传成功之后，Github的配置需要一定的时间，立即访问可能无法成功

## 个性化配置