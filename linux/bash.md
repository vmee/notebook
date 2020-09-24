# bash

## bash的基础特性

命令历史：shell进程会在其会话中保存此前用户提交执行的过的命令
```sh
$ history
```
定制history功能，可以通过环境变量实现
- HISTSIZE: shell进程可保留的命令的历史的条数
- HISTFILE：持久保存命令历史的文件 ~/.bash_history
- HISTFILESIZE：命令历史文件的大小

### History

> history [-c] [-d offset] [n] or history -anrw [filename] or history -ps arg [arg...]

- -c:清空历史记录
- -d offset: 删除指定命令历史
- -r: 从文件读取命令历史到历史列表中
- -w: 把历史列表中的命令追加至历史文件中
- history #: 显示最近的#条命令

调用命令历史列表中的命令
- !#： 再一次执行历史列表中的第#条命令
- !!: 再一次执行上一条命令
- !STRING： 再一次执行命令历史列表中最近一个以STRING开头的命令
  - 注意：命令的重复执行有时需要依赖于幂等性
  
调用上一条命令的最后一个参数：
- 快捷键：ECS+.
- 字符串：!$

控制命令历史记录的方式
环境变量：HISTCONTROL
- ignoredups: 忽略重复的命令
- ignorespace: 忽略以空白字符开头的命令
- ignoreboth: 以上两者同时生效

修改变量的值 
```sh
$ NAME="VALUE"
```

## 命令补全 
tab键
- 命令补全
- 路径补全

## 命令行展开
- ～ 自动展开为用户的家目录，或指定用户的家目录
- {} 可承载一个以逗号分隔的路径列表，并能够将其展开为多个路径
  - /tmp/{a,b} = /tmp/a, tmp/b

## 命令执行的状态结果
命令执行的状态结果
bash通过状态返回值来输出此结果
- 失败：0
- 成功：1-255
命令执行完成之后，其状态返回值保存于bash的特殊变理$?值里

命令正常执行时，有的还回有命令的返回值：根据命令及其功能的不同，结果各不相同

## 引用命令的执行结果
引用命令的执行结果：$(COMMAND) 或 `COMMAND`

## 引用 
- 强引用 ''
- 弱引用 ""
- 命令引用 ``

## 快捷键
- CTRL+a 跳转命令行首
- CTRL+e 跳转至命令行尾
- CTRL+u 删除行首至光标所在处之间的所有字符
- CTRL+k 删除行尾至光标所在处之间的所有字符
- CTRL+l 清屏 相当于clear


## 变量
变量是命名的内存空间
### 变量命名
变量名只能包含数字字母下划线，不能以字母开头
变量名：见名知义，命名机制遵循某种法则

### 变量类型
- 整型
- 浮点型 
- 字符型
- 布尔型
- 日期时间型

- 字符型 
- 数值型
  - 精确数值
  - 近似数值


变量的类型确定了存储格式，数据范围，参与运算。

- int 8： 1000， 0000 1000
- int 16： 10000， 0001 0000 
- string 16: 16bit

弱类型变量
> bash把所有变量统统视作字符型
> bash中的变量无需事先声明


变量替换：把变量名出现的位置替换为其所指向的内存空间中的数据
变量引用：${var_name} $var_name

### 变量作用域
- 本地变量：作用域仅为当前shell进程
- 环境变量 作用域仅当前shell进程及其子进程
- 局部变量 作用域仅某段代码片段或函数上下文

位置参数变量
特殊变量：shell内置的有特殊功用的变量

### 本地变量
- 变量赋值 name=value
- 变量引用 ${name}, $name
  - "": 变量名会替换为其值 
  - '': 变量名不会替换为其值
- 查看变量 set
- 撤销变量： unset var_name 注意此处非变量引用

### 环境变量
变量赋值 
- export  name=value
- name = value; export name
- declare -x name=value
- name=value; declare -x name
变量引用 ${name} $name

> 注意 bash内嵌了许多环境变量（通常为全大与字符），用于定义bash的工作环境

查看环境变量命令：set、printenv、declare -x、env
撤销环境变量：unset name

### 只读变量
- declare -r name
- readonly name

> 只读变量无法重新赋值 ，并且不支持撤销; 存活时间为当前shell进程的生命周期，随shell进程终止而终止



### 程序：指令+数据
- 指令 运行的程序
- 数据 IO设备、文件、管道、变量


### 程序：算法+数据结构

### set命令
查看定义过的变量

## globbing 文件名通配
匹配模式之元字符
- * 匹配任意长度的任意字符
- ？匹配任意单个字符
- [] 匹配指定范围内容的任意单个字符 [a-z] [A-Z] [0-9] [a-z0-9] [abdyx]
  - [[:upper:]] 匹配大写字母
  - [[:lower:]] 匹配所有小写字母
  - [[:alpha:]] 所有字母
  - [[:digit:]] 所有数字
  - [[:alnum:]] 所有字母和数字
  - [[:space:]] 所有空白字符
  - [[:punct:]] 所有标点符号
- [^] 匹配指定范围外的任意字符 [^a-z]

## IO重定向及管道
程序：指令+数据
程序： IO

可用于输入的设备：文件 
键盘设备，文件系统的常规文件，网卡等
可用于输出的设备 ：文件
显示器，文件系统上的常规文件，网卡等

程序的数据流有三种
- 输入的数据流： <-- 标准输入（stdin），键盘
- 输出的数据流： --> 标准输出 （stdout） 显示器
- 错误输出流： --> 错误输出(stderr) 显示器
  
fd: file descriptor, 文件描述符
- 标准输入： 0
- 标准输出： 1
- 错误输出： 2

IO重定向

输出重定向: > --覆盖输出 
输出重定向: >> -- 追加输出

> set -C 禁止覆盖输出到已存在的文件 可以使用强制覆盖：>|
> set +c 关闭上述特性
> 仅对当前shell有效

错误输出流重定向：2> 2>>

合并正确输出流和错误输出流：
- &>, &>>
- COMMOND > /path/to/file 2>&1
- COMMOND >> /path/to/file 2>&1

输入重定向： <
HERE DOCUMENT: <<
```sh

cat <<EOF
> how are you?
> doing
> EOF

```

### tr命令
tr [OPTION]... SET1 [SET2]
把输入的数据当中的字符，凡是在SET1定义范围内出现的，通通对位转换为SET2出现的字符

用法
- tr SET1 SET2 < path/file
- tr -d SET1 < path/file

> 注意不修改源文件

### 管道
连接程序，实现将前一个命令的输出直接定向后一个程序当作输入
> COMMAD | COMMAD | COMMAD 


### tee命令
read from standard input and write to standard output and files
从标准输入里读取数据，写到标准输出或文件
> COMMAND | tee /PATH/TO/file


### 特殊设备 /dev/null
数据黑洞

## 命令hash

Determine and remember the full pathname of each command NAME.  If
    no arguments are given, information about remembered commands is displayed.

确定和记住每命令的完整的路径，

缓存此前的命令的查找结果：key-value
- key 搜索键
- value: 值

hash命令
- hash列出 
- hash -D COMMAND 删除
- hash -r 清空


## bash多命令执行
```sh
COMMAND1;COMMAND2;COMMAND3;...
```

## 逻辑运算
运算数：
- 真（true, yes, on, 1）
- 假（false,no ,off, 0）

与：&&
或：||
非：！
异域：

短路法则
```sh
$ command1 && command2 //1为假2则不会执行 1为真2必须执行
```
```sh
$ command1 || command2 //1为假2则执行 1为真2则不会执行
```

## 配置文件
配置文件分两类
- profile类 为交互式登录shell提供配置
- bashrc类 为非交互式登录的shell进程提供配置

交互式登录shell进程

直接通过某终端输入账号和密码后登陆打开的shell进程
使用su命令 su - USERNAME 或者使用 su -l USERNAME执行的登录切换

非交互式登录shell进程

su USERNAME 执行的登录切换
图形界面下打开的终端
运行脚本 

### profile类
- 全局：对所有用户都生效
  - /etc/profile
  - /etc/profile.d/*.sh
- 用户个人：仅对当前用户有效
  - ～/bash_profile

功能
- 用于定义环境变量
- 运行命令或脚本

### baserc类
- 全局
  - /etc/bashrc
- 用户个人
  - ～/.bashrc

功用
- 定义本地变量
- 定义命令别名

> 仅管理员可修改全局配置文件

交互式登录shell进程
/etc/profile -> etc/profile.d/* -> bash_profile -> ~/.bashrc -> /etc/bashrc

非交互式登录的shell进程
～/.bashrc -> /etc/bashrc -> /etc/profile.d/*

命令行中定义的特性，例如变量和别名作用域为当前shell进程的生命周期;
配置文件定义的我，只能随后新启动的shell进程有效

让通过配置文件定义的文件立即生效
- 通过命令行重复定义一次
- 让shell进程重读配置文件
  - source /PATH/CONF
  - . /PATH/CONF
