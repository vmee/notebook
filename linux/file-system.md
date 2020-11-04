# 文件系统

基于层次管理文件

目录：路径影射
文件：存储空间存储的一段流式数据，对数据可以做到按名存取

文件系统：层次结构 倒置树状结构 有索引
/：原始起点
/dev/pts/2: 最左侧/表示目录 其他/表示路径分隔符

文件的路径表示
- 绝对路径： 从根开始表示出的路径
- 相对路径：从当位置开始表示出的路径
  
文件名使用法则
- 严格区分字符大小写：file File FILE
- 目录也是文件，在同一路径下，两个文件不能同名
- 支持使用除/以外的任意字符
- 最长不能超过255个字符
- 以.开头的为隐藏文件
  - .为当前目录
  - ..为当目录的上一级目录

用户都有家目录：home
用户的起始目录：普通用户管理文件的位置

工作目录：
/etc/sysconfig/network-scripts/ifcfg-eno1688
basename: 基名 最右侧的文件或目录名
dirname: basename 左侧的目录

## 常用命令
```sh
$ pwd #显示当前目录
$ cd #切换目录 空为切换到家目录
$ cd ~ #切换到家目录
$ cd ~USERNAME #切换到指定用户家目录
$ cd - #切换到上次目录
$ ls -a #显示所有文件
$ ls -A #不显示./..
$ ls -l #显示详细信息
$ ls -h --human-readable #人类可读大小
$ ls -d #读取目录自身
$ ls -r #逆序显示
$ ls -R #递归显示
$ cat filename #查看文本文件
$ cat -n filename #显示文件编号
$ cat -E filename #显示文件内容行换行符
$ tac filename #逆序显示每一行
$ file filename #查看文件内容类型
$ echo "hello" #回显命令
$ echo -n "hello" #不换行
$ echo -e "hello\n" #让转义符生效
```


相关的环境变量
- $PWD：当前工作目录
- $OLDPWD： 上一次的工作目录

-rw-r--r--. 1 root root  1363 Jul 31 17:27 init_master.sh
- -:文件类型 -,d,b,c,l,s,p
- -rw-r--r--.: 左三：文件属主权限 中三文件属组的权限 右三其他用户权限
- root: 文件属主
- root: 文件属组
- 1363： 文件大小 Byte字节
- Jul 31 17:27： 最后修改时间
- init_master.sh 文件名


## 程序编译方式

Linux公共库 glibc GNU标准类库

- 动态链接
- 静态编译

## 进程的类型
终端： 硬件设备 ，关联一个用户接口
与终端相关：通过终端启动
与终端无关：操作引导启动过程 当中自动启动

## 操作系统的组成
- 静态： kernel, application


## FHS

- /bin 供所有用户可用的基础命令程序文件
- /sbin 供系统管理员使用的工具程序
- /boot 引导加载器必须用到的各静态文件 kernel initramfs(initrd), grub等
- /dev 特殊文件或硬件设备文件
  - 设备有两种类型：字符设备（线性设备），块设备（随机设备）
- /etc 系统程序中的配置文件
- /home 用户的家目录 普通用户家目录集中位置，一般每个普通用户的家目录默认为此目录下与用户名同名的子目录 /home/USERNAME
- /root 管理员的家目录 可选 
- /lib  为系统启动或根文件系统上的应用程序（/bin, sbin等）提供共享库，以及内核提供的内核模块
  - libc.so.*: 动态链接的C库
  - ld*:运行时链接器与加载器
  - modules: 用户于存储内核模块的目录
- /lib64：64系统特有的存放64位共享库的路径
- /media： 便携式设备的挂载点，cdrom, floppy等
- /mnt： 其他文件系统的临时挂载点
- /opt： 附加应用程序的安装位置  第三方程序，可选路径
- /srv 当前主机为服务提供的数据
- /tmp 为那些会产生临时文件的程序提供时用于存储临时文件的目录，可供所有用户执行写操作，有特殊权限
- /usr user Hierarchy 全局共享的只读数据
  - bin, sbin
  - lib, lib64
  - include c程序头文件
  - share 架构特有的文件的存储位置，命令自带文档和手册页
  - local 另一个层级目录
  - x11RG x-widow程序的安装位置
  - src 程序源码文件的存储位置
- /usr/local  Local Hierarchy 让系统管理 员安装本地应用程序，也通常安装第三方程序
- /var Hierarchy 存储常发生变化的数据的目录
- proc 虚拟文件系统，用于内核及进程存储其相关信息，它们多为内核参数，例如net.ipv4.ip_forward, 虚拟为net/ipv4/ip_forward, 存储于/proc/sys/,因此其完整的路径为/proc/sys/net/ipv4/ip_forward
- sys sysfs虚拟文件提供了种比proc更为理想的访问内核数据的途径 其主要作用管理Linux设备提供一种统一的模型接口

## Linux的文件系统
- -： 常规文件， 即f
- d: 目录文件
- b: block device 块设备 以“block”为单位随机访问
- c: character device 字符设备文件，支持以“character”为单位进行线性访问
  - major number: 主设备号，用于标识设备类型，进而确定要加载的驱动程序
  - minor number: 次设备号，用于标识同一类型中的不同的设备 
- l：symbolic link; 符号链接文件
- p：pipe 命名管道
- s：socket 套接字文件

## 目录管理命令

路径的基名为命令的操作对象，基名之前的路径必须存在

### mkdir
make directories
- -p 自动按需创建父路径
- -v verbose 显示详细过程
- -m mode 创建时自动设置权限
  
### rmdir
remove empty directories
删除空目录
- -p 自动删除路径空目录
- -v verbose 显示详细过程

### tree
tree [options] [directory]
- -d 只显示目录
- -L  Level 显示目录层级


## disk
持久存储数据

接口类型
- IDE(ata):并口 133MB/s
- SCSI: 并口 ultrascsi320 320MB/S; UltraSCSI640 640MB/S 100iops
- SATA: 串口 6gbps 150-200iops
- SAS: 串口 6gbps
- USB： 串口， 480MB/S
- 并口： 同一线缆可以接多块设备
  - IDE： 两个 主，从
  - SCSI： 宽带16-1  窄带8-1
- 串口：同一线缆可以接一个设备
> iops: io per second

硬盘
- 机械硬盘 硬件设备
  - 同轴旋转 固定角速旋转
  - track 磁道 
  - sector:扇区 512bytes
  - cylinder 柱面 分区划分是基于柱面进行的 
  - 平均寻道时间
    - 5400rpm/7200rpm/10000rpm/15000rpm
- 固态 400iops 电气设备
  - PCI-e接口 
  - 桌面多为sata接口

### mknod 
创建块设备或字符设备文件
mknod - make block or character special files


### 设备文件命名
由ICANN命名
- IDE: /dev/hd[a-z]
- SCSI，SATA，USB，SAS： /dev/sd[a-z]

引用设备的方式
- 设备的文件名
- 卷标
- UUID

> Centos6和7系统将硬盘设备文件标识为/dev/sd[a-z]

### 磁盘分区
- MBR： 0 sector
  - master Boot Record
    - 分为三部分
    - 446bytes bootloader 程序 引导启动操作系统的程序
    - 64bytes 分区表 每16个字节标识一个分区，一共只能有四个分区
      - 4主分区
      - 3y1扩展：
        - n逻辑分区
      - 主分区为1-4
      - 逻辑分区为5+
    - 2byte： MBR区域的有效性标识; 55AA为有效
 

 ### fdisk 命令
1. 查看磁盘的分区信息
   fdisk -l [-u] [device...] 列出指定磁盘设备上的分区情况
2. 管理分区
   fdisk device
   fdisk提供了一个交互式接口来管理分区，它有许多子命令，分别用于不同的管理功能，所有的操作均在内存中存中完成，没有直接同步到磁盘;直到使用w命令保存到磁盘上

常用命令
- n: 创建新分区 
- d：删除已有分区
- t：修改分区类型
- l: 查看所有已知ID
- w: 保存并退出
- q: 不保存并退出
- m：查看帮助信息
- p：显示所有分区信息

> 注意：在已经分区并且已经挂载其中的某个分区的磁盘设备上创建的新分区，内核可能在创建完成后无法直接识别

查看分区： 
```sh
$ cat /proc/partitions
```
通知内核强制重读磁盘分区表
- centos5 : partprobe [device]
- centos6,7: partx, kpartx
  - partx -a [device]
  - kpartx -af [device]

> 分区创建工具：parted stdisk

## 创建文件系统
格式化：
- 低级格式化 分区之前进行 划分磁道
- 高级格式化 分区之后进行 创建文件系统

元数据区，数据区
- 元数据区：inodex(index node)
  - 文件：大小、权限 、属主，属组，时间戳, 数据块指针
- 数据区 block


> 链接文件： 存储数据指针的空间当中存储的是真实文件的访问链接
> 设备文件：存储数据指针的空间当中存储的是设备号（major minor）

bitmap位图索引
- inode位图索引
- block位置索引

superblock
gdt 块组描述符

文件定位：先找根的inode,然后找根的数据块，数据块是子目录与inode，若是目录继续查找目录数据块，若文件则是文件的数据块

## vfs
virtual file system
Linux的文件系统:ext2(无日志功能) ext3 ext4 xfs reiserfs btrfs(btree) 
光盘：iso9660
网络文件系统：nfs，clfs
内核级分布式文件系统：ceph
windows的文件系统：vfat,ntfs
伪文件系统：proc,sysfs,tmpfs, hugepagefs
Unix的文件系统： UFS，FFS, JFS
交换文件系统：swap
用户空间的分布式文件系统：mogilefs, moosefs, glusterfs

## 文件系统管理工具
### 创建文件的工具
mkfs mkfs.ext2 mkfs.ext3...
### 检测及修复文件系统的工具
fsck fsck.ext2 fsck.ext3...
### 查看其属性的工具
dumpe2fs tune2fs
### 调整文件系统特性
tuner2fs


内核级文件系统的组成部分：
		文件系统驱动：由内核提供
		文件系统箮理工具：由用户空间的应用程序提供

### blkid
查看磁盘类型

### Centos6 如何使用xfs文件系统
$ yum -y installl xfsprogs

### mke2fs
ext系列专用管理工具：mke2fs
- -t [ext2|ext3|ext4] 指定要创建的文件系统类型
- -b [1024|2048|4096] 指定文件系统的块大小
- -L LABEL 指明卷标
- -j 创建有日志功能的文件系统
  - mke2fs -j = 
- -i # bytes-per-inode 每多少字节一个inode 指明inode与字节的比率
- -N # 直接指明要给此文件系统创建的inode的数量 
- -m # 指定预留空间，百分比
- -O [^]FEATURE 以指定的特性创建目标文件系统

## ext系列文件系统的管理工具：
		mkfs.ext2, mkfs.ext3, mkfs.ext4
		
		mkfs -t ext2 = mkfs.ext2
		
		ext系列文件系统专用管理工具：mke2fs
			mke2fs [OPTIONS]  device
				-t {ext2|ext3|ext4}：指明要创建的文件系统类型
					mkfs.ext4 = mkfs -t ext4 = mke2fs -t ext4
				-b {1024|2048|4096}：指明文件系统的块大小；
				-L LABEL：指明卷标；
				-j：创建有日志功能的文件系统ext3；
					mke2fs -j = mke2fs -t ext3 = mkfs -t ext3 = mkfs.ext3
				-i #：bytes-per-inode，指明inode与字节的比率；即每多少字节创建一个Indode; 
				-N #：直接指明要给此文件系统创建的inode的数量；
				-m #：指定预留的空间，百分比；
				
				-O [^]FEATURE：以指定的特性创建目标文件系统； 
				
			e2label命令：卷标的查看与设定
				查看：e2label device
				设定：e2label device LABEL
				
			tune2fs命令：查看或修改ext系列文件系统的某些属性 
				adjust tunable filesystem parameters on ext2/ext3/ext4 filesystems；
				注意：块大小创建后不可修改；
				
				tune2fs [OPTIONS] device
					-l：查看超级块的内容；
					
					修改指定文件系统的属性：
						-j：ext2 --> ext3；
						-L LABEL：修改卷标；
						-m #：调整预留空间百分比；
						-O [^]FEATHER：开启或关闭某种特性；
						
						-o [^]mount_options：开启或关闭某种默认挂载选项
							acl
							^acl
							
			dumpe2fs命令：显示ext系列文件系统的属性信息
				dumpe2fs  [-h] device

## 用于实现文件系统检测的工具
				
因进程意外中止或系统崩溃等 原因导致定稿操作非正常终止时，可能会造成文件损坏；此时，应该检测并修复文件系统； 建议，离线进行； 

ext系列文件系统的专用工具：
  e2fsck : check a Linux ext2/ext3/ext4 file system
    e2fsck [OPTIONS]  device
      -y：对所有问题自动回答为yes; 
      -f：即使文件系统处于clean状态，也要强制进行检测；
      
  fsck：check and repair a Linux file system
    -t fstype：指明文件系统类型；
      fsck -t ext4 = fsck.ext4
    -a：无须交互而自动修复所有错误；
    -r：交互式修复；	


## journal
性能损失 元数据要写两次

## link 链接文件

符号链接
权限：lrwxrwxrwx 

硬链接 指向同一个inode的多个文件路径
特性
- 目录不支持硬链接
- 硬链接不能跨文件系统
- 创建硬链接会增加inode引用计数
符号链接 指向一文件路径的另一个文件路径
创建
```sh
ln src link_file
```
特性
- 符号链接与文件是两个各自独立 的文件，各有自己的inode，对原文件不会增加引用计数 
- 支持对目录创建的符号链接，可以跨文件系统
- 删除符号链接文件，不影响原文件，但删除原文件，符号指定的路径不存在，此时会变成无效链接

> 注意：符号链接的文件大小是其指定的文件的路径字符串的字节数

创建：
```sh
ln -s src link_file
```



## CentOS 6如何使用xfs文件系统：
		# yum  -y  install  xfsprogs
			
		事先：
				# cd /etc/yum.repos.d/
				# wget  http://172.16.0.1/centos6.7.repo 
				# mv CentOS-Base.repo CentOS-Base.repo.bak
				
		创建：mkfs.xfs 
		检测：fsck.xfs 		
		
## blkid命令：
		blkid device
		blkid  -L LABEL：根据LABEL定位设备
		blkid  -U  UUID：根据UUID定位设备 
		
## swap文件系统：
		Linux上的交换分区必须使用独立的文件系统；
			且文件系统的System ID必须为82；
			
		创建swap设备：mkswap命令
			mkswap [OPTIONS]  device
				-L LABEL：指明卷标
				-f：强制
				
	Windows无法识别Linux的文件系统； 因此，存储设备需要两种系统之间交叉使用时，应该使用windows和Linux同时支持的文件系统：fat32(vfat); 
		# mkfs.vfat device


## mount命令和umount命令
首先要“挂载”：mount命令和umount命令
		
		根文件系统这外的其它文件系统要想能够被访问，都必须通过“关联”至根文件系统上的某个目录来实现，此关联操作即为“挂载”；此目录即为“挂载点”；
		
			挂载点：mount_point，用于作为另一个文件系统的访问入口；
				(1) 事先存在；
				(2) 应该使用未被或不会被其它进程使用到的目录；
				(3) 挂载点下原有的文件将会被隐藏；
			
		mount命令：
			mount  [-nrw]  [-t vfstype]  [-o options]  device  dir
			
				命令选项：
					-r：readonly，只读挂载； 
					-w：read and write, 读写挂载； 
					-n：默认情况下，设备挂载或卸载的操作会同步更新至/etc/mtab文件中；-n用于禁止此特性；
					
					-t vfstype：指明要挂载的设备上的文件系统的类型；多数情况下可省略，此时mount会通过blkid来判断要挂载的设备的文件系统类型；
					
					-L LABEL：挂载时以卷标的方式指明设备；
						mount -L LABEL dir
						
					-U UUID：挂载时以UUID的方式指明设备；
						mount -U UUID dir
						
				-o options：挂载选项
					sync/async：同步/异步操作；
					atime/noatime：文件或目录在被访问时是否更新其访问时间戳；
					diratime/nodiratime：目录在被访问时是否更新其访问时间戳；
					remount：重新挂载； 
					acl：支持使用facl功能；
						# mount -o acl  device dir 
						# tune2fs  -o  acl  device 
						
					ro：只读 
					rw：读写 
					dev/nodev：此设备上是否允许创建设备文件；
					exec/noexec：是否允许运行此设备上的程序文件；
					auto/noauto：
					user/nouser：是否允许普通用户挂载此文件系统；
					suid/nosuid：是否允许程序文件上的suid和sgid特殊权限生效；					
					
					defaults：Use default options: rw, suid, dev, exec, auto, nouser, async, and relatime.
					
			一个使用技巧：
				可以实现将目录绑定至另一个目录上，作为其临时访问入口；
					mount --bind  源目录  目标目录
					
			查看当前系统所有已挂载的设备：
				# mount 
				# cat  /etc/mtab
				# cat  /proc/mounts
				
			挂载光盘：
				mount  -r  /dev/cdrom  mount_point
				
				光盘设备文件：/dev/cdrom, /dev/dvd
				
			挂载U盘：
				事先识别U盘的设备文件；
				
			挂载本地的回环设备：
				# mount  -o  loop  /PATH/TO/SOME_LOOP_FILE   MOUNT_POINT 
				
		umount命令：
			umount  device|dir
			
			注意：正在被进程访问到的挂载点无法被卸载；
				查看被哪个或哪些进程所占用：
					# lsof  MOUNT_POINT
					# fuser -v  MOUNT_POINT
					
					终止所有正在访问某挂载点的进程：
					# fuser  -km  MOUNT_POINT

## 交换分区的启用和禁用：
		创建交换分区的命令：mkswap
		
		启用：swapon
			swapon  [OPTION]  [DEVICE]
				-a：定义在/etc/fstab文件中的所有swap设备；
				
		禁用：swapoff
			swapoff DEVICE

## 设定除根文件系统以外的其它文件系统能够开机时自动挂载：/etc/fstab文件 
		每行定义一个要挂载的文件系统及相关属性：
			6个字段：
				(1) 要挂载的设备：
					设备文件；
					LABEL
					UUID
					伪文件系统：如sysfs, proc, tmpfs等
				(2) 挂载点 
					swap类型的设备的挂载点为swap；
				(3) 文件系统类型；
				(4) 挂载选项
					defaults：使用默认挂载选项；
					如果要同时指明多个挂载选项，彼此间以逗号分隔；
						defaults,acl,noatime,noexec
				(5) 转储频率
					0：从不备份；
					1：每天备份；
					2：每隔一天备份；
				(6) 自检次序
					0：不自检；
					1：首先自检，通常只能是根文件系统可用1；
					2：次级自检
					...
				
			mount  -a：可自动挂载定义在此文件中的所支持自动挂载的设备；

## 两个命令 df和du
		df命令：
			df [OPTION]... [FILE]...
				-l：仅显示本地文件的相关信息；
				-h：human-readable
				-i：显示inode的使用状态而非blocks
				
		du命令：
			du [OPTION]... [FILE]...
				-s: sumary
				-h: human-readable


## 删除文件

将此文件指向的所有data block标记为未使用状态，将此文件的inode标记为未使用

## 复制与移动文件
复制：新建文件

移动：
- 同一文件系统上，改变的仅是其路径 
- 在不同文件系统，复制数据到目标文件，并删除原文件

符号链接：
- 权限：lrwxrwxrwx
- 权限受链接的文件管理
  
硬链接
- 指向同一个inode

## 挂载光盘设备 
光盘设备文件
- IDE:/dev/hdc
- SATA:/dev/sr0

符号链接文件
- /dev/cdrom
- /dev/cdrw
- /dev/dvd
- /dev/dvdrw

```sh
mount -r /dev/cdrom /media/cdrom
unmount /dev/cdrom
```

## 挂载U盘
- fdisk
- mount

## dd命令:convert and copy a file
用法
dd if=/PATH/FROM/SRC of=/PATH/TO/DEST
- bs=# block size 复制单元大小
- count=# 复制数量

磁盘拷贝
dd if=/dev/sda of=/dev/sdb

备份MBR
dd if=/dev/sda of=/tmp/mbr.bak bs=512 count=1

破坏MRB中的boot loader:
dd if=/dev/zero of=/dev/sda bs=256 count=1


## 两个特殊设备
/dev/null: 数据黑洞
/dev/zero: 吐零机


## 压缩及解压缩工具

压缩比
压缩目的: 时间换空间 cpu的时间->磁盘空间

- compress/uncompress .z
- gzip/gunzip .gz
- bzip2/bunzip2 .bz2
- xz/unxz .xz
- zip/unzip 
- tar
- cpio


### gzip/gunzip/zcap 
- zcap 展开压缩包
- -d 解压缩 相当于gunzip
- -# 指定压缩比 默认6 数字越大压缩比越大(1-9)
- -c 将压缩结果输出到标准输出
  - gzip -c FILE > compress.file

### bzip2/bunzip2/bcat
bzip [option] FILE
- -d 解压缩 相当于bunzip2
- -# 指定压缩比 默认6 数字越大压缩比越大(1-9)
- -k 保留原文件

### xz/unxz/xzcat  lzma/unlzma/lzcat

xz [option]  FILE
- -d 解压缩
- -# 指定压缩比,默认是6,数字越大压缩比越大(1-9)
- -k 保留原文件

### 归档 tar/cpio

tar [options] file
- -c -f 创建归档
- -x 展开归档
- -C 展开到指定目录
- -t 查看归档的文件列表
- -cf 创建归档
- -xf file.tar -C 目录
- -tf 展开
- -v 打印详细输出

归档完成后通需要压缩, 给合此前的压缩工具,就实现多个文件了

归档并压缩
- -z gzip
  - tar -zct /PATH/TO/FIle.tar.gz FILE...
  - tar -zxf /PATH/TO/FIle.tar.gz
- -j: bzip2
  - tar -jct /PATH/TO/FIle.tar.bz2 FILE...
  - tar -jxf /PATH/TO/FIle.tar.bz2
- -J:xz
  - tar -Jct /PATH/TO/FIle.tar.xz FILE...
  - tar -Jxf /PATH/TO/FIle.tar.xz

### zip/unzip
最通用的压缩工具
压缩比有限
即能归档又能压缩
后缀名.zip


