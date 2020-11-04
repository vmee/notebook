# yum

Centos: yum, dnf

YUM: yellow dog  Yellowdog Update Modifier

yum repository: yum repo
存储了众多rpm包,以及包的相关的无数据文件(放置于特定的目录下:repodata): 

仓库 文件服务器:
- ftp://
- http://
- nfs://
- file:///

## yum客户端
配置文件
- /etc/yum.conf 仓库公共配置和不属于仓库的配置 
- /etc/yum.repos.d/*.repo 仓库配置 为仓库的指向提供配置


### 仓库指向的定义
[repositoryID]
```config

```


### yum命令的用法 
yum [options] command [package ...]


command is one of:
* install package1 [package2] [...]
* update [package1] [package2] [...]
* update-to [package1] [package2] [...]
* update-minimal [package1] [package2] [...]
* check-update
* upgrade [package1] [package2] [...]
* upgrade-to [package1] [package2] [...]
* distribution-synchronization [package1] [package2] [...]
* remove | erase package1 [package2] [...]
* autoremove [package1] [...]
* list [...]
* info [...]
* provides | whatprovides feature1 [feature2] [...]
* clean [ packages | metadata | expire-cache | rpmdb | plugins | all ]
* makecache [fast]
* groups [...]
* search string1 [string2] [...]
* shell [filename]
* resolvedep dep1 [dep2] [...]
   (maintained for legacy reasons only - use repoquery or yum provides)
* localinstall rpmfile1 [rpmfile2] [...]
   (maintained for legacy reasons only - use install)
* localupdate rpmfile1 [rpmfile2] [...]
   (maintained for legacy reasons only - use update)
* reinstall package1 [package2] [...]
* downgrade package1 [package2] [...]
* deplist package1 [package2] [...]
* repolist [all|enabled|disabled]
* repoinfo [all|enabled|disabled]
* repository-packages <enabled-repoid> <install|remove|remove-or-reinstall|remove-or-distribution-synchronization> [package2] [...]
* version [ all | installed | available | group-* | nogroups* | grouplist | groupinfo ]
* history [info|list|packages-list|packages-info|summary|addon-info|redo|undo|rollback|new|sync|stats]
* load-transaction [txfile]
* updateinfo [summary | list | info | remove-pkgs-ts | exclude-updates | exclude-all | check-running-kernel]
* fssnapshot [summary | list | have-space | create | delete]
* fs [filters | refilter | refilter-cleanup | du]
* check
* help [command]


- 显示仓库列表: yum repolist [all|enabled|disabled]
- 显示程序包: yum list 
  - yum list all
  - yum list [glob]
  - yum list [availabe|installled|updates] [glob_exp1] [...]
- 安装: yum install PACKAGE_NAME
- 升级: yum update PACKAGE_NAME
- 检查升级: yum check-update
- 卸载: yum remove|erase PACKAGE_NAME
- 查看包的简要信息: yum info PACKAGE_NAME
- 查看指定的特性CAPABILITY由哪个程序包提供: yum provides|whatprovides CAPABILITY
- 清理本地缓存 yum clear [packages | mediadata | expire-cache | rpmdb | plugins | all ]
- 创建缓存 yum make cache
- 搜索程序包名及summary信息: yum search KEY
- 重新安装 yum reinstall PACKAGE_NAME
- 程序降级: yum downgrade  PACKAGE_NAME
- 查询指定包所依赖的CAPABILITY: yum deplist PACKAGE_NAME
- 查看yum的事务历史: yum history [info|list|packages-list|packages-info|summary|addon-info|redo|undo|rollback|new|sync|stats]


安装升级本地程序包
-  localinstall rpmfile1 [rpmfile2] [...] (maintained for legacy reasons only - use install)
- localupdate rpmfile1 [rpmfile2] [...]  (maintained for legacy reasons only - use update)

包组管理的相关命令
yum group [install | list | info | upgrade ...]


如何使用光盘当作本地yum仓库
- 挂载光盘 /media/cdrom # mount  -r -t iso9660 /dev/cdrom /media/cdrom
- 创建配置文件
  - [Centos7]
  - name=
  - baseurl=
  - gpgcheck=
  - enabled=

yum命令行选项
- --nogpgckeck 禁止gpg check
- -y 自动回答yes
- -q 静默模式
- --disablerepo=repoidglob 临时禁用repo
- --enablerepo=repoidglob 临时启用repo
- --noplugin 禁用所有插件 

yum的repo配置文件中可用的变量
- $releasever: 当前os的发行版的主版本号
- $arch: 平台
- $basearch: 基础平台
- $YUM0-$YUM9

> httpL//mirrors.xx.com/centos/$releasever/$basearch/os

## 创建自己的yum仓库

create [options] <directory>
- --basedir

## 程序包的编译安装

test-VERSION-release.src.rpm
安装后,使用rpmbuild命令制作成二进制格式的rpm包,而后再安装

源代码-> 预处理 -> 编译(gcc) --> 汇编 --> 链接 --> 执行

源代码的组织格式:
- 多文件,文件中的代码之间,很可能存在跨文件的依赖关系
- c c++: make config: Makefile.in -> makefile
- java: maven

编译安装三步骤
- ./configure #配置 从Makefile.in模块生成makefile
  - 通过选项传递参数,指定启用特性,安装路径竺, 执行时会参考用户的指定以及Makefile.in文件生成makefile
  - 检查依赖到的外部环境
- make # 根据makefile 编译构建应用程序
- make install  # 把文件复制到安装目录

开发工具
- autoconf 生成configure脚本
- automake 生成Makefile.in

> 建议:安装前看INSTALL, README

开源程序源代码的获取
- 官方自建站点
- 代码托管 sourceForge/github/code.google.com

c/c++: gcc(GNU c complier)

编译c源代码

前提:提供开发工具及开发环境
- 开发工具: make, gcc等
- 开发环境: 开发库,头文件,glibc:标准库 
- 通过"包组"提供开发组件
  - Centos: "Deveploment Tools" "Server Platform Development"
  
第一步: configure脚本

选项
- 安装路径
  - --prefix=/PATH 指定默认安装位置 一般默认/usr/local
  - sysconfdir=/PATH 配置文件安装路径 
  - System Type: 交叉编译
  - Optional Feature 可选特性
    - --disalbe-FEATURE
    - --enable-FEATURE 
   - Options Package
     - --with
     - --nowith

第二步: make

第三步: make install


安装后的配置
- 导出二进程程序目录 到PATH环境变量
  - 编译文件/etc/profile.d/NAME.sh
  - export PATH=/PATH/FILE:$PATH
- 导出库文件路径 
  - 编译/etc/ld.so.conf/NAME.conf
  - 添加新的库文件所在目录到此文件
  - 让系统重新生成缓存 ldconfig [-v]
- 导出头文件
  - 基于链接的方式实现 ln -sv
- 导出帮助手册 
  - 编译/etc/man.config文件 添加一个MANPATH

