# Substrate

波卡
要把不同的区块链连接到一起 跨链

解决的问题
- 跨链协作
- 可扩展怀
- 共享安全


通用  易用 易修改

区块链链节点需要
- 数据库
- 点对点网络
- 共识算法
- 交易处理
- 状态转换函数(Runtime)



核心组件
- 数据库
- 点对点网络
- 加密算法
- 序列化
- 共识算法
- 交易队列

定制化自由度很高
- 区块链的基础/核心组件
- 治理\升级模型 链逻辑升级 链上治理
- 智能合约

## Substrate包含什么
核心模块
- 数据库
- 交易队列
- 命令行界面
- 公/密钥生成
- RPC

基本逻辑
- 数据结构
- 结算
- 时间戳

p2p网络
- 网络节点管理
- 私讯协议集成
- 分布式哈希表

共识机制
- 抵押
- Babe
- Grandpa
- 区块落实追踪

链上治理
- 民主
- 投票
- 议会
- 国库

Polkadot


中文学习资料
- b站 https://space.bilibili.com/67358318
- 知乎 https://zhuanlan.zhihu.com/substrate
- 知乎 https://zhuanlan.zhihu.com/v2web3
- 公众号 polkadot中文平台
- 公众号 Polkaworld
- 公众号 Polkabase


如何学习Substrate https://zhuanlan.zhihu.com/p/161771205


## 启动
下载代码
```sh
$ git clone https://github.com/paritytech/substrate.git
$ cargo build --release #编译
$ ./target/release/substrate --dev #启动dev网络
$ ./target/release/substate purge-chain --dev # 删除存储目录
```
打开网站
https://polkadot.js.org
点击 右上角polkadot substrate app

### 出现错误"Rust nightly not installed, please install it!"

The latest Rust nightly is broken for MacOS.

To solve this for Substrate, run the following:
```sh
$ rustup toolchain install nightly-2020-08-23
$ rustup target add wasm32-unknown-unknown --toolchain nightly-2020-08-23
```
Then make sure to compile your node with:
```sh
$ cargo +nightly-2020-08-23 build --release
```

## 启动两个节点
```sh
$ ./target/release/substrate --alice --chain local --base-path=/tmp/alice
$ ./target/release/substrate --bob --chain local --base-path=/tmp/alice
```


## substrate node template

```sh
$ git clone https://github.com/SubstrateCourse/substrate-node-template.git #下载代码
$ cargo build --release #编译
$ ./target/release/node-template purge-chain --dev #清除本地数据
$ ./target/release/node-template --dev
```