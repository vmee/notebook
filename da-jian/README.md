# Kubernetes

## 常用两种部署方式

* 传统方式 手动部署 组件做为独立进程 部署较为麻烦，要设置多组CA,若是进程挂掉的要手动启动
* kubeadm方式 组件部署为pod 部署较为简单，组件托管给k8s

## kubeadm

1. master,nodes: 安装kubelet, kubeadm, docker
2. master: kubeadm init

   [https://github.com/kubernetes/kubeadm/blob/master/docs/design/design\_v1.10.md](https://github.com/kubernetes/kubeadm/blob/master/docs/design/design_v1.10.md)

3. nodes: kubeadm join

> 时间同步服务是基础 确保iptables和firewalld 都没有启动

### docker repo

yum 镜像设置

```text
cd /etc/yum.repos.d/
wget http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### kubernetes repo

```text
cd /etc/yum.repos.d
vim kubernetes.repo
```

在kubernetes.repo里写入

```yaml
[kubernetes]
name=Kubernetes Repo
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
gbgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
enabled=1
```

## install

```bash
yum install docker-ce kubelet kubeadm kubectl
```

遇到错误

```text
Public key for 14bfe6e75a9efc8eca3f638eb22c7e2ce759c67f95b43b16fae4ebabde1549f3-cri-tools-1.13.0-0.x86_64.rpm is not installed


 Failing package is: cri-tools-1.13.0-0.x86_64
 GPG Keys are configured as: http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
```

手动导入gpg

```text
 wget http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
 rpm --import yum-key.gpg
 wget http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
 rpm --import rpm-package-key.gpg
```

再次运行

```bash
yum install docker-ce kubelet kubeadm kubectl
```

```text
vim /usr/lib/systemd/system/docker.service
```

```text
[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
Environment="HTTPS_PROXY=http://www.ik8s.io:10080" #这里新增
Environment="NO_PROXY=127.0.0.0/8,172.16.0.0/16" #这里新增
```

```bash
systemctl daemon-reload
systemctl  enable docker
systemctl  enable kubelet
systemctl start docker
```

若是有提示

```text
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
```

确保下面的文件的值都为1

```text
/proc/sys/net/bridge/bridge-nf-call-iptables
/proc/sys/net/bridge/bridge-nf-call-ip6tables
```

修改办法

```text
vi /etc/sysctl.conf
```

添加以下内容

```text
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
```

然后执行

```text
sysctl -p
```

```text
systemctl enable docker
```

由于kubernetes默认不允许使用swap所以修改文件

```text
vim /etc/sysconfig/kubelet
```

添加

```text
KUBELET_EXTRA_ARGS="--fail-swap-on=false"
```

run

```text
kubeadm init --kubernetes-version=v1.18.6 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/12 --ignore-preflight-errors=Swap
```

## 参考文档

[https://kuboard.cn](https://kuboard.cn/install/install-k8s.html#%E6%96%87%E6%A1%A3%E7%89%B9%E7%82%B9)

## kubeadm安装

### 说明

#### 集群

单master节点 一个master节点 两个worker节点

#### 软件版本

Kubernetes v1.18.x calico 3.13.1 nginx-ingress 1.5.5 Docker 19.03.8

#### 配置要求

3台2核VM CentOS7.8

### 检查

#### 检查hostname

```text
# 此处 hostname 的输出将会是该机器在 Kubernetes 集群中的节点名字
# 不能使用 localhost 作为节点的名字
hostname
```

#### 检查Centos

检查linue版本

```text
# 在 master 节点和 worker 节点都要执行
cat /etc/redhat-release
```

查看cpu

```text
# 请使用 lscpu 命令，核对 CPU 信息
# Architecture: x86_64    本安装文档不支持 arm 架构
# CPU(s):       2         CPU 内核数量不能低于 2
lscpu
```

#### 检查网络

所有节点执行

```text
ip route show
```

所有节点上 Kubernetes 所使用的 IP 地址必须可以互通（无需 NAT 映射、无安全组或防火墙隔离）

#### 关闭防火墙

centos7.6启用的防火墙一般都是firewalld

```text
systemctl stop firewalld
systemctl disable firewalld
```

查看firewall状态

```text
firewall-cmd --state
# not running 若是running 请用kill手动关闭
```

#### 所有节点开启时间同步服务

ntp [时间同步设置](https://blog.csdn.net/qq_38591756/article/details/85243965)

#### 设置hosts

各节点都配置 192.168.31.134 apiserver.name 192.168.31.134 master134 192.168.31.188 worker188 192.168.31.24 worker24

#### 请认真核对如下选项，7 个都确认可以后才能安装。

安装条件检查

* 我的任意节点 centos 版本为 7.6 或 7.7
* 我的任意节点 CPU 内核数量大于等于 2，且内存大于等于 4G
* 我的任意节点 hostname 不是 localhost，且不包含下划线、小数点、大写字母
* 我的任意节点都有固定的内网 IP 地址
* 我的任意节点都只有一个网卡，如果有特殊目的，我可以在完成 K8S 安装后再增加新的网卡
* 我的任意节点上 Kubelet使用的 IP 地址 可互通（无需 NAT 映射即可相互访问），且没有防火墙、安全组隔离
* 我的任意节点不会直接使用 docker run 或 docker-compose 运行容器
* 我的任意节点的防火墙都已关闭
* 我的任意节点的时间同步服务都已开启

### 安装docker-ce kubelet kubeadm kubectl

> worker节点可不安装kubectl

#### yum仓库地址配置

**docker repo**

yum 镜像设置

```text
cd /etc/yum.repos.d/
wget http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

**kubernetes repo**

```text
cd /etc/yum.repos.d
vim kubernetes.repo
```

在kubernetes.repo里写入

```yaml
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
       http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
```

```text
# 配置K8S的yum源
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
       http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```

检查镜像

```text
yum repolist
```

#### 安装

镜像无问就可以安装了

```text
 yum install docker-ce kubelet kubeadm kubectl
```

### 启动docker

启动

```text
systemctl enable docker
systemctl start docker
```

#### 设置网络参数

```text
# 修改 /etc/sysctl.conf
# 如果有配置，则修改
sed -i "s#^net.ipv4.ip_forward.*#net.ipv4.ip_forward=1#g"  /etc/sysctl.conf
sed -i "s#^net.bridge.bridge-nf-call-ip6tables.*#net.bridge.bridge-nf-call-ip6tables=1#g"  /etc/sysctl.conf
sed -i "s#^net.bridge.bridge-nf-call-iptables.*#net.bridge.bridge-nf-call-iptables=1#g"  /etc/sysctl.conf
sed -i "s#^net.ipv6.conf.all.disable_ipv6.*#net.ipv6.conf.all.disable_ipv6=1#g"  /etc/sysctl.conf
sed -i "s#^net.ipv6.conf.default.disable_ipv6.*#net.ipv6.conf.default.disable_ipv6=1#g"  /etc/sysctl.conf
sed -i "s#^net.ipv6.conf.lo.disable_ipv6.*#net.ipv6.conf.lo.disable_ipv6=1#g"  /etc/sysctl.conf
sed -i "s#^net.ipv6.conf.all.forwarding.*#net.ipv6.conf.all.forwarding=1#g"  /etc/sysctl.conf
# 可能没有，追加
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
echo "net.bridge.bridge-nf-call-ip6tables = 1" >> /etc/sysctl.conf
echo "net.bridge.bridge-nf-call-iptables = 1" >> /etc/sysctl.conf
echo "net.ipv6.conf.all.disable_ipv6 = 1" >> /etc/sysctl.conf
echo "net.ipv6.conf.default.disable_ipv6 = 1" >> /etc/sysctl.conf
echo "net.ipv6.conf.lo.disable_ipv6 = 1" >> /etc/sysctl.conf
echo "net.ipv6.conf.all.forwarding = 1"  >> /etc/sysctl.conf
# 执行命令以应用
sysctl -p
```

#### 设置docker镜像

修改 /etc/docker/daemon.json 文件（如果没有，则创建）：

```text
vim /etc/docker/daemon.json
```

```javascript
{
 "registry-mirrors": ["https://registry.cn-hangzhou.aliyuncs.com"]
}
```

配置生效

```text
systemctl daemon-reload
systemctl restart docker
```

```text
docker info
# 查看结果
```

### master节点初始化

#### Swap禁用或错误忽略

kubernetes默认是不让开启swap的 但是我们由于是用的虚拟测试机，配置较低， 不用swap的话， 性能会有大降低的，所以我们这里忽略错误

错误忽略

```text
vim /etc/sysconfig/kubelet
```

增加内容

```text
KUBELET_EXTRA_ARGS="--fail-swap-on=false"
```

```text
# 忽略[ERROR Swap]: running with swap on is not supported. Please disable swap
kubeadm init --ignore-preflight-errors=Swap
```

### 初始化worker节点

#### Swap禁用或错误忽略

kubernetes默认是不让开启swap的 但是我们由于是用的虚拟测试机，配置较低， 不用swap的话， 性能会有大降低的，所以我们这里忽略错误

错误忽略

```text
vim /etc/sysconfig/kubelet
```

增加内容

```text
KUBELET_EXTRA_ARGS="--fail-swap-on=false"
```

```text
# 忽略[ERROR Swap]: running with swap on is not supported. Please disable swap
kubeadm init --ignore-preflight-errors=Swap
```

在master节点上执行

```text
# 只在 master 节点执行
kubeadm token create --print-join-command
```

```text
W0801 18:27:52.310225   32186 configset.go:202] WARNING: kubeadm cannot validate component configs for API groups [kubelet.config.k8s.io kubeproxy.config.k8s.io]
kubeadm join apiserver.name:6443 --token uxijt6.o96l216kfnah55gg     --discovery-token-ca-cert-hash sha256:aa3e9631e8b0a5f478ece1ca4ab78110b50137396475527afc74508a2ffd5ceb
```

设置worker节点hosts

