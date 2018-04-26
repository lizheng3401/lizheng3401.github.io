---
title: Docker Redis的官方镜像简单使用
date: 2018-04-26 17:25:59
tags:
 - Redis
categories: 后端开发
---

Redis是一种键值对形式的分布式缓存数据库
<!--more -->


## 拉取镜像

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

首先查看redis有没有配置临时密码，无密码会返回这个
```Bash
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) ""
```

然后设置密码， 1234
```Bash
127.0.0.1:6379> config set requirepass 1234
```

再次查看当前redis就提示需要密码：
```Bash
127.0.0.1:6379> config get requirepass
(error) NOAUTH Authentication required.
```

## python的redis数据库连接——插件库(redis)

### 数据库连接，默认执行前后数据库连接然后释放连接
```Bash
r = redis.Redis(host='0.0.0.0', port=6379, db=0, password="1234")
r.set('name', 'test')
print(r.get('name'))
```

### 数据库连接池配置
```Bash
pool = redis.ConnectionPool(host="0.0.0.0", port=6379, db=0, password="1234")
r = redis.Redis(connection_pool=pool)
r.set("age", "16")
r.get("age")
```

### 数据库的事务性操作

redis默认在执行每次请求都会创建（连接池申请链接）和断开（归还连接池）一次连接操作，如果想要再一次请求中指定多个命令，则可以使用pipline实现一次请求指定多个命令，并且默认情况下一次pipline是原子性操作。

```Bash
pool = redis.ConnectionPool(host='0.0.0.0', port=6379)
r = redis.Redis(connection_pool=pool)
pipe = r.pipeline(transaction=True)
r.set('name', 'python')
r.set('age', '18')
pipe.execute()
```
[欢迎访问我的博客了解更多]((http://lizheng3401.github.io/2018/04/26/Redis的入门/)