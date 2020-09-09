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