---
layout: post
title:  "elasticsearch5.2.2简要安装"
date:   2017-03-12 19:30:54
categories: elasticsearch
---
### 准备软件：
- jdk-8u121-linux-x64.tar.gz
- elasticsearch-5.2.2.tar.gz
- kibana-5.2.2-linux-x86_64.tar.gz

### 安装jdk8
#### 创建java目录

```
cd /usr
mkdir java
```
##### 解压jdk目录

```
tar xvf jdk-8u121-linux-x64.tar.gz
```
##### 设置环境变量

```
vim /etc/profile
```
```
export JAVA_HOME=/usr/java/jdk1.8.0_121
export PATH=$JAVA_HOME/bin:$PATH
```
##### 测试jdk有没有安装成功

```
java -version
```
返回如下，代表成功

```
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```

### 简单安装es
##### 解压es到opt/es下

```
tar xvf elasticsearch-5.2.2.tar.gz
```
##### 然后设置es5.2的环境变量

```
vim /etc/profile
```

```
export ES_HOME=/opt/es/elasticsearch-5.2.2
```
根据官方资料，为保证ES的安全性，不可以root身份启动ES,当使用root的时候，需要切换用户。自身本机为ubuntu，启动脚本如下：

```
vim start_es.sh
```

```
#!/bin/bash
$ES_HOME/bin/elasticsearch -d &
tail -f $ES_HOME/logs/elastic_search.log
```
##### 简要配置如下

```
cluster.name: elastic_search
node.name: node1
network.host: 127.0.0.1
http.port: 9200
```
详细配置可参考es的官方文档，生产环境比这个复杂的多。

启动es之后，本机效果

```
curl 127.0.0.1:9200
```
```
{
  "name" : "node1",
  "cluster_name" : "elastic_search",
  "cluster_uuid" : "pbZEHRJsTG2wgOwB9AYspQ",
  "version" : {
    "number" : "5.2.2",
    "build_hash" : "f9d9b74",
    "build_date" : "2017-02-24T17:26:45.835Z",
    "build_snapshot" : false,
    "lucene_version" : "6.4.1"
  },
  "tagline" : "You Know, for Search"
}

```
### 安装kibana
##### 解压kibana到opt/kibana下

```
tar xvf kibana-5.2.2-linux-x86_64.tar.gz
```
然后编辑启动脚本

```
vim start_kb.sh
```

```
#!/bin/bash
/opt/kibana/kibana-5.2.2-linux-x86_64/bin/kibana &
```
另外，kibana的简单配置如下，详细配置请参考官方文档

```
server.port: 5601
server.host: "127.0.0.1"
elasticsearch.url: "http://127.0.0.1:9200"
```
启动脚本，另 以前的sense插件现在已经集成在kibana上面了，打开localhost：5601界面，其中 DEV TOOLS就是以前的sense。

关于权限部分还有关闭脚本省略，其他详细的后续再补充。












