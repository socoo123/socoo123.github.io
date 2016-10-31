---
layout: post
title:  "ElasticSearch备份与还原"
date:   2016-09-06 19:22:54
categories: elasticsearch
---
## ElasticSearch备份与还原 ##
1.elasticsearch.yml添加path.repo路径配置：

```
path.repo: ["/opt/index_data_backup"]
```

2.重启es，若存在就不用重启了

3.修改备份文件的属性

```
mkdir -p /opt/index_data_backup/my_backup
chmod 777 /opt/index_data_backup/my_backup
chown -R es:es /opt/index_data_backup/my_backup
```
环境共享
安装 sshfs
使用yum去安装

下面这两个命令是如果共享目录错了解除共享的
```
fusermount -u YOUR_MNT_DIR
sudo umount -l YOUR_MNT_DIR
```

暴力一些，在其中一台机器上面设置一下共享目录，比如我选中了10.200.2.95这台机器。

```
sudo mkdir -p /opt/my_data_backup/es_backup
chmod 777 /opt/my_data_backup/es_backup
```
建立共享，在每个节点上面都使用这个命令

```
sudo sshfs lvmama_admin@10.200.2.95:/opt/my_data_backup/es_backup/ /opt/index_data_backup/my_backup -o allow_other
```

4.创建repository

```
PUT _snapshot/my_backup 
{
    "type": "fs", 
    "settings": {
        "location": "/opt/index_data_backup/my_backup" 
    }
}
```

max_snapshot_bytes_per_sec:默认20M每秒的速度，可修改
max_restore_bytes_per_sec：默认20M每秒的速度，可修改
```
POST _snapshot/my_backup/ 
{
    "type": "fs",
    "settings": {
        "location": "/mount/backups/my_backup",
        "max_snapshot_bytes_per_sec" : "50mb", 
        "max_restore_bytes_per_sec" : "50mb"
    }
}
```


查看repository信息

```
GET _snapshot/my_backup/ 
```

5.创建快照

```
PUT _snapshot/my_backup/snapshot_1
```
同步执行的话，索引很大会比较慢：

```
PUT _snapshot/my_backup/snapshot_1?wait_for_completion=true
```
只备份部分索引，我们就使用这个：

```
PUT _snapshot/my_backup/snapshot_2
{
    "indices": "index_1,index_2"
}
```

查看备份情况

```
GET _snapshot/my_backup/snapshot_2
```

删除镜像，备份

```
DELETE _snapshot/my_backup/snapshot_2
DELETE _snapshot/my_backup
```
观察备份进程

```
GET _snapshot/my_backup/snapshot_3
GET _snapshot/my_backup/snapshot_3/_status
```

镜像恢复

```
POST _snapshot/my_backup/snapshot_1/_restore
```

指定索引恢复

```
POST /_snapshot/my_backup/snapshot_1/_restore
{
    "indices": "index_1", 
    "rename_pattern": "index_(.+)", 
    "rename_replacement": "restored_index_$1" 
}
```
rename_pattern 和 rename_replacement 是利用正则来重命名索引








