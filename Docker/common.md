# 常用命令

1. 杀死所有正在运行的容器
   ```bash
   docker kill $(docker ps -a -q)
   ```
2. 删除所有已经停止的容器
   ```bash
   docker rm $(docker ps -a -q)
   ```
3. 删除所有镜像
   ```bash
   docker rmi $(docker images -q)
   ```
4. 关闭容器
   ```bash
   docker stop CONTAINER ID或者NAMES
   ```
5. 重新启动关闭容器
   ```bash
   docker start CONTAINER ID或者NAMES
   ```
6. 移除本地容器
   ```bash
   docker rm CONTAINER ID或者NAMES
   ```
7. 查看本地容器
   ```bash
   docker ps // 查看正在运行的容器
   docker ps -a //查看所有容器
   ```
8. 查看本地镜像
   ```bash
   docker images
   ```
9. 创建镜像
   ```bash
   docker build -t name:tag Dockerfile路径
   ```
10. 修改本地镜像标记
   ```bash
   docker tag IMAGE ID name:tag
   docker rmi name:tag
   ``` 
11. 删除本地镜像 
   ```bash
   docker rmi name:tag或者IMAGE ID
   ```
12. 进入容器
   ```bash
   docker exec -it IMAGE ID或者NAMES /bin/bash
   ```
13. 获取镜像中心的镜像
   ```bash
   docker pull name:tag
   ```
14. 获取窗口的端口的映射配置
   ```bash
   docker port CONTAINER ID或者NAMES
   ``` 
15. 要是dockerfile运行过程中出错，会在images中生成<none>的无用镜像，删除方法来自百度粘贴
   ```bash
   # 删除命令：
   docker rmi $(docker images | grep "none" | awk '{print $3}')
   # 上一步报错还有未停掉的容器后可进行下面的步骤
   # 停止容器
   docker stop $(docker ps -a | grep "Exited" | awk '{print $1 }') 
   # 删除：
   docker rm $(docker ps -a | grep "Exited" | awk '{print $1 }')
   # 删除镜像
   docker rmi $(docker images | grep "none" | awk '{print $3}')
   ```    
