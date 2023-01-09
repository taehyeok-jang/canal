[![build status](https://travis-ci.com/alibaba/canal.svg?branch=master)](https://travis-ci.com/alibaba/canal)
[![codecov](https://codecov.io/gh/alibaba/canal/branch/master/graph/badge.svg)](https://codecov.io/gh/alibaba/canal)
![maven](https://img.shields.io/maven-central/v/com.alibaba.otter/canal.svg)
![license](https://img.shields.io/github/license/alibaba/canal.svg)
[![average time to resolve an issue](http://isitmaintained.com/badge/resolution/alibaba/canal.svg)](http://isitmaintained.com/project/alibaba/canal "average time to resolve an issue")
[![percentage of issues still open](http://isitmaintained.com/badge/open/alibaba/canal.svg)](http://isitmaintained.com/project/alibaba/canal "percentage of issues still open")

## Introduction

![](https://img-blog.csdnimg.cn/20191104101735947.png)

**Canal [kə'næl]**, translated as waterway/pipe/ditch, the main purpose is to provide incremental data subscription and consumption based on MySQL database incremental log parsing

In the early days, due to the deployment of dual computer rooms in Hangzhou and the United States, there was a business requirement for cross-computer room synchronization. The implementation method was mainly based on business triggers to obtain incremental changes. Since 2010, the business has gradually tried to parse database logs to obtain incremental changes for synchronization, and a large number of incremental database subscription and consumption businesses have been derived from this.

Services based on log incremental subscription and consumption include
- Database mirroring
- Database real-time backup
- Index construction and real-time maintenance (split heterogeneous index, inverted index, etc.)
- Business cache refresh
- Incremental data processing with business logic

The current canal supports source MySQL versions including 5.1.x , 5.5.x , 5.6.x , 5.7.x , 8.0.x

## Working principle
#### MySQL master and backup replication principle
![](![mysql_master_slave_replication](https://user-images.githubusercontent.com/31732943/211264721-878603fd-39d8-4940-bb2e-199cd78f867b.png)
- MySQL master writes data changes to the binary log (binary log, where the records are called binary log events, which can be viewed through show binlog events)
- MySQL slave (I/O thread) receives the master's binary logs and copies to its relay log
- MySQL slave (SQL thread) replays events in the relay log, reflecting data changes to its own data

#### How canal works
- Canal simulates the interactive protocol of MySQL slave, pretends to be MySQL slave, and sends dump protocol to MySQL master
- MySQL master receives dump request and starts to push binary log to slave (ie canal)
- Canal parse binary log object (originally byte stream)

## Important version update instructions

1. Canal 1.1.x version ([release_note](https://github.com/alibaba/canal/releases)), has a big breakthrough in performance and function, and important improvements include:

- Overall performance test & optimization, improved by 150%. #726 Reference: [Performance](https://github.com/alibaba/canal/wiki/Performance)
- Native support for prometheus monitoring #765 [Prometheus QuickStart](https://github.com/alibaba/canal/wiki/Prometheus-QuickStart)
- Native support for kafka message delivery #695 [Canal Kafka/RocketMQ QuickStart](https://github.com/alibaba/canal/wiki/Canal-Kafka-RocketMQ-QuickStart)
- Natively supports binlog subscription of aliyun rds (solves automatic master/standby switching/oss binlog offline parsing) Reference: [Aliyun RDS QuickStart](https://github.com/alibaba/canal/wiki/aliyun-RDS-QuickStart)
- Native support for docker images #801 Reference: [Docker QuickStart](https://github.com/alibaba/canal/wiki/Docker-QuickStart)

2. Canal version 1.1.4 ushers in the most important WebUI capability, introduces the canal-admin project, supports WebUI-oriented canal dynamic management capabilities, and supports online white screen operation and maintenance capabilities such as configuration, tasks, and logs. Specific documents: [Canal Admin Guide](https://github.com/alibaba/canal/wiki/Canal-Admin-Guide)

## Document

- [Home](https://github.com/alibaba/canal/wiki/Home)
- [Introduction](https://github.com/alibaba/canal/wiki/Introduction)
- [QuickStart](https://github.com/alibaba/canal/wiki/QuickStart)
  - [Docker QuickStart](https://github.com/alibaba/canal/wiki/Docker-QuickStart)
  - [Canal Kafka/RocketMQ QuickStart](https://github.com/alibaba/canal/wiki/Canal-Kafka-RocketMQ-QuickStart")
  - [Aliyun RDS for MySQL QuickStart](https://github.com/alibaba/canal/wiki/aliyun-RDS-QuickStart)
  - [Prometheus QuickStart](https://github.com/alibaba/canal/wiki/Prometheus-QuickStart)
- Canal Admin
  - [Canal Admin QuickStart](https://github.com/alibaba/canal/wiki/Canal-Admin-QuickStart)
  - [Canal Admin Guide](https://github.com/alibaba/canal/wiki/Canal-Admin-Guide)
  - [Canal Admin ServerGuide](https://github.com/alibaba/canal/wiki/Canal-Admin-ServerGuide)
  - [Canal Admin Docker](https://github.com/alibaba/canal/wiki/Canal-Admin-Docker)  
- [Admin Guide](https://github.com/alibaba/canal/wiki/AdminGuide)
- [Client Example](https://github.com/alibaba/canal/wiki/ClientExample)
- [Client API](https://github.com/alibaba/canal/wiki/ClientAPI)
- [Performance](https://github.com/alibaba/canal/wiki/Performance)
- [Dev Guide](https://github.com/alibaba/canal/wiki/DevGuide)
- [Binlog Change(MySQL 5.6)](https://github.com/alibaba/canal/wiki/BinlogChange%28mysql5.6%29)
- [Binlog Change(MariaDB)](https://github.com/alibaba/canal/wiki/BinlogChange%28MariaDB%29)
- [TableMeta TSDB](https://github.com/alibaba/canal/wiki/TableMetaTSDB)
- [Release Notes](http://alibaba.github.com/canal/release.html)
- [Download](https://github.com/alibaba/canal/releases)
- [FAQ](https://github.com/alibaba/canal/wiki/FAQ)
- MySQL
  - [Binary Log](https://dev.mysql.com/doc/refman/8.0/en/binary-log.html)
  - [Replication Protocol](https://dev.mysql.com/doc/dev/mysql-server/latest/page_protocol_replication.html)

## Multi-languages

Canal specially designed the client-server mode, the interactive protocol uses protobuf 3.0, the client side can use different languages to implement different consumption logic, welcome to submit a pull request
  
- canal java client: [https://github.com/alibaba/canal/wiki/ClientExample](https://github.com/alibaba/canal/wiki/ClientExample)
- canal c# client: [https://github.com/dotnetcore/CanalSharp](https://github.com/dotnetcore/CanalSharp)
- canal go client: [https://github.com/CanalClient/canal-go](https://github.com/CanalClient/canal-go)
- canal php client: [https://github.com/xingwenge/canal-php](https://github.com/xingwenge/canal-php)
- canal Python client: [https://github.com/haozi3156666/canal-python](https://github.com/haozi3156666/canal-python)
- canal Rust client: [https://github.com/laohanlinux/canal-rs](https://github.com/laohanlinux/canal-rs)
- canal Nodejs client: [https://github.com/marmot-z/canal-nodejs](https://github.com/marmot-z/canal-nodejs)

Canal, as a MySQL binlog incremental acquisition and parsing tool, can deliver change records to MQ systems, such as Kafka/RocketMQ, and can take advantage of MQ's multilingual capabilities

- Reference document: [Canal Kafka/RocketMQ QuickStart](https://github.com/alibaba/canal/wiki/Canal-Kafka-RocketMQ-QuickStart)

## Related Open Source & Products
- [canal consumer open source project: Otter](http://github.com/alibaba/otter)
- [Alibaba to Oracle data migration synchronization tool: yugong](http://github.com/alibaba/yugong)
- [Alibaba offline synchronization open source project DataX](https://github.com/alibaba/datax)
- [Alibaba database connection pool open source project Druid](https://github.com/alibaba/druid)
- [Alibaba real-time data synchronization tool DTS](https://www.aliyun.com/product/dts)

## Similar Open Source & Products
- [Linkedin Databus](https://github.com/linkedin/databus)
- [tungsten-replicator](http://code.google.com/p/tungsten-replicator)
- [open-replicator](http://code.google.com/p/open-replicator)
	
## feedback
- Report issue: [github issues](https://github.com/alibaba/canal/issues)
