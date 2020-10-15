# RAID

独立冗余磁盘阵列

Redundant Arrays of Inexpenslve Disks
Redundant Arrays of Independent Disks

提高IO能力: 磁盘并行读写
提高耐用性: 磁盘冗余实现

级别: 多块磁盘组织在一起的工作方式有所不同

RAID实现的方式
- 外接式磁盘阵列: 通过扩展卡提供适配能力
- 内模式RAID: 主板集成RAID控制器
- Software RAID:

## 级别:LEVEL
- RAID-0 : 0, 条带卷 strip
- RAID-1 : 1, 镜像卷 mirror
- RAID-2 
- RAID-5
- RAID-6
- RAID-10
- RAID-01

### RAID-0
读 写性能提升
可用空间: N*min(s1, s2....)
无容错能力
最少磁盘数:2, 2+

### RAID-1
读性能提升 写性能略有下降
可用空间:1*min(s1, s2...)
有容错能力
最少磁盘数:2

### RAID-4
s1 s2 校验码盘s

1101, 0110, 1011

### RAID-5
读写能力提升
可用空间: (N-1)*min(s1, s2...)
有容错能力: 1块磁盘
最少磁盘数: 3,3+

### RAID-6
读写能力提升
可用空间:(N-2)*min(s1, s2...)
有容错能力:2块磁盘
最少磁盘数: 4, 4+

### RAID-10 混合类型
先做1后做0
读写性能提升
可用空间: N*min(s1, s2, ...)/2
有容错能力: 每组镜像最多只有坏一块
最少磁盘数:4, 4+

### RAID-01 混合类型
先做0后做1


### RAID-50 RAID7 混合类型

### JBOO Just  a Bunch of disk
功能:将多块磁盘的空间合并一个大的连续空间使用
可用空间:sum(s1,s2...)


常用级别: RAID-0, RAID-1, RAID-5 RAID-10 RAID-50 JBOO


## 实现方式:
- 硬件实现方式
- 软件实现方式

Centos 6上的软件的RAID的实现
结合内核中的md(multi devices)

mdadm 模式化的工具
命令的语法格式: mdadm [model] <raiddevice> [options] <componet-devices>
支持的RAID级别: LINEAR, RAID0, RAID1, RAID4, RAID5, RAID6, RAID10

模式:
- 创建 -c
- 装配 -A
- 监控 -f
- 管理 -f -r -a

<raiddevice>:/dev/md#

-c: 创建模式
- -n #: 使用#个块设备 来创建此RAID
- -l #: 指明要创建的RAID的级别
- -a {yes|no} 自动创建目录RAID设备的设备文件
- -c CHUNK_SIZE 指明块大小
- -x # 指明空间闲盘的个数

-D: 显示raid的详细信息
mdadm -D /dev/md#

管理模式:
-f 标记指定磁盘为损坏
-a 添加磁盘
-r 移除磁盘

观察md设备 
mdadm -s /dev/md#

watch命令
-n #: 刷新间隔,单位是秒

watch -n# 'COMMAND'
