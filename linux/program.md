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


安装
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
  