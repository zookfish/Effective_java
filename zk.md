
#### zookeeper
+ Server端具有发申通fast fail特性leader宕机就会通过选举
+ zk的用途
	+ zk用作配置中心
	+ zk的分布式通知,不同系统对同一个节点node进行注册(watcher),某一个应用对该节点进行update,zk会通知到所有对该节点监听的应用
	+ zk的分布式锁
	+ 集群的管理
	+ 分布式的队列