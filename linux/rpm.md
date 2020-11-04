# 程序包的安装卸载

## 概述
API: application program interface
ABI: application binary interface

Unix-like: ELF格式
Windows:exe,msi

库级别的虚拟化
- WinE: linux上运行windows程序
- cywin: windows运行linux应用程序

系统级开发:c/c++/go: httpd,vsftpd,nginx
应用级开发:java/python/perl/ruby/php 
- java: hadoop,hbase,es (jvm c)
- python: openstack (pvm c/c++)
- perl:(perl解释器)
- ruby:(ruby解释器)
- php:(php解释器)

程序格式
- 源代码: 文本格式的程序代码
  - 编译开发环境: 编译器,头文件,开发库
- 二进制格式: 文本格式的程序代码->编译器->二进制格式(二进制程序,库文件,配置文件,帮助文件)

java/python程序格式
- 源代码: 编译成能够在其虚拟机(jvm/pvm)运行格式;
  - 开发环境:编译器,开发库
- 二进制:


项目构建工具
- c/c++: make
- java:maven


## 程序包管理器
源代码->目标二进制格式->组织成一个或有限几个"包"文件

安装,升级,卸载,查询,校验

管理器
- debian: dtp,dpkg, ".deb"
- redhat: redhat packeg manager, rpm, ".rpm"; rpm is package manager
- S.u.S.E: rpm, ".rpm"
- Gentoo: ports
- ArchLinux: 

源代码:name-VERSION.tar.gz
- VERSION: major.minor.release
  
rpm包命名格式
- name-VERSION-release.arch.rpm
- VERSION: major.minor.release
- release.arch: rpm包的发行号
  - archetecture: i386, x64(amd64),ppc,noarch
  - release.os: 2.el7.i386.rpm
- 例:redis-3.0.2-1.centos7.x64.rpm

changelog


拆包: 主包和支包
- 主包:name-VERSION-release.arch.rpm
- 支包:name-function-release.arch.rpm
  - function: devel, utils,libs...

依赖关系
- 前端工具: 自动解决依赖关系
  - yum: rpm包管理器的前端工具 rhel系列系统
  - apt-get(apt-cache): deb包管理器的前端工具
  - zypper: suse的rpm管理器前端工具
  - dnf: Fedora 22+系统上rpm包管理器的前端工具

### 功能
将编译好的应用程序的各组成文件打包成一个或几个程序包文件,从而更方便地实现程序包的安装,升级,卸载和查询等管理操作

1. 程序包的组成清单(每个程序包都单独实现)
   - 文件清单
   - 安装或卸载时运行的脚本 
2. 数据库(公共)
   - 程序包的名称和版本
   - 依赖关系
   - 功能说明
   - 安装生成的各文件的文件路径及校验码信息
   - 等等
3. /var/lib/rpm

### 获取程序包的途径
1. 系统发行版的光盘或官方的文件服务器(镜像站点)
   - http://mirrors.aliyun.com
   - http://mirrors.sohu.com
   - http://mirrors.163.com
2. 项目的官方站点
3. 第三方组织:EPEL
4. 搜索引擎: pkgs.org,rpmfind.net,rpm.pbone.net
5. 自己动手,丰衣足食

建议: 检查其合法性
- 来源合法性
- 程序包的完整性

## Centos系统上rpm命令的管理程序包
安装,升级,卸载,查询和校验,数据库维护

### rpm命令
rpm [option] PACKAGE_FILE

- -i --install 安装
- -U --update -F -freshen 升级
- -e --erase 卸载
- -q --query 查询
- -v --verify 校验
- --builddb --initdb 数据库维护


##### 安装
 rpm {-i|--install} [install-options] PACKAGE_FILE ...

rpm -ivh PACKAGE_FILE
rpm -ql zhs 查看安装文件包

options:
 - -v verbose 详细信息
 - -vv 更详细的输出  
 - -h 进度条 hash marks 输出进度,每个#表示2%的进度
 - --test 测试安装,检查并报告依赖关系及冲突消息等
 - --nodeps 忽略依赖关系,不建议
 - --replacepkgs 重新安装
  
注意: rpm可以自带安装脚本
有四类
- preinstall 安装过程之前运行的脚本 %pre --nopre
- postinstall 安装过程这后运行的脚本 %post --post
- preuninstall 卸载之前运行的脚本 %preun --nopreun
- postuninstall 卸载之后运行的脚本 $postun --nopostun

- --nosignature 不检查 包签名信息,不检查来源合法性
- --nodigest 不检查包完整性信息

#####  升级

- rpm {-U|--upgrade} [install-options] PACKAGE_FILE ...
- rpm {-F|--freshen} [install-options] PACKAGE_FILE ...


options
- -u 升级或安装 rpm -Uvh PACKAGEFILE
- -F 升级 rpm -fvh PACKAGEFILE
- --oldpackage 降级
- --force 强制升级

> 不要对内核做升级操作;
> linux支持多内核版本并存,因此,直接安装新版本内核
> 如查某原程序包的配置文件安装后曾被被修改过,升级时,新版本的程序提供的同一个配置文件不会覆盖原有版本的配置文件件,而是把新版本的配置文件重命名(FILENAME_rpmnew)后提供


##### 卸载
- rpm {-e|--erase} [--allmatches] [--justdb] [--nodeps] [--noscripts] [--notriggers] [--test] PACKAGE_NAME ...

- --allmatches 卸载所有西欧指定名称的程序包的各版本
- --nodeps 忽略依赖
- --test  测试卸载 dry run 
  
##### 查询-重要功能
- rpm {-q|--query} [select-options] [query-options]

select-options
- PACKAGE_NAME: 查询指定包名的程序包是否安装,及其版本
- -a 查询所有安装的包 
- -f FILE 查询指定文件由哪个程序包安装生成
- -p --package PACKAGE_FILE 用于实现对未安装的程序包执行查询操作
- --whatprovides CAPABILITY: 查询指定的CAPABILITY由哪个程序包提供
- --whatrequired CAPABILITY: 查询指定的CAPABILITY被哪个包所依赖


query-options
- --changelog 查询rpm包的changelog 
- -l --list 程序包安装的所有文件
- -i --info : 程序包相关的信息 版本号,大小,所属包组等 
- -c --configfiles 查询指定的程序提供的配置文件
- -d --docfiles 查询指定程序的包的帮助文档
- --providers : 列出指定程序包提供的CAPABILITY
- -R --requires 查询指定程序包的依赖关系
- --scripts 查询程序包自带的脚本版本

用法
- -qi PACKAGE 
- -qf PACKAGE 
- -qc PACKAGE
- -ql PACKAGE
- -qpf PACKAGE_FILE
- -qpl PACKAGE_FILE
- -qd PACKAGE

##### 校验
- rpm {-V|--verify} [select-options] [verify-options]

options 
- -V 

result:
- S file Size differs
- M Mode differs (includes permissions and file type)
- 5 digest (formerly MD5 sum) differs
- D Device major/minor number mismatch
- L readLink(2) path mismatch
- U User ownership differs
- G Group ownership differs
- T mTime differs
- P caPabilities differ

包来源的合法性和完整性验证
- 来源合法性验证
  - 数字签名
- 完整性验证

获取并导入信任的包制作者的密钥 对于CentOS发行版本来说: rpm --import /etc/pkl/rpm-gg/RPM-GPG-KEY-Centos-7

验证:
- 安装此组织签名的程序时,会自动执行验证
- 手动验证: rpm -K PACKAGE_FILE

##### 数组库重建
rpm管理器数据库路径: /var/lib/rpm
查询操作: 通过此处的数据库进行

获取帮助
- CentOS 6:man rpm
- CentOS 7:man rpmdb

rpm {--initdb|--rebuilddb}
- --initdb 初始化数据库,当前无任何数据库可初始化一个新数据库,当前有时不执行任务操作
- --rebuilddb 重建数据库 重新构建,通过读取当前系统上所有已经安装过的程序进行重新创建



