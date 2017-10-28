
#### 高可用redis实战学习笔记

+ redis的安装配置
	+ 本地配置,安装教程网上一大堆

+ redis的两种数据持久化
	+ rdb持久化
		+ 周期性的对redis的数据做快照
		+ rdb持久化的配置 配置文件中配置save项 
			+ eg : save 60 1000 每个60秒超过了1000个key发生变化,就会生成一个快照
			+ redis-cli shutdown的时候也会生成一个快照
	+ aof持久化	
		+ 每次写入的命令写入日志文件。redis的缓存数据是有大小限制的,aof文件不会一直增加。缓存到达限制之后,redis会执行缓存清除策略,aof文件会执行rewriter操作,新建一个文件记录当前的数据,删除老的aof文件

		+ aof 的配置 appendonly项 改为yes 
		+ 如果同时设置了rdb和aof，redis默认使用aof来恢复文件

	```bash
		每小时copy一次备份，删除48小时前的数据
		crontab -e
		0 * * * * sh /usr/local/redis/copy/redis_rdb_copy_hourly.sh
		redis_rdb_copy_hourly.sh

		#!/bin/sh 
		cur_date=`date +%Y%m%d%k`
		rm -rf /usr/local/redis/snapshotting/$cur_date
		mkdir /usr/local/redis/snapshotting/$cur_date
		cp /var/redis/6379/dump.rdb /usr/local/redis/snapshotting/$cur_date		
		del_date=`date -d -48hour +%Y%m%d%k`
		rm -rf /usr/local/redis/snapshotting/$del_date

		每天copy一次备份
		crontab -e		
		0 0 * * * sh /usr/local/redis/copy/redis_rdb_copy_daily.sh
		redis_rdb_copy_daily.sh
		
		#!/bin/sh 
		cur_date=`date +%Y%m%d`
		rm -rf /usr/local/redis/snapshotting/$cur_date
		mkdir /usr/local/redis/snapshotting/$cur_date
		cp /var/redis/6379/dump.rdb /usr/local/redis/snapshotting/$cur_date
		
		del_date=`date -d -1month +%Y%m%d`
		rm -rf /usr/local/redis/snapshotting/$del_date
	```

+ redis 的读写分离
	+ 主节点master,从节点slave。从节点第一次连接master,master会执行全量的复制数据给slave,slave重连之后会进行增量的复制。
	+ 主从复制的断点续传,master和slave会记录一个数据偏移量offset,再次复制会根据偏移量进行复制。

	+ redis默认是开启从机器的强制只读 slavae-read-only yes

	```
		利用redis自带工具对redis做压力测试
		./redis-benchmark -h 192.168.31.187

		-c <clients>       Number of parallel connections (default 50)
		-n <requests>      Total number of requests (default 100000)
		-d <size>          Data size of SET/GET value in bytes (default 2)
	```
	+ 如果是一主多从肯定会影响redis的高可用性,主机master挂了就会不可用

+ redis的哨兵模式实现高可用
	+ fail over 故障转移  主机挂了,会选择一个slave作为master 
+ redis cluster实现大数据量，高可用
	+ hash取模: 某一台master宕机会导致所有的数据都找不到,因为要针对机器重新hash找不到对应的机器
	+ 一致性hash算法: 数据取模均匀分布在一个圆环上,顺时针寻找最近的redis master,某一台master宕机,做数据迁移的工作量会很少
	+ hash-slot redis-cluster将数据分成16384分，每个数据对16384取模,找到对应的机器(其实就是一致性hash的一中实现)
	+ 自动对数据进行分片
		+ 每次从redis上存取值，会自动对该key进行16384取模,如果在该台服务器上就直接返回值,否则返回一个moved error告诉你去哪台机器上面执行存取操作
	
	+ redis cluster集群的搭建
		+ 网上教程!!!!
			+ **集群搭建开放端口号:port 6379 redis集群间通讯的端口默认+10000 即为16379 要开放这个两个端口 **
+ redis支持的数据类型
	+ string 
		+ set/get 设置获取值
		+ setex 设置值同时设置过期时间
		+ setnx key不存在才会设置值.存在不设置
		+ incr 对指定的key的value值加1 | incrby key 增加指定增量的值
		+ decr /decrby  
	+ list 可以有重复的元素
		+ lpush key value 
		+ rpush key [value...]
		+ llen key 指定key的长度
		+ lpop 移除并返回第一个元素
	+ hash 
		+ hmset key field value [file... value...]
		+ hmget key field
		+ hlen 属性的个数
		+ hgetall 获取所有的属性和值
	+ set  没有重读的元素
		+ sadd
	+ zset 有序的set
		+ zadd key score value zdd通过与key关联的score来对元素进行排序 