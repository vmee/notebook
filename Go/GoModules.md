# Go Mudules

Go modules (前身 vgo) 是 Go team (Russ Cox) 强推的一个理想化的类语言级依赖管理解决方案，
为淘汰GOPATH而生
Opt-in式设计

它是和 Go1.11 一同发布的，在 Go1.13 做了大量的优化和调整，目前已经变得比较不错，

如果你想用 Go modules，但还停留在 1.11/1.12 版本的话，强烈建议升级。

[官方wiki](https://github.com/golang/go/wiki/Modules)


Go modules 的出现也解决了在 Go1.11 前的几个常见争议问题：
- Go 语言长久以来的依赖管理问题。
- “淘汰”现有的 GOPATH 的使用模式。
- 统一社区中的其它的依赖管理工具（提供迁移功能）。


### go get

- 拉取最新的版本(优先择取 tag)：go get golang.org/x/text@latest
- 拉取 master 分支的最新 commit：go get golang.org/x/text@master
- 拉取 tag 为 v0.3.2 的 commit：go get golang.org/x/text@v0.3.2
- 拉取 hash 为 342b231 的 commit，最终会被转换为 v0.3.2：go get golang.org/x/text@342b2e
- 用 go get -u 更新现有的依赖

。

### go mod

- 用 go get -u 更新现有的依赖
- 用 go mod download 下载 go.mod 文件中指明的所有依赖
- 用 go mod tidy 整理现有的依赖
- 用 go mod graph 查看现有的依赖结构
- 用 go mod init 生成 go.mod 文件
- 用 go mod edit 编辑 go.mod 文件
- 用 go mod vendor 导出现有的所有依赖 (事实上 Go modules 正在淡化 Vendor 的概念)
- 用 go mod verify 校验一个模块是否被篡改过



### 快速迁移项目到 Go Modules
第一步: 升级到 Go 1.14。
第二步: 让 GOPATH 从你的脑海中完全消失，早一步踏入未来。

- 修改 GOBIN 路径（可选）：go env -w GOBIN=$HOME/bin。
- 打开 Go modules：go env -w GO111MODULE=on。
- 设置 GOPROXY：go env -w GOPROXY=https://goproxy.cn,direct # 在中国是必须的，因为它的默认值被墙了。

第三步(可选): 按照你喜欢的目录结构重新组织你的所有项目。
第四步: 在你项目的根目录下执行 go mod init <OPTIONAL_MODULE_PATH> 以生成 go.mod 文件。
第五步: 想办法说服你身边所有的人都去走一下前四步。