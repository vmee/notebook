# 用户,组和权限管理
Multi-tasks Multi-Users

## 用户
每个使用者：
- 用户标识
- 密码(认证)
- 组：用户组，用户容器


Authentication: 认证
Authorization： 授权
Audition: 审计


### 用户类型
- 管理
- 普通用户 系统用户、登陆用户
- 用户标识：userID,UID, 16bits二进制数字： 0-65535
  - 管理员：0
  - 普通用户： 1-65535
    - 系统用户：1-499（CentOS6），1-999（CentOS7）
    - 登录用户：500-60000（CentOS6）1000-60000（CentOS7）
- 名称解析：名称转换
  - user <> uid
  - /etc/passwd 名称解析库


## 组
组类别
- 管理员组
- 普通用户组
  - 系统组
  - 登陆组

组标识：GroupID，GID
- 管理员组：0
- 普通用户组： 1-65535
  - 系统用户组：1-499（CentOS6），1-999（CentOS7）
  - 登录用户组：500-60000（CentOS6）1000-60000（CentOS7）

名称解析：GroupName <> gid
解析库 /etc/group

组类别2：
- 用户的基本组（主组）
- 用户的附加组

组类别3
- 私有组 组名同用户名且只有一个用户
- 公共组 组内包含多个用户


## 认证信息
通过比对事先存储的信息，与登陆时提供的信息是否一致

password密码库：/etc/shadow
组密码库：/etc/gshadow


### 密码的使用策略
1. 使用随机密码
2. 最短长度不要低于8位
3. 应该使用大写字母，小写字母、数字和标点符号四类字符中到少三类
4. 定期更换

### 加密算法 
- 对称加密： 加密和解密使用同一个密钥
- 非对称加密： 加密和解密使用一对儿密钥
  - 密钥对儿：
    - 公钥：public key
    - 私钥：private key
- 单向加密：只能加密，不能解密，提取数据特征码
  - 定长输出
  - 雪崩效应
- 算法
  - md5: message digest 128bits
  - sha：secure hash algorithm 160bits
  - sha224
  - sha256
  - sha386
  - sha512

> 单向加密， 在计算时加salt,添加随机数

## 相关文件

/etc/passwd: 用户的信息库
> name:password:UID:GID:GECOS:directory:shell

- name: 用户名
- password: 可以是加密的密码，也可是占位符
- UID
- GID
- GECOS 注释信息
- directory: 用户的家目录
- shell: 用户的默认shell,登陆时默认的shell程序

/etc/shadow:用户密码
> 用户名:加密的密码:最近一次的修改密码的时间:最短使用期限:最长使用期限:警告期限:过期期限:保留字段

/etc/group:组
> group_name:password:GID:user_list
user_list: 该组的用户成员，以此组为附加组的的用户的用户列表

## 相关命令
- useradd
- userdel
- usermod
- passwd
- groupadd 
- groupdel
- gruopmod
- gpasswd
- chage
- chsh
- id
- su 切换用户身份


### 安全上下文
- 进程以其发起者的身份运行
  - 进程对文件的访问权限，取决 于发起此进程的用户的权限

系统用户：为了能够让后台进程或服务类进程以非管理员的身份运行，通常需要为此创建多个普通用户：这类用户从不用登陆系统

### groupadd


group [options] group_name
- -g GID：指定GID,默认上一个组的GID+1
- -r 创建系统组

### groupdel

### groupmod

修改组属性

- -g GID 修改GID
- -n new name 修改组名

### useradd

useradd [options] username
- -u --uid UID 指定UID; 默认上一个uid+1
- -g --gid group 指定基本组id,此组得事先存在
- -G --GROUPS GROUP1 GROUP2 指定附加组
- -c --comment 注释信息
- -d --home 以指定路径作为用户家目录， 通过复制/etc/skel此目录并重命名实现，指定的家目录路径已经存在，则不会为此用户复制配置文件
- -s --shell SHELL 指定用户的默认shell 可用shell列表/etc/shells文件 
- -r --system 创建系统用户
- -m 强制创建主目录
- -M 不创建主目录
- -f 用户可以登陆的天数
- -D 显示默认配置的信息
- -D 选项 修改选项的值

> 注意 创建用户时的诸多默认配置文件为/etc/login.defs
> -D 修改的数据存储于/etc/default/useradd文件中

### usermod

- -u --uid UID 修改UID
- -g --gid group 修改用户基本组
- -G --GROUPS GROUP1 GROUP2 修改附加组，覆盖原来
- -a --append 与-G一起使用，追加附加组
- -c --comment 修改注释
- -d --home 修改用户的家目录
- -m --move-home 只能与-d一起使用，用户于移动家目录 
- -l --login NEW-LOGIN 修改用户名
- -s --shell 修改shell
- -l --lock 锁定用户密码， 即在用户原来的密码字符串之前加个“!”
- -U --unlock 解锁用户密码

### userdel 
- -r 删除时 一并删除家目录 

### passwd

- -d 清除密码
- -l -u 锁定和解锁用户
- -e DATE  用户过期时间
- -i DAYS 非活动期限
- -n DAYS 密码最短使用天数
- -x DAYS 密码最长使用天数
- -w DAYS 警告期限
- --stdin 一次输入修改密码 echo "password" | passwd --stdin login


### gpasswd
给组定义修改密码

> 密码用于限制用户随意切换基本组，切换基本组用newgrp

- -a USERNAME 向组中添加用户
- -d USERNME 从组中移除用户

### newgrp
临时切换用户的基本组
- - 模拟用户重新登陆以实现的重新初始化其工作环境

### chage
更改用户密码过期信息

### id
- -u 仅显示用户的UID
- -g 仅显示用户的基本组
- -G 仅显示用户的所有组
- -n 显示名称而非id
  
### su
switch user 切换用户

登陆式切换：会通过重新读取目标用户的配置文件来重新初始化
- su - username
- su -l username
非登陆切换：不会读取目录用户的配置文件进行初始化
- su username

> 注意：管理员可无密码切换到其他任何用户

不切换用户，仅以指定用户身份运行命令
- -c `COMMAND`

### 其他不常用 
- chsh 更改shell
- finger 用户信息 
- chfn 更改finger
- whoami



## 权限管理

进程安全上下文

进程对文件的访问权限应用模型
- 进程的属主与文件的属主是否相同，如果相同，则应用属主权限
- 否则，则检查进程的属主是否属于文件的属组，如果是，则应用属组权限
- 否则，就只能应用other的权限

权限
- r read
- w write
- x excute

文件
- r 可读取文件的数据
- w 可修改的文件的数据
- x 可将此文件运行为进程 

目录
- r 可使用ls命令获取其下的所有文件列表
- w 可修改此目录下的文件列表，即创建或删除文件
- x 可cd到此目录中，且可使用ls -l 获取所有文件详细属性信息

mode:rwxrwxrwx  
ownership: user,group  

权限组合机制
- --- 000 0
- --x 001 1
- -w- 010 2
- -wx 011 3
- r-- 100 4
- r-x 101 5
- rw- 110 6
- rwx 111 7

权限管理命令
### chmod
三类用户
- u 属主
- g 属组
- o 其他
- a 所有
mode表示法
- 赋权表示法 u=/g= ...
- 授权表示法 u+/u- g+/g- ...
- 8进制表示法 777 755 644 ...

options
- --reference 引用性修改 --reference 引用文件  复用其他文件权限
- -R --recusive 递归修改

### chown
更改属主与属组

- -R 递归修改 
- - --reference 引用性修改 --reference 引用文件 复用其他文件属主与组
  
### chgrp
- -R 递归修改 
- - --reference 引用性修改 --reference 引用文件 复用其他文件属主与组


> 注意 仅管理员可以修改属主与属组


### install
install - copy files and set attributes
- install [OPTION]... [-T] SOURCE DEST
- install [OPTION]... SOURCE... DIRECTORY
- install [OPTION]... -t DIRECTORY SOURCE...
- install [OPTION]... -d DIRECTORY...

常用选项
- -m --mode=mode 设定文件权限，默认755
- -o --owner=OWNER 设定属主
- -g --group=GROUP 设定属组
- -d 创建目录


### mktemp

 mktemp - create a temporary file or directory

 