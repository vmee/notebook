# 中间件

## 概念术语

- Raft: etcd所采用的保证分布式系统强一致性的算法
- Node: 一个Raft状态机实例
- Member: 一个etcd实例。 它管理着一个Node， 并且可以客户端请求提供服务。
- Cluster: 由多个Member构成可以协同工作的etcd集群。
- Peer: 对同一个etcd集群中另外一个Member的称呼。
- Client: 向etcd集群发送HTTP请求的客户端。
- WAL: 预写式日志，etcd用于持久存储的日志格式。
- snapshot：
- Proxy
- Leader
- Follwer
- Candidate: 当Follower超过一定时间接收不到Leader的心跳时转变为Candidate开始竞选。
- Term: 某个节点成为Leader到下一次竞选时间，称为一个Term
- Index: 数据项编号。Raft中通过Term和Index来定位数据


## 数据读写顺序
- 读取: 可以从任一节点读取
- 写入: 1. Leader写入 Leader到所有Follower 2. Follower写入 Follower到Leader到所有Follower
> 为了保证数据的强一致性，数据流向为从Leader流向Follower

## 领导选举


## 判断数据是否写入