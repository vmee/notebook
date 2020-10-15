# 基础知识

## 命令
### 命令的语法通用格式
> COMMAND OPTIONS ARGUMENTS

发起一命令： 请求内核将某个二进程程序运行为一个进程
程序 --> 进程
静态 --> 动态（有生命周期）

OPTIONS：指定命令的运行特性
选项有两种表现形式
- 短选项 -C  例如-l,-d
  - 有些命令的选项没有-
  - 如果同一命令同时使用多个短选项，多数可合并 ：-l, -d = -ld
- 长选项 --word --help --Human-readable
  - 长选项不能合并

注意：有些选项可带参数，此称为选项参数

ARGUMENTS：命令的作用对象，命令对什么生效
注：不同的命令的参数，有些命令可同时带多个参数，多个之间以空白字符分隔


### 命令的帮助
- 内部命令 help COMMAND
- 外部命令 
  - 命令自带简要格式的使用帮助 $ COMMAND --help
  - 使用手册： manual
    - 目录/usr/share/man
    - man COMMAND
    - MANUAL SPECTION
      - name 命令的名称或简短使用
      - SYNOPSIS 简要命令的格式
        - []： 可选内容
        - <>: 必须提供的内容
        - a|b|c: 多选一
        - {} 分组
        - ...: 同类内容出现多个
      - DESCRIPTION 描述
      - OPTIONS 选项
      - EXAMPLES 使用示例
      - AUTHOR 作者
      - BUGS 报告bug的方式
      - SEE ALSO 参考
    - 使用手册：压缩格式的文件，有章节之分，
      -  /usr/share/man /usr/share/man1 /usr/share/man2...
         1. 内部命令
         2. 系统命令
         3. c库调用
         4. 设备文件及特殊文件
         5. 文件格式：（配置文件）
         6. 游戏使用帮助
         7. 杂项
         8. 管理工具及守护进程
      - man CHAPTAER COMMAND
        - 注意：并非每个COMMAND在所有章节下都有手册
        - 查看手册： $ whatis COMMAND 注意：其他执行过程是查询数据库进行的，手动更新数据库:$ makewhatis
      - man命令打开手册后的操作方法
        - 翻屏
          - 空格键：向文件尾翻一屏
          - b: 向文件首部翻一屏
          - Ctrl+d: 向文件尾部翻半屏
          - Ctrl+u: 向文件首部翻工屏
          - 回车键： 向文件尾部翻一行
          - k: 向文件首部翻一行
          - G：跳转至最后一行
          - #G： 跳转到指定行
          - 1G： 跳转到文件首部
        - 文本搜索
          - /keyword: 从文件首部向文件尾部依次查找，查找不区分大小写
          - ?keyword: 从文件尾部问向文件首部依次查询
            - n: 与查找命令方向相同
            - N：与查找命令相反
        - 退出
          - q： quit
      - 选项 -m /PATH/TO/SOMEDIR: 到指定目录下查看手册并打开之
  - info COMMAND: 获取命令的帮助文档
  - 很多应用程序会自带帮助文件 /usr/share/doc/APP-VERSION
    - README: 程序相关的信息
    - INSTALL： 安装帮助
    - CHANGES： 版本迭代时的改动信息
  - 主流发行版官方文档
    - http://www.redhat.com/doc
  - 程序官方的文档
    - 官方站点的"document"
  - 搜索引擎 google
    - keyword filetype:pdf
    - keyword site:domain.tld
    - Google Hacker
  - 发行版文档
  - Linux Kernel：Documentation


### 命令程序
命令本身是一个可以执行的程序文件：二进制格式的文件，有可能会调用共享库文件
多数据程序文件都放在：/bin, /sbin, /usr/bin, /usr/sbin/, /usr/local/bin, /usr/local/sbin
普通命令：/bin,/usr/bin,/usr/local/bin
管理命令：/sbin, /usr/sbin, /usr/local/sbin

注意：并非所有的命令都有一个在某目录与之对应的可执行程序文件

### 共享库
/lib, /lib64, /usr/lib, /usr/lib64, /usr/local/lib, /usr/local/lib64
32bits的库: /lib, /usr/lib, /usr/local/lib
64bits的库: /lib64, /usr/lib64, /usr/local/lib64

命令必须遵行特定格式规范：exe， msi， ELF（Linux） 

### 分类
- 由shell程序自带的命令： 内置命令（builtin）
- 独立可执行的文件： 文件名即命令名，外部命令

### shell
shell是一个独特的程序，负责解析用户提供的命令
命令查询目录
环境变量：PATH
```sh
echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
# 自左向右查询
```
查看命令的类型
```sh
$ type COMMAND
$ type type
type is a shell builtin
```

## 基础命令
命令类型
- 外部命令： 显示为命令文件路径
  - 注意：命令可以有别名，别名可以与原名相同，此时原名被隐藏，此时如果运行原命令，则使用\COMMAND
- shell内嵌命令： builtin

### 命令别名
```sh
$ alias # 获取所有可用别名的定义
$ alias NAME="COMMAND" #定义别名， 仅对当前shell进程有效
$ unalias NAME #撤销别名
```


## 书籍出版社
- 尽量看外文书籍
- o'Reiley
- Wrox
- 机械工业，电子工业，人邮，清华大学，水利水电
- 《奇点临近》
- www.ibm.com/developerworks

## 特殊权限
特殊权限
- SUID
- SGID
- STICKY

安全上下文
1. 进程以某用户的身份运行，进程是发起此进程的用户的代理 ，因此以此用户的身份和权限完成所有操作
2. 权限匹配模式
   1. 进程的属主，是否为被访问的属主，如果是，则应用属主的权限，否则进入第2步
   2. 判断进程的属主，是否属于被访问的文件属组，如果是，则应用属组权限;否则进入第3步
   3. 应用other权限

### SUID
默认情况下：用户发起的进程，进程的属主是其发起者;因此，以发起者的身份运行
SUID的功用：用户运行某程序时，如果此程序拥有SUID权限，那么程序运行为进程时，进程的属主不是发起者，而程序文件自己的属主

管理文件的SUID权限
chmod u+|-s file...

展示位置：属主的执行权限位
- 如查属主原本有执行权限，显示为小写s
- 否则显示为大写S

### SGID
功用： 当前属组有写权限，且有SGID权限时，那么所有属于此目录的属组，且以属组身份在此目录中新建文件或目录时，而是此目录的属组

管理文件的SGID权限

chmod g+|-s file...

展示位置：属组的执行权限位
- 如查属组原本有执行权限，显示为小写s
- 否则显示为大写


### STICKY

功用：对于属组可写或全局可写的目录，组内的所有用户或系统上的所有用户对此目录中都能创建新文件或删除所有的已有文件如果为此类目录设置stick权限，则每个用户能创建新文件，且只能删除自己的文件。

管理文件的STICK权限
chmod o+|-t file...

展示位置：其他用户的执行权限位
- 如查属组原本有执行权限，显示为小写t
- 否则显示为大写T

> 系统上的/tmp和/var/tmp目录默认均为sticky权限

管理特殊权限的另一方式
suid sgid sticky 八进制权限

基于八进制方式赋树时，可于默认的三位八进制数字左侧再加一位八进制数字
例如：chmod 1777

## facl

file access control lists
文件的额外的赋权机制
- 在原有的U、G、O之外，另一层让普通用户能控制赋权给另外用户或组的赋权机制

### getfacl 命令

getfacl FILE...
- user:USERNAME:MODE
- group:GROUPNAME:MODE
  
  ### setfacl
  - 赋权给用户 setfacl -m u:USERNAME:MODE FILE..
  - 赋权给组：setfacl -m g:GROUPNAME:MODE FILE
  - 撤销赋权： setfacl -x u:USERNAME FILE... setfacl -x g:GROUPNAME FILE...