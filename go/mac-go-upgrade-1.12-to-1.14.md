# mac 系统 go 升级 1.12.6 到 1.14.4

### 升级
由于 brew 里版本还是 1.12，目前升级到 1.14 的话，需要从官网下载 pkd 包手动升级

1. 官网[下载 pkg 包](https://golang.org/dl/)
2. 下载后直接安装，默认安装目录是 /usr/local/go
3. 修改硬链接该文件

```zsh
    $ cd /usr/local/go
    $ ll go*
    $ rm go
    $ sudo ln -s /usr/local/go/bin/go go
    $ rm gofmt
    $ sudo ln -s /usr/local/go/bin/gofmt go
    $ go version
```
> go version go1.14.4 darwin/amd64

升级下 go doc

```zsh
go get -v -u golang.org/x/tools/cmd/godoc
ln -s $GOBIN/godoc godoc
```
4. 更改环境变理

若是你用默认 bash, 则修改 .bash_profile
```zsh
export GOROOT="/usr/local/go" 
```

然后重新加载使修改生效
```zsh
    $ source .bash_profile
```

若你使用的 oh-my-zsh， 则修改 .zshrc
```zsh
export GOROOT="/usr/local/go" 
```

然后重新加载使修改生效
```zsh
    $ source .zshrc
```

5. 删除旧版本的编译生成的文件
```zsh
    $ rm -rf $GOPATH/pkg
```

至此 Go 版本升级完成，但若想使用新版本 Go 跑起程序来还不行，你还要进一步对 Go 的 ENV 做配置

### Go Env设置

1. 开启 Go Modules
   
   你可以
   ```zsh
    $ go env -w GO111MODULE=on
   ```
   你也可以
   ```zsh
   export GO111MODULE=on
   ```
   你还可以设置系统环境变理

2. 设置 GOPROXY 
   
   默认是
   > https://proxy.golang.org,direct
   需要换成国内镜像 

   我这里用的七牛云的镜像站
   ```zsh
   $ go env -w GOPROXY=https://goproxy.cn,direct
   ```
   
   当然也可以用其他公有镜像

3. 设置 GOSUMDB
   默认值：sum.golang.org
   这个国内也无法访问

   ```zsh
   $ go env -w GOSUMDB="goproxy.cn"
   ```

4. 设置 GOPRIVATE 

    这里主要涉及 GONOPROXY/GONOSUMDB/GOPRIVATE 三个环境变量， 三个环境变量都是用在当前项目依赖了私有模块（non-public modules），也就是依赖了由 GOPROXY 指定的 Go module proxy 或由 GOSUMDB 指定 Go checksum database 无法访问到的模块时的场景

    其中 GOPRIVATE 较为特殊，它的值将作为 GONOPROXY 和 GONOSUMDB 的默认值，所以建议的最佳姿势是只是用 GOPRIVATE

   ```zsh
    $ go env -w GOPRIVATE="gitlab.xxx.com"
   ```
    > gitlab.xxx.comy为你的私有模块域名


show下我的go env
```
GO111MODULE="on"
GOARCH="amd64"
GOBIN="/Users/user/go/bin"
GOCACHE="/Users/user/Library/Caches/go-build"
GOENV="/Users/user/Library/Application Support/go/env"
GOEXE=""
GOFLAGS=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOINSECURE=""
GONOPROXY="gitlab.xxx.com"
GONOSUMDB="gitlab.xxx.com"
GOOS="darwin"
GOPATH="/Users/user/go"
GOPRIVATE="gitlab.mfwdev.com"
GOPROXY="https://goproxy.cn,direct"
GOROOT="/usr/local/go"
GOSUMDB="goproxy.cn"
GOTMPDIR=""
GOTOOLDIR="/usr/local/go/pkg/tool/darwin_amd64"
GCCGO="gccgo"
AR="ar"
CC="clang"
CXX="clang++"
CGO_ENABLED="1"
GOMOD="/Users/user/Projects/xxx/go.mod"
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/dj/6zwmzvhj73q5nm1nxsj8kxgm0000gp/T/go-build128048676=/tmp/go-build -gno-record-gcc-switches -fno-common"
```

现在你的程序应该可以跑起来了


### 参考文档
[精通Golang项目依赖Go modules](https://zhuanlan.zhihu.com/p/113506780)