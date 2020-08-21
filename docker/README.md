# Docker

## 主机级虚拟化：vmware

* Type-I：exsi  硬件之上操作虚拟化
* Type-II: vmware 操作系统之上虚拟化

```text
graph TD
 用户空间 -->
 内核 --> 
 操作系统 -->
 虚拟化软件 --> 硬件硬件4
```

## 隔离\(应用安全运行\)

* UTS：设置主机名和对该命名空间中正在运行的进程可见的域
* Mount： 文件， 挂载树
* IPC： 命名空间提供命名的共享内存段，信号量和消息队列的分离
* PID：
* User： 
* Network

> namespaces: clone\(\), setns\(\),

## 容器级虚拟化

## Control Groups\(cgroups\)

* blkio： 块设备IO
* cpu： CPU
* cpuacct： CPU资源的使用报告 
* cpuset： 多处理器平台上的CPU集合
* devices： 设备访问
* freezer： 挂起或恢复任务
* memory： 内容用量及报告 
* pref\_event： 对cgroup中的任务进行统一的性能测试
* net\_cls: cgroup中任务创建的数据报文的类别识别符

## LinuX Container\(LXC\)

Docker是LXC的二次发行版

## Docker event state

[Docker event state](https://img2018.cnblogs.com/blog/1198389/201902/1198389-20190226150526305-1795295624.png)

## Docker Images

* 分层构建机制 最底层bootfs 其之是rootfs
* 
