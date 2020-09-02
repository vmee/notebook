# DNS


## DNS and Bind
Sockets：
- c/s
    client: 发起应用请求的程序
    server: 响应请求的程序（提供服务的程序） Listen Socket

- 传输层协议：TCP、UDP、SCTP
    TCP: Transmission Control Protocol: 面向连接的协议 双方通信之前需要先建立虚连接
    UCP：User Datagram Protocol 无连接的协议 双方通信之前无须建立虚连接 

DNS：Domain Name Service, 协议 c/s架构
使用53/udp, 53/tcp

tld: Top Level Domain
- 组织域
- 国家域

DNS查询类型：
- 递归查询
- 迭代查询

DNS服务器类型：
- 负责解析至少一个域
  - 主名称服务器
  - 辅助名称服务器
- 不负责域解析： 缓存名称服务器

一次完整的查询请求经过的流程：
Client --> hosts文件 --> DNS Local Cache -> DNS Server(recursion) ->
    自己负责的域：直接查询数据库并返回结果
    不是自己负责的域名： SeverCache->Iteration(迭代)

解析答案
    肯定答案
    否定答案：不存在查询的键

    权威答案：由直接负责的DNS服务器返回的答案
    非权威答案：其他 比如由缓存服务器返回的结果

主-辅DNS服务器：
- 主DNS服务器：维护，读写操作均可
- 从DNS服务器：从主或从从复制同步，只读
  - 复制同步方式
    - 序列号：数据版本，版本号递增
    - 最新时间间隔： refresh
    - 重试时间间隔：retry
    - 过期时长： expire , 停止提供服务
    - 否定答案的缓存时长
  - 主服务器通知从服务器随时更新

  - 区域传送
    - 全量传送
    - 增量传送
区域(zone)和域(domain)

DNSSec机制：安全，签名校验

区域数据库文件
- 资源记录：Resource Record 简称rr
  - 记录有类型：A,AAAA, PTR, SOA, NS, CNAME, MX
  - SOA: start of Authority 起始授权记录，一个区域解析库只能有一个SOA记录，而且必须放在第一条
  - NS：Name Service,域名服务记录，一个区域解析库可以 有多个NS记录;其中一个为主
  - A：Address, 地址记录 FQDN-> IPv4
  - AAAA: 地址记录，FQDN->IPv6
  - CNAME: Canonical Name, 别名记录
  - PTR: Pointer ip -> FQDN
  - MX: Maix Exchanger, 邮件交换器
    - 优先级：0-99 数字越小优先级越高

资源记录的定义格式
> 语法： name [TTL] IN RR_TYPE value
  
SOA:
    - name： 当前区域的名称 xxx.com./2.3.4.1.in-addr.arpa.
    - value：有多部分组成
      - 当前区域的区域名称（也可以使用主DNS的服务名称）
      - 当前区域的管理的邮箱地址，但地址不能合用使用@符号，一般使用点号代替
      - 主从服务协调属性的定义以及否定答案的TTL
例如：xxx.com. 864000 IN SOA xxx.com. admin.xxx.com. (
            20202021 ; serial
            2H  ;refresh
            10M ;retry
            1w  ;expire
            1D  ;negative answer ttl
  )

NS:
- name: 当前区域的区域名称
- value: 当前区域的某DNS服务器的名称 ，例如：ns.xxx.com.;
注意：一个区域可以有多个NS记录
例如：
  xxx.com. 86400 in NS ns1.xxx.com.
  xxx.com. 86400 in NS NS2.xxx.com.

MX:
- name: 当前区域的区域名称
- value： 当前区域的某邮件的交换器的主机名
注意：mx记录可以有多个，但每个记录的value之前应该一个数字的优先级

例如：
xxx.com. IN MX 10 mx1.xxx.com.
xxx.com. IN MX 20 mx2.xxx.com

A:
- name: 某FQDN 如www.xxx.com.
- value: ipv4

例如：
www.xxx.com. IN A 1.1.1.1
www.xxx.com. IN A 2.2.2

AAAA:
- name: 某FQDN 如www.xxx.com.
- value: ipv6
例如： 同上


PTR
- name: IP地址 有特定格式 ip反过来写，而且加特定后缀，如1.2.3.4记录应该写成4.3.2.1.in-addr.arpa
- value: FQDN

例如：4.3.2.1.in-addr.arpa in PTR www.xxx.com.

CNAME
- name : FQDN格式的别名
- value:FQDN正式的名字

例如：
web.xxx.com. IN CNAME www.xxx.com.


注意
- TTL可以从全局继承
- @表示当前区域的名称
- 相邻两个记录的名称相同时，后面的可省略
- MX、NS等 类型的记录的value为一个FQDN，此FADN应该有一个A记录


## 如何安装配置使用DNS服务
BIND的安装配置
BIND：Berkeley Internet Name Domain， ISC.org
DNS: 协议
BIND： dns协议的一种实现
NameD: 

## 测试工具
- dig
- host
- nslookup

### dig

dig [-t RR_TYPE] name [@SERVER] [query option]
用于测试dns系统，因此其不会查询hosts文件

查询选项：
+[no]trace: 跟踪解析过程
+[no]recurse:进行递归解析


反向解析测试
```sh
$ dig -x ip
```
模拟安全区域传送
```sh
dig -t axfr DOMAIN [@server]
```

### host

host [-t RR_TYPE] name SERVER_IP

### nslookup

nslookup [-options] [name] [server]

交互式模式：
  nslookup>
    server IP: 以指定的IP为DNS服务器进行查询;
    set q=RR_TYPE: 要查询的资源记录类型
    name: 要查询的名称

### rndc: named服务控制命令
rndc stats
rndc flush 

## 配置解析一个正向区域
- 定义区域
- 建议区域数据文件
- 让服务器重载配置文件和区域数据文件


## 学习资源
[dns](https://juejin.im/post/5c88b8f7518825068d1d1d7e)