# 常用网络命令


由于nio的普及，ck10k的问题已经成为过去式。现在随便一台服务器，就可以支持数十万级别的连接了。那么我们来算一下，100万的连接需要多少资源。
首先，每一个连接都是文件句柄，所以需要文件描述符数量支持才行，每一个socket内存占用15k-20k之间，这样，仅维护相应 socket，就需要20G内存；而广播一个1KB的消息需要占用的带宽为1000M！

## 查看当前系统的链接
如何看当前系统有多少连接呢？可以使用netstat结合awk进行统计。如下脚本，统计了每一种状态的tcp连接数量
```bash
$ netstat -antp | awk '{a[$6]++}END{ for(x in a)print x,a[x]}'

LISTEN 41
CLOSE_WAIT 24
ESTABLISHED 150
Foreign 1
TIME_WAIT 92
```

但如果你在一台有上万连接的服务器上执行这个命令，你可能会等上很长时间。所以，我们有了第二代网络状态统计工具：netstat => ss。
```bash
$ ss -s 
```
netstat属于net-tools工具集，而ss属于iproute。其命令对应如下，是时候和 net-tools 说 Bye 了。

## ss命令

基本使用

我们按照使用场景来看下ss的用法。

查看系统正在监听的tcp连接
```bash
$ ss -atr 
$ ss -atn #仅ip
```
查看系统中所有连接
```bash
$ ss -alt
```
查看监听444端口的进程 pid
```bash
$ ss -ltp | grep 444
```
查看进程555占用了哪些端口
```bash
$ ss -ltp | grep 555
```
显示所有 UDP 连接
```bash
$ ss -u -a
```
查看TCP sockets，使用-ta选项
查看UDP sockets，使用-ua选项
查看RAW sockets，使用-wa选项
查看UNIX sockets，使用-xa选项
和某个 IP 的所有连接
```bash
$ ss dst 10.66.224.130
$ ss dst 10.66.224.130:http
$ ss dst 10.66.224.130:smtp
$ ss dst 10.66.224.130:443
```
显示所有的 HTTP 连接
```bash
$ ss  dport = :http
```
查看连接本机最多的前 10 个 IP 地址
```bash
netstat -antp | awk '{print $4}' | cut -d ':' -f1 | sort | uniq -c  | sort -n -k1 -r | head -n 10
```

###  Recv-Q 和 Send-Q

注意ss的执行结果，我们说明一下Recv-Q和Send-Q。

这两个值，在LISTEN和ESTAB状态分别代表不同意义。一般，正常的应用程序这两个值都应该为0（backlog除外）。数值越大，说明问题越严重。

LISTEN 状态

- Recv-Q：代表建立的连接还有多少没有被accept，比如Nginx接受新连接变的很慢
- Send-Q：代表listen backlog值

ESTAB 状态

- Recv-Q：内核中的数据还有多少(bytes)没有被应用程序读取，发生了一定程度的阻塞
- Send-Q：代表内核中发送队列里还有多少(bytes)数据没有收到ack，对端的接收处理能力不强
## 查看网络流量

### 查看流量

有很多工具可以看网络流量，但我最喜欢sar。sar是linux上功能最全的监控软件。如图，使用sar -n DEV 1即可每秒刷新一次网络流量。

```bash
watch cat /proc/net/dev
```

### 查看占流量最大的 IP

有时候我们发现网络带宽占用非常高，但我们无法判断到底流量来自哪里。这时候，iftop就可以帮上忙了。如图，可以很容易的找出流量来自哪台主机。


当你不确定内网的流量来源，比如有人在压测，api调用不合理等，都可以通过这种方法找到他。
抓包
```bash
$ tcpdump
```
当我们需要判断是否有流量，或者调试一个难缠的 netty 应用问题，则可以通过抓包的方式去进行进一步的判断。在 Linux 上，可以通过 tcpdump 命令抓取数据，然后使用Wireshark 进行分析。
tcpdump -i eth0 -nn -s0 -v port 80
-i 指定网卡进行抓包
-n 和ss一样，表示不解析域名
-nn 两个n表示端口也是数字，否则解析成服务名
-s 设置抓包长度，0表示不限制
-v 抓包时显示详细输出，-vv、-vvv依次更加详细
1）加入-A选项将打印 ascii ，-X打印 hex 码。

tcpdump -A -s0 port 80
2）抓取特定 IP 的相关包

tcpdump -i eth0 host 10.10.1.1
tcpdump -i eth0 dst 10.10.1.20
3）-w参数将抓取的包写入到某个文件中

tcpdump -i eth0 -s0 -w test.pcap
4）tcpdump支持表达式，还有更加复杂的例子，比如抓取系统中的get,post请求（非https)

tcpdump -s 0 -v -n -l | egrep -i "POST /|GET /|Host:"
更多参见
https://hackertarget.com/tcpdump-examples/

抓取的数据，使用 wireshark 查看即可。



HTTP 抓包

抓包工具将自身当作代理，能够抓取你的浏览器到服务器之间的通讯，并提供修改、重放、批量执行的功能。是发现问题，分析协议，攻击站点的利器。常用的有以下三款：
Burpsuite （跨平台)
Fiddle2 (Win)
Charles (Mac)
流量复制

你可能需要使你的生产环境HTTP真实流量在开发环境或者预演环境重现，这样就用到了流量复制功能。

有三个工具可供选择，个人倾向于Gor。

Gor
TCPReplay
TCPCopy
连接数过多问题



根据TCP/IP介绍，socket大概包含10个连接状态。我们平常工作中遇到的，除了针对SYN的拒绝服务攻击，如果有异常，大概率是TIME_WAIT和CLOSE_WAIT的问题。
TIME_WAIT一般通过优化内核参数能够解决；CLOSE_WAIT一般是由于程序编写不合理造成的，更应该引起开发者注意。
TIME_WAIT

TIME_WAIT 是主动关闭连接的一方保持的状态，像 nginx、爬虫服务器，经常发生大量处于time_wait状态的连接。TCP 一般在主动关闭连接后，会等待 2MS，然后彻底关闭连接。由于 HTTP 使用了 TCP 协议，所以在这些频繁开关连接的服务器上，就积压了非常多的 TIME_WAIT 状态连接。
某些系统通过 dmesg 可以看到以下信息。
__ratelimit: 2170 callbacks suppressed
TCP: time wait bucket table overflow
TCP: time wait bucket table overflow
TCP: time wait bucket table overflow
TCP: time wait bucket table overflow
通过ss -s命令查看，可以看到timewait已经有2w个了。

ss -s
Total: 174 (kernel 199)
TCP:   20047 (estab 32, closed 20000, orphaned 4, synrecv 0, timewait 20000/0), ports 10785
sysctl 命令可以设置这些参数，如果想要重启生效的话，加入/etc/sysctl.conf文件中。
# 修改阈值
net.ipv4.tcp_max_tw_buckets = 50000
# 表示开启TCP连接中TIME-WAIT sockets的快速回收
net.ipv4.tcp_tw_reuse = 1
#启用timewait 快速回收。这个一定要开启，默认是关闭的。net.ipv4.tcp_tw_recycle= 1   
# 修改系統默认的TIMEOUT时间,默认是60s
net.ipv4.tcp_fin_timeout = 10
测试参数的话，可以使用 sysctl -w net.ipv4.tcp_tw_reuse = 1 这样的命令。如果是写入进文件的，则使用sysctl -p生效。

CLOSE_WAIT

CLOSE_WAIT一般是由于对端主动关闭，而我方没有正确处理的原因引起的。说白了，就是程序写的有问题，属于危害比较大的一种。

我们拿”csdn 谐音太郎”遇到的一个典型案例来说明。



代码是使用HttpClient的一个使用片段。在这段代码里，通过调用in.close()来进行连接资源的清理。但可惜的是，代码中有一个判断：非200状态的连接直接返回null。在这种情况下，in连赋值的机会都没有，当然也就无法关闭，然后就发生了连接泄漏。
所以，HttpClient的正确关闭方式是使用其api：abort()。
其他常用命令

应用软件

# 断点续传下载文件
wget -c $url
# 下载整站
wget -r -p -np -k $url
# 发送网络连接（常用)
curl -XGET $url
# 传输文件
scp
sftp
# 数据镜像备份
rsync
检测工具

# 连通性检测
ping google.com
# 到对端路由检测
tracepath google.com
# 域名检测
dig google.com
nslookup google.com
# 网络扫描工具
nmap
# 压力测试
iperf
# 全方位监控工具（好东西)
nmon
配置工具

# 停止某个网卡
ifdown
# 开启某个网卡
ifup
# 多功能管理工具
ethtool
压力测试

wrk
ab
webbench
http_load
多功能工具

# 远程登录
telnet
ssh
nc
# 防火墙
iptables -L
结尾

除了基本的工具，本文提到的很多网络命令，都不是预装的，需要使用yum自行安装。网络编程方面的学习，我觉得，读一下《TCP/IP详解 卷1：协议》这本书，然后写几个Netty应用就可以了。
NIO我们已经在I/O篇提起了，在此不再做详细介绍。等你碰到所谓的拆包粘包问题，遇到心跳和限流问题，甚至遇到了流量整形问题，那么证明你离一个专业的网络编程程序员越来越近了。

