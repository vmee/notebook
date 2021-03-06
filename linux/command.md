# 常用命令
## 命令格式
> COMMAND OPTIONS ARGUMENTS

发起一命令： 请求内核将某个二进程程序运行为一个进程
程序 --> 进程
静态 --> 动态（有生命周期）

命令本身是一个可以执行的程序文件：二进制格式的文件，有可能会调用共享库文件
多数据程序文件都放在：/bin, /sbin, /usr/bin, /usr/sbin/, /usr/local/bin, /usr/local/sbin
普通命令：/bin,/usr/bin,/usr/local/bin
管理命令：/sbin, /usr/sbin, /usr/local/sbin




## CLI
命令行接口
[root@knode ~]# COMMAND
命令行提示符prompt
- root: 当前登录用户
- knode: 当前主机名，非完整格式
- ～： 用户当前所在的目录（current directory）, 也称为工作目录（working directory）:相对路径
- #： 命令提示符
  - #： 管理员账号， 为root,拥有最高权限，能执行所有操作
  - $：普通用户，非root用户，不具有管理权限，不能行系统管理类操作
注意： 建议使用非管理员账号登录，执行管理操作临时切换至管理员，操作完成即退回 


## 远程连接：
ssh协议：secure shell


## ss
查看系统监听的端口
```sh
$ ss -tln //查看系统是否监听22号端口
```
## echo 回显命令

echo OPTIONS STRING
```sh
$ echo "hello"
hello 
```

String 可以使用引号，单引号与双引号
- 单引号：强引用，变量引用不执行替换
- 双引号：弱引用，变量引用会被替换
- 变量引用的正规符号 ${name}

## ip
查看ip地址
```sh
$ ip addr list/show
```
ip为inet字段

## ifconfig
查看ip地址
```sh
$ ifconfig 
```
ip为inet字段

## ping 
测试网络连通性
```sh
$ ping ip
```
ctrl+c 终止命令

## iptables

```sh
$ iptables -L -n #显示所有端口
$ iptables -F ##清除端口
```

## systemctl 
```sh
systemctl 
```

## service



## exec
exec是用新的进程去代替原先的进程，原先的进程就消失了


## 确保防火墙处于关闭状态
```sh
$ iptable -L -n
$ systemctl disable firewalld.service #禁用自动启动 centos 7
$ systemctl stop firewalld.service #停用iptables centos 7
$ service iptables stop #centos 6 停止iptables
$ service iptables off # centos 7 停止iptables
```

## 查看所有shell
```sh
$  echo $SHELL
```

## 显示所在终端 
```sh
$ tty
```

## 启动GUI
启动GUI（图形界面）：在某一虚拟终端接口运行命令
```sh
$ startx &
```

## 关机与重启命令
```sh
$ systemctl poweroff #centos 7
$ poweroff
$ halt
$ reboot #重启
$ shutdown -h #关机
$ shutdown -r #重启
$ shutdown -c #取消
$ shutdown -h now #指定时间关机 now/hh:mm/+m
$ shutdown -r +10 "hello erveryone" #指定时间发送消息
```

## 向所有终端发送信息/广播消息wall
```sh
$ wall "message"
Broadcast message from root@master134 (pts/0) (Sat Sep  5 10:03:59 2020):
message
```

## 获取文件名basename

```sh
$ basename /path/to/basename
```

## 获取路径dirname
```sh
$ dirname /path/to/basename
```

## 查看文件格式
```sh
$ file /bin/ls
/bin/ls: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=aaf05615b6c91d3cbb076af81aeff531c5d7dfd9, stripped
```

## 查看命令类型
```sh
$ type COMMAND
$ type type
type is a shell builtin
```

## 查看命令手册 man
- 内部命令 
  - $ help COMMAND
- 外部命令 
  - 命令自带简要格式的使用帮助 $ COMMAND --help
  - 使用手册： manual
    - 目录/usr/share/man
    - $ man COMMAND

## 日期相关的命令
### date
- 显示时间 date [OPTION]... [+FORMAT] 
- 设置时间 date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]

### hwclock/clock
硬件时钟，设置或显示硬件时钟
-w, --systohc
-s, --hctosys
### cal
显示日历
```sh
$ cal [[month] year]
$ cal 2020 #显示一年日历
```

## which
显示一个命令的全路径
--skip-alias 忽略别名
```sh
$ which which
```

## whereis 
定位一个命令的二进制，源代码和手册 文件

## who
显示已经登录用户
-b 显示此次启动的时间
-r 显示运行级别
## w
显示登录的用户并显示他们下在做什么
增强版的who命令


## 文本查看类命令
### 分屏查看命令 more
翻屏至文件尾部自动退出
```sh
$ more file 
```

### 分屏查看命令 less
```sh
$ less file
```

### head 
查看文件的前n行

```
$ head [options] file
```

### tail 
查看文件的后n行
```sh
$ tail [options] file
```

- -n #
- -#
- -f 查看文件尾部内容不退出，跟随显示新增的行

### stat
显示文件和文件系统的状态 也叫数据的元数据

```sh
$ stat file
```
文件:两类数据
- 元数据：metadata
- 数据: data

时间戳：
- 最近访问时间 access time 
- 最近更改时间 modify time 数据改变 数据改变元数据一定会变
- 最近改动时间 change time 元数据改变时间 如更改名字或权限

### 修改元数据命令 touch
- -c 指定文件路径不存在，不予创建
- -a 仅修改access time
- -m 仅修改modify time
- -t stamp yymmddhhii[ss] 指定修改时间

## 文件管理工具 cp, mv, rm
### cp
copy
复制文件只是复制文件的数据
```sh
$ cp 源文件 目录文件
```
单源复制
>  cp [-R [-H | -L | -P]] [-fi | -n] [-apvX] source_file target_file
- 如果target文件不存在，则创建复制数据
- 如果target文件存在，则覆盖
- 如果target是目录 ，则在target目录下创建一个同名文件，并复制数据

多源复制
>  cp [-R [-H | -L | -P]] [-fi | -n] [-apvX] source_file ... target_directory

- 如果target不存在，则错误
- 如果target是文件，则错误
- 如果target是目录, 分别复制每个文件到目录

常用选项
- -i ,交互式复制，即覆盖之前提醒用户
- -f，强制覆盖目录文件
- -r 递归复制目录
- -d 复制符号链接本身，而非其指向的源文件
- -a -dR --preserv=all archive 用于实现归档
- --preserv= mode:李建 ownership:属主和属组 timestamp:时间戳 context:安全标签 xattr:扩展属性 links：符号链接 all：上述所有属性

### mv
流程： 在target上创建文件 > 复制数据 > 删除源文件

options
- -i 交互式
- -f force

### rm
remove 移除 删除文件

options 
- -i 交互式
- -f 强制删除
- -r 递归删除

删除目录：rm -rf /PATH/TO/DIR
危险操作：rm -rf /

> 注意： 所有不用的文件建议不要直接删除，而是移动至某个专用目录：（模拟回收站）



### tr命令
tr [OPTION]... SET1 [SET2]
把输入的数据当中的字符，凡是在SET1定义范围内出现的，通通对位转换为SET2出现的字符

用法
- tr SET1 SET2 < path/file
- tr -d SET1 < path/file

> 注意不修改源文件

### tee命令
read from standard input and write to standard output and files
从标准输入里读取数据，写到标准输出或文件
> COMMAND | tee /PATH/TO/file


### scp命令

### rsync命令


## 文本处理工具
Linux文本处理三剑客
- grep 文本过滤工具（模式：pattern）工具
  - grep 基本正则表达式
  - egreg 扩展正式表达式
  - fgreg 
- sed stream editor 流编辑器，文本编辑工具
- awk Linux上实现的为gawk 文本报告生成器（格式化文本）


## 正则表达式 Regual Expression REGEXP
由一一类特殊字符及文本字符所编写的模式，其中有些字符不表示其字面意义 ，而是用于表示控制或通配的功能
分两类
- 基本正则表达式 BRE
- 扩展正则表达式 ERE

元字符：

## grep 
Grobal search regular expression and print out the line
作用：文本搜索工具，根据用户指定的“模式（过滤条件）” 对目标文本逐行进行匹配检查，打印匹配到的行
模式：由正则表达式的元字符及文本字符所编写出的过滤条件

正则表达式引擎：
- perl pcre

格式：grep [option] pattern [file..]

options
- --color=auth 对匹配到的文本着色后高亮显示 
- -i igonrecase 忽略字符的大小写 
- -o 仅显示匹配的字符串本身
- -v --invert-match 显示不能被匹配的行
- -E --extend-regexp 支持扩展的正则表达式
- -q --quiet 静默，不显示执行结果
- -A # after 显示后面行
- -B # before 显示前面行
- -c # context 显示前后的行


基本正则表达式：

贪婪模式

字符匹配
- . 匹配任意单个字符
- [] 匹配指定范围内的任意单个字符
- [^] 匹配指定范围外的任意单个字符
匹配次数
- * 匹配其前面字符的任意次 0，1多次
- .* 任意长度的任意字符
- \? : 匹配前面字符0次/1次，即其前面的字符是可有可无的
- \+ 匹配其前面的字符1次或多次; 即其前面的字符要出现至少一次
- \{m} 匹配其前面的字符m次
- \{m,n\} 匹配前面的字符至少m次，至少n次
  - \{0,n\}至多n次
  - \{m,\}至少m次


位置锚定
- ^: 行首锚定 用于模式的最左侧
- $：行尾锚定 用于模式的最右侧
- ^PATTERN$ 用于PATTERN来匹配整行
  - ^$ 空白行
  - ^[[:space:]]*$ 空行或包含空白字符的行


单词：非特殊字符组成的连续字符（字符串）都称为单词
- \<或\b 词首锚定 用于单词模式的左侧
- \>或\b 词尾锚定 用于单词模式的右侧
- \<PATTERN\> 匹配完整单词


分组及引用 
- \(\) 将一个或多个字符捆绑在一起，当作一个整体进行处理
  - \(xy\)*ab

> Note: 分组括号中的模式匹配到的内容会被正则表达式引擎自动记录于内部的变量中， 这些变量为：
> \1: 模式从左侧起，第一个左括号以及与之匹配的右括号之间的模式 所匹配到的字符
> \2: 模式从左侧起，第二个左括号以及与之匹配的右括号之间的模式 所匹配到的字符
> eg: grep "\(l..e\).*\1"

## egrep

支持扩展正则表达式 实现类似于grep文本过滤功能  grep -E

格式：egrep [option] pattern [file..]
options
- --color=auth 对匹配到的文本着色后高亮显示 
- -i igonrecase 忽略字符的大小写 
- -o 仅显示匹配的字符串本身
- -v --invert-match 显示不能被匹配的行
- -E --extend-regexp 支持扩展的正则表达式
- -q --quiet 静默，不显示执行结果
- -A # after 显示后面行
- -B # before 显示前面行
- -c # context 显示前后的行

扩展正则表达式的元字符：
字符匹配
- .: 任意单个字符
- []：指定范围内的字符
- [^]: 指定范围外的任意单个字符
次数匹配
- * 任意单个字符
- ？ 0次数或1次 其前面的字符可有可元
- + 其前字符至少1次
- {m} 其前的字符m次
- {m, n} 至少m次 至多n次
位置猫定
- ^: 行首锚定
- $：行尾锚定
- \<或\b 词首锚定
- \>或\b 词尾锚定
分组及引用
- ()分组： 括号内的模式匹配到的字符会被记录于正则表达式引擎的内部变量中
- 后向引用：\1,\2
或
- a|b : a或者b


## fgrep 
不支持正则表达式元字符
当无需要用到元字符去编写模式时，使用fgrep必能更好


## 文本查看及处理工具 wc、cut、sort、uniq、diff、patch

### wc 
打印文件的行数字节数
print newline, word, and byte counts for each file
格式 wc [option] file
- -l lines
- -w words
- -c bytes

### cut
remove sections from each line of files
移除文件行的片段
文本截取工具

格式：cut [option] file

options
- -d 分隔符 以指定的字符为分隔符
- -f fields 挑选出的字符
  - #指定单个字段
  - #-#连续多个字符
  - #,#离散的多个字符
  - 可混合使用

### sort
排序

格式：sort [option] file

options 
- -t CHAR 指定分隔符
- -k # 用户排序比较的字段
- -n 基于数值大小排序 而非字符
- -r 逆序排序
- -f 忽略字符大小写
- -u 重复的行只保留一份 重复行 连续且相同


### uniq

格式 uniq [option] file

options 
- -c 显示每行的重复次数
- -u 仅显示未曾重复过的行
- -d 仅显示重复的行

### diff

比较文件差异 生成补丁

格式 diff [options] file

生成补丁命令：
diff /ptaht/to/oldfile /path/to/newfile > /path/to/patchfile

options
- -u 使用unfiled机制，即显示修改行的上下文，默认3行



### patch
打补丁

- patch [option] -i patchfile oldfile
- patch oldfile < patchfil