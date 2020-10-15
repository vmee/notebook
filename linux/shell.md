# shell

终端：附着在终端的接口程序
- GUI： KDE,GHome,xfce
- CLI: /etc/shells
  - bash
  - zsh
  - fish

bash的特性
- 命令行展开：～，{}
- 命令别名：alias unalias
- 命令历史：history
- 文件名通配： glob
- 快捷键ctrl+a, e, u, k, l
- 命令补全：$PATH，各shell内部命令
- 路径补全：

bash特性之一： 命令hash
缓存之前执行的命令结果
执行命令先查找hash的命令结果，如果找不到再去hash下查找
- key 搜索键
- value 值

hash命令
- hash 列出
- hash -d COMMAND 删除
- hash -r 清空
  
bash特性之一：变量


## 编程语言的分类
根据运行方式
编译运行：源代码->编译器-->程序文件
解释运行：源代码->运行时启动解释器，由解释器边解释边运行

根据编程过程中功能的实现是调用库还是调用外部的程序文件
shell脚本编译：利用系统上的命令及编程组件进行编程
完整编程：利用库或编程缓件进行编程

编程模型：过程式编程语言，面向对象的编程语言
程序=指令+数据
过程式：以指令为中心来组织代码，数据服务于代码
- 顺序执行
- 选择执行
- 循环执行
代表：C,bash
对象式：以数据为中心来组织代码，围绕数据来组织指令
- 类class 实例化对象
- method 方法
代表：java,C++,python

shell脚本编程：过程式编程，解释运行，依赖于外部程序文件运行

## 如何写shell脚本
脚本文件的第一行，顶格，给出shebang,解释路径，用于指明解释执行当前脚本的解释器程序文件
常见的解释器
- #!/bin/bash
- #!/usr/bin/python
- #!/usr/bin/perl

文本编程器:nano
行编辑器：sed
全屏幕编程器：nano vi vim

shell脚本是什么？
命令的堆积，但很多命令不具有幂等性，需要用程序逻辑来判断运行是是否满足，以避免发生逻辑错误

脚本文件格式
- 第一行 顶格:#!/bin/bash
- 注释信息： #
- 代码注释
- 缩进，适度添加空白行

```sh
#!/bin/bash
# descripton
# version: 0.0.1
# author: Tom<tom@s.cn> 
# date:
```

## 运行脚本
- 赋予执行权限，并直接运行此程序文件
  - chmod +x file
  - /path/to/file
- 直接运行解释器，将脚本以命令行参数传递给解释器程序
  - bash /path/script_file

> 脚本中的空白行会被解释器忽略
> 脚本除了shebang,余下的所有#开头的行，都会被视作注释而被忽略，即注释行


## 编程思想
- 问题空间 --> 解空间

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


## 算术运算

+，-，*，/，%，**

- let VAR=expression
- var=$[expression]
- var=$((expression))
- var=$(expre argu1 argu2 argu3)

> 注意：有些时候乘法符号需要转义

### 增强型赋值
变量做某做算术运算后回存此变量中
- let i=$i+#
- let i+=#
- += -= *= /= %=
- 自增 let VAR++
- 自减 let VAR--

## 条件测试

判断需求是否满足，需要由测试机制来实现

如何编写测试表达式以实现所需要的测试
- 执行命令，并利用命令状态返回值来判断
  - 0 成功
  - 1-255 失败
- 测试表达式
  - test EXPRESSION
  - [ EXPRESSION ]
  - [ EXPRESSION ] 注意：EXPRESSION两端必须有空白字符，否则会有语法错误


bash的测试类型
- 数值测试
- 字符串测试
- 文件测试

数值测试：数值比较
- -eq 是否等于
- -ne 是否不等于
- -gt 是否大于
- -ge 是否大于等于
- -lt 是否小于
- -le 是否小于等于

字符串测试
- == 是否等于
- > 是否大于
- < 是否小虎于
- != 是否不等于
- =~ 左侧的字符串是否能够被右侧的PATTERN匹配 
- -z "string" 判断指定的字符串是否为空，空为真 不空则假
- -n "string" 判断指定的字符串是否不空， 不空则真，空则假
  
> 字符串比较尽量使用双中括号[[]]
> 字符串要加引用 

文件测试
- 存在性测试 存在则为真，否则则为假
  - -a FILE
  - -e FILE
- 存在性及类型测试
  - -b FILE： 是否存在且为块设备文件
  - -c FILE 是否存在且为字符设备文件
  - -d FILE 是否存在并且为目录文件
  - -f FILE 是否存在并且为普通文件
  - -l FILE 或 -h file 是否存在并且为符号链接文件
  - -p FILE 管道文件
  - -S FILE 是否存在且为套接字文件
- 文件权限测试
  - -r FILE 是否存在并且 对当前用户可读
  - -W FILE 是否存在并且对当前用户可写
  - -x FILE 是否存在并且对当前用户可执行
- 特殊权限测试
  - -u FILE 是否存在并且拥有suid权限
  - -g FILE 是否存在并且拥有sgid权限
  - -k FILE 是否存在并且拥有sticky权限
- 文件内容是否有内容
  - -s FILE 是否有内容
- 时间戳
  - -N FILE 文件自己从上次操作后是==否被修改过
- 从属关系测试
  - -O FILE 当用户量是不用户的属主
  - -G FILE 当用户量是不用户的属组
- 双目测试 
  - FILE1 -ef FILE2 file1与file2是否指向同一个文件系统的相同inode的硬链接
  - FILE1 -nt FILE2 file1是否新于file2
  - FILE1 -ot FILE2 file1是否旧于file2
- 组合测试条件
  - 逻辑运算
    - 第一种方式
      - &&
      - ||
      - ！
    - 第二种方式
      - EXPRESSION -a EXPRESSION
      - EXPRESSION -o EXPRESSION2
      - !EXPRESSION



脚本的状态返回值

默认是脚本中执行的最后一个条件命令的状态返回值 

自定义状态退出状态码
- exit [n]: n为自己指定的状态码

> shell进程遇到exit时，即会终止，因此整个脚本就会退出进程



## 向脚本传递参数

位置参数变量

> myscript.sh argu1 argu2 
引用方式：
- $1，$2...

轮替
- shift [n]: 位置参数轮替

## 特殊变量
- $0 脚本路径变量本身
- $# 脚本参数的个数
- $* 所有参数
- $@ 所有参数


## 过程式编程语言的代码执行解释
- 顺序执行: 逐条运行
- 选择执行
  - 代码有一个分支:条件满足时才会执行
  - 两个或以上的分支:只会执行其中一个满足条件的分支
- 循环执行
  - 代码片断(循环体)要执行0, 1或多个来回
  
### 选择执行
- if then fi
- if then else fi
