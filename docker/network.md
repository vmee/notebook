# Network

6种命名空间： UTS，User， Mount， IPC，Pid， Net
网络虚拟化：OpenVSwitch,
Overlay Network: 叠加网络， 隧道转发

物理桥接： 把物理网卡当交换机



## Bridge
Docker安装后，会自动生成虚拟网卡docker0, docker0可能当网卡也可以做交换机

每运行一个Docker Container会生成一对网卡，一个在主机上，一个在Container里
```sh
$ ifconfig #可以查看所有主机上网卡
```

查看所有Container生成的网卡与docker9网卡关系，可以合用brctl工具

```sh
$ yum -y install bridge-utils #安装
$ brctl show #展示
```

## 四种
- 封闭式容器网络： loopback interface
- 桥接 : Private Inteface / loopback inteface  一半在桥上， 一半在容器里
- 联盟式容器 ：部分隔离 共享UTS，NET ，IPC ，可以本地通信
- 开放式 直连物理网卡 共享宿主机网络

