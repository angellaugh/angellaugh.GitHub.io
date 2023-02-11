---
layout: posts
title: ES
date: 2023-02-11 20:37:46
tags: ES
categories: Teckknowledge
---

sudo zypper refresh
sudo zypper update -y
sudo zypper install yum


### Import the Elasticsearch GPG Key
`sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch`


### Install Elasticsearch from the RPM repository

create elasticsearch.repo and config the es install repo

`sudo vim /etc/zypp/repos.d/elasticsearch.repo`


<!--more-->

```
[elasticsearch]
name=Elasticsearch repository for 8.x packages
baseurl=https://artifacts.elastic.co/packages/8.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md

```

install ES
```
sudo zypper modifyrepo --enable elasticsearch && \
  sudo zypper install elasticsearch; \
  sudo zypper modifyrepo --disable elasticsearch 
```

download and install RPM 
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.5.2-x86_64.rpm
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.5.2-x86_64.rpm.sha512
shasum -a 512 -c elasticsearch-8.5.2-x86_64.rpm.sha512 
sudo rpm --install elasticsearch-8.5.2-x86_64.rpm
```

config the es start automatically when system boots up
```
ES-suse:/etc/zypp/repos.d # sudo /bin/systemctl daemon-reload
ES-suse:/etc/zypp/repos.d # sudo /bin/systemctl enable elasticsearch.service
Created symlink /etc/systemd/system/multi-user.target.wants/elasticsearch.service → /usr/lib/systemd/system/elasticsearch.service.

```


check the ES
```
ES-suse:/etc/zypp/repos.d # rpm -qi elasticsearch
Name        : elasticsearch
Epoch       : 0
Version     : 7.17.7
Release     : 1
Architecture: x86_64
Install Date: Sun Dec  4 21:37:47 2022
Group       : Application/Internet
Size        : 525810397
License     : Elastic License
Signature   : RSA/SHA512, Mon Oct 17 14:57:09 2022, Key ID d27d666cd88e42b4
Source RPM  : elasticsearch-7.17.7-1-src.rpm
Build Date  : Mon Oct 17 11:35:59 2022
Build Host  : packer-virtualbox-iso-1646848364
Relocations : /usr
Packager    : Elasticsearch
Vendor      : Elasticsearch
URL         : https://www.elastic.co/
Summary     : Distributed RESTful search engine built for the cloud
Description :
Reference documentation can be found at
  https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html
  and the 'Elasticsearch: The Definitive Guide' book can be found at
  https://www.elastic.co/guide/en/elasticsearch/guide/current/index.html
Distribution: Elasticsearch

```

start es server

```
ES-suse:/etc/zypp/repos.d # sudo systemctl start elasticsearch
ES-suse:/etc/zypp/repos.d # sudo systemctl status elasticsearch
● elasticsearch.service - Elasticsearch
     Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; disabled; vendor preset: disabled)
     Active: active (running) since Sun 2022-12-04 21:39:35 EST; 21s ago
       Docs: https://www.elastic.co
   Main PID: 8555 (java)
      Tasks: 97 (limit: 4915)
     CGroup: /system.slice/elasticsearch.service
             ├─ 8555 /usr/share/elasticsearch/jdk/bin/java -Xshare:auto -Des.networkaddress.cache.ttl=60 -Des.networkaddress.cache.negative.ttl=10 -XX:+Always>
             └─ 8767 /usr/share/elasticsearch/modules/x-pack-ml/platform/linux-x86_64/bin/controller

Dec 04 21:39:17 ES-suse systemd[1]: Starting Elasticsearch...
Dec 04 21:39:35 ES-suse systemd[1]: Started Elasticsearch.

```

related log:
` vi /var/log/elasticsearch/elasticsearch.log`


datapath
`/var/lib/elasticsearch`

To list journal entries for the elasticsearch service:

`sudo journalctl --unit elasticsearch`


verify es installation
1. `ss -antpl | grep 9200`
2. `curl -X GET "localhost:9200/"`

```
ES-suse:/etc/zypp/repos.d # ss -antpl | grep 9200
LISTEN 0      4096   [::ffff:127.0.0.1]:9200             *:*    users:(("java",pid=8555,fd=305))
LISTEN 0      4096                [::1]:9200          [::]:*    users:(("java",pid=8555,fd=304))
ES-suse:/etc/zypp/repos.d # curl -X GET "localhost:9200/"
{
  "name" : "ES-suse",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "0zL15OxVTY6SrXO2aQ6qGQ",
  "version" : {
    "number" : "7.17.7",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "78dcaaa8cee33438b91eca7f5c7f56a70fec9e80",
    "build_date" : "2022-10-17T15:29:54.167373105Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

```

input one data:
```
 curl -H 'Content-Type: application/json' -X POST 'http://localhost:9200/todo/task/1' -d '{ "name": "Go to the mall." }'
```
and read it again:
```
ES-suse:/etc/zypp/repos.d # curl -X GET 'http://localhost:9200/todo/task/1?pretty'
{
  "_index" : "todo",
  "_type" : "task",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Go to the mall."
  }
}

```










 curl -XGET 'http://localhost:9200/_nodes/stats?pretty' | grep thread
```
}ES-suse:/etc/zypp/repos.d #  curl -XGET 'http://localhost:9200/_nodes/stats?pretty' | grep thread
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 39463  100 39463    0     0  1144k      0 --:--:-- --:--:--        "threads" : {
 --:--:--      "thread_pool" : {
 1          "threads" : 0,
16          "threads" : 0,
7          "threads" : 0,
k          "threads" : 0,

          "threads" : 0,
          "threads" : 1,
          "threads" : 0,
          "threads" : 11,
          "threads" : 2,
          "threads" : 0,
          "threads" : 2,
          "threads" : 0,
          "threads" : 0,
          "threads" : 1,
          "threads" : 3,
          "threads" : 0,
          "threads" : 0,
          "threads" : 0,

```



```

ES-suse:/etc # ll /usr/share/elasticsearch/bin
total 3028
-rwxr-xr-x 1 root root     101 Nov 17 14:04 elasticsearch
-rwxr-xr-x 1 root root     376 Nov 17 13:59 elasticsearch-certgen
-rwxr-xr-x 1 root root     376 Nov 17 13:59 elasticsearch-certutil
-rwxr-xr-x 1 root root     674 Nov 17 14:04 elasticsearch-cli
-rwxr-xr-x 1 root root     353 Nov 17 13:59 elasticsearch-create-enrollment-token
-rwxr-xr-x 1 root root     352 Nov 17 13:59 elasticsearch-croneval
-rwxr-xr-x 1 root root    2340 Nov 17 14:04 elasticsearch-env
-rwxr-xr-x 1 root root    2595 Nov 17 14:04 elasticsearch-env-from-file
-rwxr-xr-x 1 root root      84 Nov 17 14:04 elasticsearch-geoip
-rwxr-xr-x 1 root root      87 Nov 17 14:04 elasticsearch-keystore
-rwxr-xr-x 1 root root      55 Nov 17 14:04 elasticsearch-node
-rwxr-xr-x 1 root root     172 Nov 17 14:04 elasticsearch-plugin
-rwxr-xr-x 1 root root     376 Nov 17 13:59 elasticsearch-reconfigure-node
-rwxr-xr-x 1 root root     353 Nov 17 13:59 elasticsearch-reset-password
-rwxr-xr-x 1 root root     353 Nov 17 13:59 elasticsearch-saml-metadata
-rwxr-xr-x 1 root root     353 Nov 17 13:59 elasticsearch-service-tokens
-rwxr-xr-x 1 root root     353 Nov 17 13:59 elasticsearch-setup-passwords
-rwxr-xr-x 1 root root      55 Nov 17 14:04 elasticsearch-shard
-rwxr-xr-x 1 root root     403 Nov 17 13:59 elasticsearch-sql-cli
-rwxr-xr-x 1 root root 3008975 Nov 17 13:59 elasticsearch-sql-cli-8.5.2.jar
-rwxr-xr-x 1 root root     353 Nov 17 13:59 elasticsearch-syskeygen
-rwxr-xr-x 1 root root     353 Nov 17 13:59 elasticsearch-users
-rwxr-xr-x 1 root root     332 Nov 17 14:01 systemd-entrypoint

```





### health check
```
curl 'localhost:9200/_cat/health?v'
```

### nodes list
```
curl 'localhost:9200/_cat/nodes?v'
```

### index list
```
curl 'localhost:9200/_cat/indices?v'
```

### search detail data using index

`curl -X GET http://localhost:9200/je_index_1663720218/`

```
root@powerload-es-2:~ # curl -X GET http://localhost:9200/je_index_1663720218/_search?pretty
{
  "took" : 7,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 0,
    "max_score" : null,
    "hits" : [ ]
  }
}
```

# 查看集群全部节点的指标
$ curl "localhost:9200/_nodes/stats"







--------------------------------------------------------------------------

Issue1

```
ES-suse:/etc/zypp/repos.d # sudo systemctl enable elasticsearch
Synchronizing state of elasticsearch.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing: /usr/lib/systemd/systemd-sysv-install enable elasticsearch
ln -sf ../elasticsearch /etc/init.d/rc2.d/S50elasticsearch
ln: failed to create symbolic link '/etc/init.d/rc2.d/S50elasticsearch': No such file or directory
```

It seems that this product is not from the standard openSUSE OSS repo.

It tries to use /etc/init.d/rc2.d which directory belongs to the old SystemV ways of doing (as the message also says). but in openSUSE that directory is no longer in use and most probably not available anymore.


Issue2: refused了

root@ldm-controller-01:/usr/local/stress_test/Island_TPPC # curl -X GET "http://10.219.61.175:9200/_cat/indices?v"
curl: (7) Failed connect to 10.219.61.175:9200; Connection refused


---------------------------------------Dry run es-opensuse on powerload------------------------
Config ES-suse to powerload json file

```
root@ldm-controller-01:/usr/local/stress_test/Island_TPPC # nslookup ES-suse.west.isilon.com
Server:         10.231.252.14
Address:        10.231.252.14#53

Non-authoritative answer:
Name:   ES-suse.west.isilon.com
Address: 10.219.61.175
```

![](vx_images/65815239672.png =800x)


### cat methods

`curl http://localhost:9200/_cat`

ES-suse:/var/lib/elasticsearch #  curl http://localhost:9200/_cat
=^.^=
/_cat/allocation
/_cat/shards
/_cat/shards/{index}
/_cat/master
/_cat/nodes
/_cat/tasks
/_cat/indices
/_cat/indices/{index}
/_cat/segments
/_cat/segments/{index}
/_cat/count
/_cat/count/{index}
/_cat/recovery
/_cat/recovery/{index}
/_cat/health
/_cat/pending_tasks
/_cat/aliases
/_cat/aliases/{alias}
/_cat/thread_pool
/_cat/thread_pool/{thread_pools}
/_cat/plugins
/_cat/fielddata
/_cat/fielddata/{fields}
/_cat/nodeattrs
/_cat/repositories
/_cat/snapshots/{repository}
/_cat/templates
/_cat/ml/anomaly_detectors
/_cat/ml/anomaly_detectors/{job_id}
/_cat/ml/trained_models
/_cat/ml/trained_models/{model_id}
/_cat/ml/datafeeds
/_cat/ml/datafeeds/{datafeed_id}
/_cat/ml/data_frame/analytics
/_cat/ml/data_frame/analytics/{id}
/_cat/transforms
/_cat/transforms/{transform_id}



https://juejin.cn/post/7145773678649671694

curl localhost:9200/_cat/nodes?help
# 指定查看每个节点的堆内存使用率，段数量和合并数量
$ curl "http://localhost:9200/_cat/nodes?h=http,heapPercent,segmentsCount,mergesTotal"
172.16.71.231:9200 56 99 108182
172.16.71.229:9200 31 95 122551
172.16.71.232:9200 50 66  73871
172.16.71.230:9200 41 63  76470
172.16.71.234:9200 32 64  93256
172.16.71.233:9200 14 90 136450


当集群为red或者yellow的时候怎么办
集群为RED表示集群中有primary shard没有分配，yellow表示有replica没有分配

root@powerload-es-2:/var/lib/elasticsearch/nodes # curl -X GET "http://localhost:9200/_cluster/allocation/explain?pretty"
{
  "index" : "je_index_1664986024",
  "shard" : 3,
  "primary" : false,
  "current_state" : "unassigned",
  "unassigned_info" : {
    "reason" : "CLUSTER_RECOVERED",
    "at" : "2022-12-05T02:18:25.712Z",
    "last_allocation_status" : "no_attempt"
  },
  "can_allocate" : "no",
  "allocate_explanation" : "cannot allocate because allocation is not permitted to any of the nodes",
  "node_allocation_decisions" : [
    {
      "node_id" : "R2DFntg7RLiwYsEN4mwYuw",
      "node_name" : "R2DFntg",
      "transport_address" : "10.219.61.247:9300",
      "node_attributes" : {
        "ml.machine_memory" : "16656269312",
        "xpack.installed" : "true",
        "ml.max_open_jobs" : "20",
        "ml.enabled" : "true"
      },
      "node_decision" : "no",
      "deciders" : [
        {
          "decider" : "same_shard",
          "decision" : "NO",
          "explanation" : "the shard cannot be allocated to the same node on which a copy of the shard already exists [[je_index_1664986024][3], node[R2DFntg7RLiwYsEN4mwYuw], [P], s[STARTED], a[id=488Lf_8lTI2raran4YNWoQ]]"
        }
      ]
    }
  ]
}


分片长时间处于未分配状态 ES内部会对一个unassigned 分片尝试5次进行分配,失败后不再尝试进行分配，这时候需要调用进行手动控制集群处理 unassigned 分片：







```
Before starting Elasticsearch for the first time
Set xpack.security.enabled: false and then start Elasticsearch

After you have started Elasticsearch for the first time
You have two options here

Set

xpack.security.enabled: false
xpack.security.transport.ssl.enabled: false
xpack.security.http.ssl.enabled: false
and then start Elasticsearch.

Run

bin/elasticsearch-keystore remove xpack.security.transport.ssl.keystore.secure_password
bin/elasticsearch-keystore remove xpack.security.transport.ssl.truststore.secure_password
bin/elasticsearch-keystore remove xpack.security.http.ssl.keystore.secure_password
Set

xpack.security.enabled: false
```


### change timezone

step1: vi /etc/sysconfig/clock
```
DEFAULT_TIMEZONE="Etc/GMT"
ZONE="Etc/GMT"
~
```  
step2: zic -l Etc/GMT
step3: date

```
ES-suse:/var/log/elasticsearch # date
Tue Dec  6 03:46:47 EST 2022
ES-suse:/var/log/elasticsearch # vi /etc/sysconfig/clock
ES-suse:/var/log/elasticsearch # zic -l Etc/GMT
ES-suse:/var/log/elasticsearch # date
Tue Dec  6 08:55:46 GMT 2022

```


