# Dockerfile

## From 命令

```text
From <image>:<tag>
```

用于设置基础镜像 ， 一般是Dockerfile的第一句

## MAINTAINER

```text
MAINTAINER <name>
```

用来指定维护者的姓名和联系方式

## RUN

```text
RUN <command> 或者 RUN ["executable", "param1", "param2"]
```

每条RUN指令将在当前镜像基础上执行指定命令，并提交为新的镜像。

## ADD

```text
ADD <src> <dest>
```

将文件复制到文件：是相对被构建的源目录的相对路径，可以是文件或目录的路径，也可以是一个远程的文件url, 是容器中的绝对路径

## COPY

```text
COPY <src> <desc>
```

复制本地主机的（为Dockerfile所在目录的相对路径）到容器中的，与ADD指令差不多

## ENTRYPOINT

```text
#推荐使用的exec形式
ENTRYPOINT ["executable", "param1", "param2"] 
#shell形式
ENTRYPOINT command param1 param2
```

一个Dockerfile中只能有一个ENTRYPOINT，如果有多个，则最后一个生效

## CMD

```text
#使用exec执行，推荐方式
CMD ["executable", "param1", "param2"] 
#在bin.sh中执行，提供给需要交互的应用
CMD command param1 param2 
#提供给ENTRYPOINT的默认参数
CMD ["param1", "param2"]
```

指定启动容器时执行的命令，每个Dockerfile只能有一条CMD命令。如果指定了多条命令，只有最后一条会被执行。 如果用户启动容器时候指定了运行的命令，则会覆盖掉CMD指定的命令

## WORKDIR

```text
WORKDIR /path/to/workdir
```

为后续的RUN,CMD，ENTRYPOINT指令配置工作目录。 可以使用多个WORKDIR指令，后续命令如果参数是相对路径，则会基于之前的命令指定的路径。例如：

```text
WORDKIR /a
WORDKIR b
WORDKIR c
RUN pwd
```

则最终路径为/a/b/c。

## EXPOSE

```text
EXPOSE <port> [<port>...]
```

告诉Docker服务端暴露的端口号，供互联系统使用。 例如

```text
#开放端口8080 和80
EXPOSE 8080 80
```

## ENV

```text
ENV <key> <value>
```

指定一个环境变理，会被后续的RUN指令合用，并在容器运行时保持。

## VOLUME

```text
VOLUME ["/data"]
```

创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。

## USER

```text
USER <UID/Username>
```

为容器内指定CMD RUN ENTRYPOINT命令运行时的用户名或UID。

## ONBUILD

```text
ONBUILD [INSTRUCTION]
```

配置当所创建的镜像作为其它新创建的镜像的基础镜像时，所执行的操作指令。 例如，利用Dockerfile创建一个镜像A，其中Dockerfile中有这么几个命令

```text
ONBUILD RUN mkdir test
ONBUILD ADD app.js /test/app.js
```

那么镜像B基于镜像A去构建的时候，默认会在最后加上这两行命令

```text
 FROM 镜像A
 RUN mkdir test
 ADD app.js /test/app.js
```

