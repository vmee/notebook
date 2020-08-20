# 常用命令

```text
# 只在 master 节点执行

# 执行如下命令，等待 3-10 分钟，直到所有的容器组处于 Running 状态
watch kubectl get pod -n kube-system -o wide

# 查看 master 节点初始化结果
kubectl get nodes -o wide
```

## 获取join命令参数

```text
# 只在 master 节点执行
kubeadm token create --print-join-command
```

## 删除一个节点

在worker节点运行

```text
kubeadm reset
```

在master节点运

```text
kubectl delete node demo-worker-x-x
```

