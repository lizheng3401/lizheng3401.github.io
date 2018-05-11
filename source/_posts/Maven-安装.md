---
title: Maven 安装
date: 2018-05-11 22:52:16
tags:
  - Maven
categories: 后端开发
---
本文记录了Window10上安装Maven

<!-- more -->

## 下载Maven

[Maven下载地址](https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.zip)
进入该页面下载Maven，完成后解压

## 配置环境变量

打开系统属性面板（右击“计算机”>"属性"），单击高级系统设置，再单击环境变量，在系统变量中新建一个变量，变量名为M2_HOME，变量值为Maven的安装目录，如图：
![Maven安装环境变量配置](img/2018051101.png)

然后添加`Maven bin` 文件夹到 `PATH` 的最后，如： `%M2_HOME%\bin;` 如图:
![Maven配置bin目录](img/2018051102.png)

## 验证安装

打开CMD，输入`mvn -version`,出现如下内容表明安装成功
![验证配置结果](img/2018051103.png)

## Maven本地仓库和远程仓库配置

Maven的本地仓库是用来保存所有的项目依赖的文件的，默认情况下，Maven的本地仓库默认为系统用户的 .m2 目录文件夹。故而如果使用默认本地仓库地址，随着各类的项目依赖不断增加，会导致占用许多C盘空间，所以一般会修改默认的本地仓库的保存位置。

首先，在`Maven`的文件夹下的`conf`文件夹中存在一个`settings.xml`
打开该文件，找到如图所示的所在行，如图所示：
![Maven的settings.xml的配置文件](img/2018051104.png)
按照划线部分的示例，添加自定义的本地仓库的位置
![Maven的settings.xml文件修改](img/2018051105.png)

[欢迎访问我的博客了解更多](http://lizheng3401.github.io/2018/05/11/Maven-安装/)

