---
title: Docker Redis的官方镜像简单使用
date: 2018-04-26 17:25:59
tags:
 - Redis
categories: 后端开发
---

Redis是一种键值对形式的分布式缓存数据库
<!--more -->


## 拉去镜像

在docker-compose.yml文件中添加如下配置:
```yaml
redis:
 image: redis
 ports:
  - "6379:6379"
```
启动docker-compose
```
dokcer-compose up
```
`dokcer-compose`会自动从云端拉取redis的镜像,由于大天朝的部分原因(QAQ),下载通常会失败,更换`Docker中国`官方镜像源:
修改/etc/docker/daemon.json文件,没有就新建一个
```Bash
vi /etc/docker/daemon.json
```
添加如下:
```JSON
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```
你会发现速度超快,我在公司实测都是MB/S级别的

## 进入Docker容器的redis的客户端

下载完成后查看docker`docker ps -a`,发现已正常启动,查看其容器实例的ID

然后进入Docker容器redis的客户端
```Bash
docker exec -it 容器ID redis-cli 
```
然后终端会呈现如下交互式环境,证明成功进入
```
127.0.0.1:6379> 
```
然后依据官方的[Interactive tutorial](http://try.redis.io/)就可以开始愉快的玩耍了~\(≧▽≦)/~
这里就不在重述
## redis配置临时密码

```Bash

```





