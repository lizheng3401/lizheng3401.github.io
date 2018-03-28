---
title: ETCD简介与使用
date: 2018-03-28 15:11:47
tags: 
  - ETCD
categories: 云
---

最近在工作中后端业务需要与etcd数据库进行数据交互，ETCD——高可用的分布式键值（key-value）数据库，由GO语言实现。
以下简单介绍下在这次业务实现中学习的ETCD的基本用法（入门小白级），<!-- more -->

## 下载安装

单机实例安装，即stardard alone形式的安装，这种方式安装后，etcd的client和etcd的server均在同一台机器，便于练习。

etcdgithub仓库的releases页面下载对应的版本，[Github下载地址](https://github.com/coreos/etcd/releases/)

目前最新版本为V3.3.2,V2版本与V3版本差异较大，以下均为V3版本上的操作
从github上下载后解压后，该目录下会有etcd、etcdctl两个可执行文件，cd至该目录下，查看对应版本

```Bash
etcd -version
```
若出现正确版本号，则可以正常使用，启动etcd服务器

```Bash
etcd
```
回车即可

## 基本用法

coreos公司在开发etcd时，预留了多种交互接口，

### etcdctl

etcdctl属于ETCD的客户端，目前V3版本也兼容了V2版本的接口API，故而在使用etcdctl时，必须在命令开头指明所使用的API版本。（PS: 这部分就学了仨命令 QAQ）

```Bash
ETCDCTL_API=3 etcdctl put key1 "A"  //返回OK表示添加key1:'A'键值对成功
ETCDCTL_API=3 etcdctl get key1      //获取key1键的值
ETCDCTL_API=3 etcdctl del key1      //删除key1键值对
```

### REST API 

etcd支持rest风格的接口，可直接利用curl直接与etcd交互

```Bash
 curl http://127.0.0.1:2379/version //查看版本

 //V2版本
 curl http://127.0.0.1:2379/v2/keys/hello -XPUT -d value="world" // 创建键值对（hello:"world"）
 curl http://127.0.0.1:2379/v2/keys/hello //查看hello键的值
 curl http://127.0.0.1:2379/v2/keys/hello -X DELETE //删除hello键值对

//V3版本，注意在V3版本中所有的key和value都必须转换为base64编码然后才可以存储
// foo is 'Zm9v' in Base64
// bar is 'YmFy' in Base64
curl -L http://127.0.0.1:2379/v3beta/kv/put \
	-X POST -d '{"key": "Zm9v", "value": "YmFy"}'
// 创建键值对 foo:bar

curl -L http://127.0.0.1:2379/v3beta/kv/range \
	-X POST -d '{"key": "Zm9v"}' 

// 查看键值对 foo
```

## ETCD事务型操作

ETCD的事务性操作这里以V3的curl方式为例：

```Python2
// python2
// 已在etcd中创建了key1:0,A:"success",B:"failure"三个键值对
import json
import base64
import requests

URL = "http://127.0.0.1:2379/v3beta/kv/%s"

url = URL % "txn"

payload = {
    "compare":[
        {
            "target": "VALUE",
            "key":base64.b64encode("key1"),
            'result': "EQUAL",
            "value": base64.b64encode("0"),
        },
    ],
    "success":[
        {
            "requestRange":{
                "key":base64.b64encode("A"),
            }
        }
    ],
    "failure":[
        {
            "requestRange":{
                "key":base64.b64encode("B"),
            }
        }
    ]
}
resp = requests.post(url,json=payload)
print json.dumps(resp.json(), indent=2)

```
以上的逻辑：
在etcd中判断key1的值是否为0，如果为0则返回A的值，否则返回B的值
compare相当于 If，success相当于then，failure相当于else
即如果compare条件成立，执行success，否则执行failure

1. compare

  compare中的条件结构如下：
  ```JSON
  {
    "target": "VALUE",// (值为以下其中之一 "VALUE"，"CREATE", "MOD", "VERSION")
    "key":base64.b64encode("key1"), // 要比较的键值对的键
    "result": "EQUAL",  .//设置的判断条件，以下几种之一 （EQUAL,GREATER, NOT_EQUAL, LESS, ）
    "value": base64.b64encode("0"), 
    // 这个的键与target对应， 值则为你想要比较的值
    /*
        'VERSION' => 'version',
        'CREATE'  => 'createRevision',
        'MOD'     => 'modRevision',
        'VALUE'   => 'value'
    */
  }
  ```   
  上述的结构可理解为：判断 key（key1）的target（value）和value("0")是否 result（EQUAL）

  每个条件都是一个类似的结构，多个条件之间同时成立，条件才为真，所以，实际上这也是为什么compare是数组的原因

2. success，failure

  这两个结构，用法类似
  ```JSON
  {
      requestRange:{
          "key": base64.b64encode("A"),
      }
  }
  ```
  这其中的requestRange,代表了range这个操作，即查看键值
  内部的值为参数

  有requestRange，requestPut，requestDel，三种操作，
  分类对应于查看，创建，删除

[欢迎访问我的博客了解更多](http://lizheng3401.github.io/2018/03/28/ETCD简介与使用/)





