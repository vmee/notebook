# Dockerfile

## Format
- comment 注释
- INSTRUCTION arguments 指令+参数 指令区分参数建议大写
- 指令按顺序执行
- 第一个指令必须是 FROM  构建时要指定的基础镜像


## File
- 专用目录，不用使用当前目录外的文件，子目录下的可以
- Dockerfile文件名首字母大写
- .dockeringore 忽略打包文件



## instruct
### FROM 命令
```Dockerfile
From <image>:<tag>
```
用于设置基础镜像 ， 一般是Dockerfile的第一句

### MAINTAINER
```Dockerfile
MAINTAINER <name>
```
镜像作者
用来指定维护者的姓名和联系方式

> MAINTAINER "auther auther@mail.com"

### LABEL
为镜像指定元数据
> LABEEL  key=value key=value



### ADD
```Dockerfile
ADD <src> <dest>
```
将文件复制到文件：是相对被构建的源目录的相对路径，可以是文件或目录的路径，也可以是一个远程的文件url, 是容器中的绝对路径

准则
- 同COPY命令
- 如果src为url且dest不以/结尾，则src指定的文件将被下载并直接被创建为dest;如果dest以/结尾，则文件名URL指定的文件将被直接下载并保存为dest/filename
- 如果src是一个本地系统上的压缩格式的tar文件，它将被展开为一个目录，其行为类似于“tar -x”命令；然而通过url获取到的tar文件将不会自动展开
- 如果src有多个，或其间接或直接使用了通配符，则dest必须是以/结尾的目录路径; 如果dest不以/结尾，则其被视作一个普通文件,src的内容将被直接写入到dest


### COPY

把宿主机的文件复制到创建的新镜像的文件

```Dockerfile
COPY <src> <desc> #或
COPY ["<src>",... "<dest>" ] # 路径中有空白字符时使用
```
复制本地主机的（为Dockerfile所在目录的相对路径）到容器中的，与ADD指令差不多

准则
- src必须是build上下文的路径，不能是其父目录中的文件
- src若是目录，则其内部文件或子目录会被递归复制，但src目录自身不会被复制
- 若是指定了多个src,或src中使用了通配符，则desc必须是一个目录，且必须以/结尾
- 如果desc事先不存在，它将会被自动创建，这包括其父目录路径

### ENTRYPOINT
```Dockerfile
#推荐使用的exec形式
ENTRYPOINT ["executable", "param1", "param2"] 
#shell形式
ENTRYPOINT command param1 param2  
```
一个Dockerfile中只能有一个ENTRYPOINT，如果有多个，则最后一个生效

### RUN
```Dockerfile
RUN <command> 或者 RUN ["executable", "param1", "param2"] 
```
每条RUN指令将在当前镜像基础上执行指定命令，并提交为新的镜像。

> RUM COMMAND 1 && \
> COMMAND 2 && COMMAND 3

### CMD
```Dockerfile
#使用exec执行，推荐方式
CMD ["executable", "param1", "param2"] 
#在bin.sh中执行，提供给需要交互的应用
CMD command param1 param2 
#提供给ENTRYPOINT的默认参数
CMD ["param1", "param2"]
```
指定启动容器时执行的命令，每个Dockerfile只能有一条CMD命令。如果指定了多条命令，只有最后一条会被执行。
如果用户启动容器时候指定了运行的命令，则会覆盖掉CMD指定的命令
### WORKDIR
```Dockerfile
WORKDIR /path/to/workdir
```
为后续的RUN,CMD，ENTRYPOINT指令配置工作目录。
可以使用多个WORKDIR指令，后续命令如果参数是相对路径，则会基于之前的命令指定的路径。例如：
```Dockerfile
WORDKIR /a
WORDKIR b
WORDKIR c
RUN pwd
```
则最终路径为/a/b/c。

### EXPOSE 
```Dockerfile
EXPOSE <port>[/<protocol>] [<port>[/<protocol>]...]
```
告诉Docker服务端暴露的端口号，供互联系统使用。
例如 
```Dockerfile
#开放端口8080 和80
EXPOSE 8080/tcp 80/udp
```

```sh
docker run -P //暴露端口
```

### ENV

指定一个环境变理，会被后续的RUN指令合用，并在容器运行时保持。
format: $variable_name 或 ${variable_name}

```Dockerfile
ENV <key> <value> 或
ENV <key>=<value>...
```
第一种格式key之后的所有内容均被视为value的组成部分，因此一次只能设置一个变量;
第二种格式可用一次设置多个变理，每个变理为一个key=value的键值对，如果value中含有空格可用反斜线(\）进行转义，也可通过对value加引号进行标识;另外反斜线也用于续行;
定义多个变量时，建议使用第二种方式，以便在同一层中完成所有功能


### VOLUME
```Dockerfile
VOLUME ["/data"]
```
创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。

### USER
```Dockerfile
USER <UID/Username>
```
为容器内指定CMD RUN ENTRYPOINT命令运行时的用户名或UID。

### ONBUILD
```Dockerfile
ONBUILD [INSTRUCTION]
```
配置当所创建的镜像作为其它新创建的镜像的基础镜像时，所执行的操作指令。
例如，利用Dockerfile创建一个镜像A，其中Dockerfile中有这么几个命令
```Dockerfile
ONBUILD RUN mkdir test
ONBUILD ADD app.js /test/app.js
```
那么镜像B基于镜像A去构建的时候，默认会在最后加上这两行命令
```Dockerfile
 FROM 镜像A
 RUN mkdir test
 ADD app.js /test/app.js
```