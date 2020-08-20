# 学习资料

## 物理架构

master/nodes\(worker\)

* master: API server, Scheduler,Controller-Manager
* node: kubelet,docker，kube-proxy

master节点一般三个，做高可用 nodes 运行容器的节点

请求创建pod &gt; master调度器 &gt; 选择最优节点

* docker register 放入k8s
* kubernetes 托管 kubernetes

service的变更由kube-proxy管理

## concept

### API Server

接收请求，解析请求

apiServer仅接收JSON格式的资源定义 yaml格式提供配置清单， apiserver可自动将其转为json格式，而后提交。

### Scheduler

调度器，观察各node的cpu/memory 创建容器 1. 先做预选，找出符合要求的节点 2. 再从预选里，根据调度算法选择最佳节点

k8s可以docker资源阈值 包括上限与下限，下限也叫请求量

### pod健康控制器

loop 周期性探测容器的用户预期状态 如果不符合用户预计将会调用apiServer,创建满足用户预期的容器状态

### 控制器管理器

监控控制器

### 健康状态检查healthcheck

持续监控容器

### kube\_cluster

CPU,RAM 为各节点的总和

### 标签选择器 selector

根据标签过滤符合条件的资源对象

### services

代理pod 通过label找到pod

### AddOns:附加组件

dns

### pod

k8s调度的原子单元 一个pod，包含多个容器 pod 模拟传统的虚拟机 共享底层的网络空间 共享存储卷

给pod打标签，标记唯一，身份识别与过滤

* Label: key=value
* Label Selector:

分类

* 自主式pod
* 控制器管理的pod
  * ReplicationController
  * ReplcaSet
  * Deployment
  * StafulSet
  * DeamonSet
  * Job Cronjob

#### HPA：Horizontal Pod AutoScaler

### sidecar

pod有一个主容器，其他辅助容器为sidecar

### 网络

pod网络 pod运行在一个网段中 集群网络 service运行在另一个网段 节点网络 node网段

* 同一个pod的容器里多个容器间怎么通信:lo
* 各pod之间如何通信 overlay NewWork 叠加网络
* Pod与Service之间的通信

#### CNI

符合此标准CNI插件要求，就可以接入k8s 以POD方式运行

* flannel: 网络配置
* calica: 网络配置 网络策略
* canel:

### RESTful

* GET, PUT, DELETE, POST
* kubectl run get edit

### 资源对象

* workload: Pod,  ReplcaSet, Deployment, StatefulSet, DaemonSet, Job,Cronjob..
* 服务发现与均衡:Service, Ingress
* 配置与存储： Volume, CSI
  * ConfigMap, Secret
  * DownwardAPI
* 集群集资源 
  * NameSpace、Node、 Role、 ClusterRole、 RolrBinding、ClusterRoleBinding
* 元数据型资源
  * HPA， PodTemplate， LimitRange

## 资源配置清单

* APIVersion： group/version

  ```bash
  $ kubectl api-versions
  ```

* kind: 资源类别
* metadata: 元数据
  * name
  * namespace
  * labels
  * annotations
  * 每个资源的引用path: /api/GROUP/VERSION/namespaces/NAMESPACE/TYPE/NAME
* spec:期望状态 disired state
* status: 当前状态， current statue

资源的清单格式

* 一级字段：apiVersion（group/version）,kind, metadata

pod资源 spec.containers &lt;\[\]object&gt;

* name 

  image 

  * imagePullPolicy  : Always,Never,IfNotPresent; latest默认Always, 其他IfNotPresent

修改镜像的中的默认应用 command, args

标签labels key=value： key字母 数了，\_, -, . value:可以为空，只有字母或数字开关及结尾，中间可使用 63字符

nodeSelector  节点标签选择器

nodeName: 指定节点

annotations: 注解 与label不同的是，它不用于挑选资源对象，仅用于对象提供的元数据

### pod生命周期

* 状态： Pending、Running、Failed、Successed、Unkown。。
* 创建Pod： 

  pod生命周期中重要的行为

     初始化容器

     容器探测 liveness

     容器探测 readiness

#### 探针

exceAtion TCPSocketAction HTTPGetAction

livenessProbe

### kubectl explain

```bash
# 命令行查看如何定义
kubectl explain pods
kubectl explain pods.metadata
kubectl explain spec
```

## pod控制器

* ReplicaSet 
* Deployment: 控制ReplicaSet 最应该掌握的控制器
* DaemonSet: 系统级的后台任务
* Job
* CronJob
* StatefulSet

## Helm

[Kubernetes Handbook——Kubernetes中文指南/云原生应用架构实践手册](https://jimmysong.io/kubernetes-handbook/)

