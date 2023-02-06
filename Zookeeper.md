	1. 安装与启动；
		a. https://www.runoob.com/w3cnote/zookeeper-setup.html
	2. 框架；
	zookeeper是一个用来实现分布式一致性的一个内核，比如： 分布式锁，分布式配置，leader选举，发布/订阅等；
		实现的原理： 通过本地存储类似于文件的znode，开放简易的create/delete/exists/getData/setData/getChildren/sync接口来实现。
		通过watch监听来确保read的性能，运行在内存当中，通过DB和log作为缓存，是一个简单、快速、高可用的系统。
		
	3. 通过zookeeper实现分布式锁；
```
Lock 
1 n = create(l + “/lock-”, EPHEMERAL|SEQUENTIAL) 
2 C = getChildren(l, false) 
3 if n is lowest znode in C, exit 
4 p = znode in C ordered just before n 
5 if exists(p, true) wait for watch event 
6 goto 2 

Unlock 
1 delete(n)
```
  
	4. 一致性：
	通过zab协议保证分布式事务的最终一致性zab（Zookeeper Atomic Broadcast），类似paxos和raft的一致性算法

	链接
	https://www.usenix.org/legacy/event/atc10/tech/full_papers/Hunt.pdf
	https://zookeeper.apache.org/
