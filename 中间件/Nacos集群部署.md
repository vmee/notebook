# Nacos集群部署

## 准备
### 硬件
- 3台虚拟机 2核4G

### 软件环境
- 64bit Linux系统 系统版本 Centos7.6
- 64bit JDK1.8
- Maven 3.6.3

### 关闭防火墙
centos7.6启用的防火墙一般都是firewalld

```shell
$ systemctl stop firewalld
$ systemctl disable firewalld
```

查看firewall状态
```shell
$ firewall-cmd --state
# not running 若是running 请用kill手动关闭
```

### 软件下载
##### JDK1.8
下载版本为jdk-8u261-linux-x64.tar.gz，由此[下载](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
##### Maven3.6.3
下载版本为apache-maven-3.6.3-bin.tar.gz	 由此[下载](https://maven.apache.org/download.cg)
##### Nacos
下载版本为nacos-server-1.3.0.tar.gz
官方下载: [zip包](https://github.com/alibaba/nacos/releases/download/1.3.0/nacos-server-1.3.0.zip) [tar.gz包](https://github.com/alibaba/nacos/releases/download/1.3.0/nacos-server-1.3.0.tar.gz)

官方速度下载特别慢，我是在网盘里下的你 [Baidu Netdisk](https://pan.baidu.com/s/1186nmlqPGows9gUZKAx8Zw) Fetch Code : rest

## 环境安装 JDK和Maven

解压安装包
```sh
$ tar zxf jdk-8u261-linux-x64.tar.gz -C /usr/local/
$ tar zxf apache-maven-3.6.3-bin.tar.gz -C /usr/local/
```

配置
```sh
$ vim /etc/profile.d/jdkmaven.sh 
```
写入以下内容
```
export JAVA_HOME=/usr/local/jdk1.8.0_261
export MAVEN_HOME=/usr/local/apache-maven-3.6.3
export PATH=$JAVA_HOME/bin:$MAVEN_HOME/bin:$PATH
```
保存后
```sh
$ source /etc/profile.d/jdkmaven.sh 
```

验证
```sh
$ java -version
# 看到以下输出
java version "1.8.0_261"
Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode)

$ mvn -v
# 看到以下输出
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /usr/local/apache-maven-3.6.3
Java version: 1.8.0_261, vendor: Oracle Corporation, runtime: /usr/local/jdk1.8.0_261/jre
Default locale: en_US, platform encoding: ANSI_X3.4-1968
OS name: "linux", version: "4.4.188-1.el7.elrepo.x86_64", arch: "amd64", family: "unix"
```

## Nacos安装
```sh
$ tar zxf nacos-server-1.3.0.tar.gz -C /usr/local/
```

### 单机模式启动测试
```sh
$ cd /usr/local/nacos/bin
$ sh startup.sh -m standalone
```
查看启动日志
```sh
$ tail -f /usr/local/nacos/logs/start.out
```
查看控制台 地址：http://ip:8848/nacos 看到界面则启动成功了
默认密码nacos/nacos


## 集群安装
### 所有节点安成以下步骤
- 所有节点完成[准备阶段](#准备)
- 所有节点完成[环境安装](#环境安装-jdk和maven)
- 所有节点完成[Nacos安装](#nacos安装)

配置cluster.conf
```sh
$ cp /usr/local/nacos/conf/cluster.conf.example /usr/local/nacos/conf/cluster.conf
$ vim /usr/local/nacos/conf/cluster.conf
```
写入内容如下：
```
#it is ip
#example
172.16.130.118
172.16.130.119
172.16.130.120
```


### 使用内置数据源启动
```sh
$ sh /usr/local/nacos/bin/startup.sh -p embedded
```

查看启动日志
```sh
$ tail -f /usr/local/nacos/logs/start.out
```
查看控制台 地址：http://ip:8848/nacos 看到界面则启动成功了
默认密码nacos/nacos

在控制台里集群管理 节点列表里，可以看到所有启动的节点，若是所有节点状态为UP,则表示成功

## 参考
[官方集群说明](https://nacos.io/zh-cn/docs/cluster-mode-quick-start.html)