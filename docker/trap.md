# 踩坑

## Docker端口映射成功，但仍无法访问

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
69864b4bcb82        gotest              "/bin/sh -c /gotest-…"   7 minutes ago       Up 7 minutes        0.0.0.0:8088->8088/tcp   stoic_varahamihira
```

### 无法访问原因

docker里面的host不能配置127.0.0.1 或者192.168.0.1 否则宿主机器将无法访问端口

#### go代码错误示例：

```go
http.HandleFunc("/", IndexHandler)
http.ListenAndServe("127.0.0.1:8088", nil)
```

127.0.0.1或者192.168.0.1都表示服务监听了具体的IP，0.0.0.0则是监听所有IP

#### go代码正确示例：

```go
http.HandleFunc("/", IndexHandler)
http.ListenAndServe("0.0.0.0:8088", nil)
```

