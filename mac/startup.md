# 开机启动程序管理 

## 界面操作管理开机启动程序

方法步骤
1. 进入系统信号设置
2. 进入用户与群组
3. 选择管理员
4. 选择管理员后,会在右边的界面看到"登录项",点击进入
5. 进入登录项后,这里会看到开机启动的程序列表, 通过+号添加 -号删除开机启动的程序

> 不能删除的程序，请解锁后再进行删除


通过界面我们可以去管理开机启动程序,但正常这里只有少数的几项目,而且经常遇到我们安装的程序,随着开机启动了,而这是却没有,这是怎么回事.

## Mac的是如何启动的

我们打开"活动监视器"(Activity Monitor), 然后选择"视图"->"所有进程",然后按pid排序,我们可以看到两个主要的进程:
- kernel_task pid:0
- launchd pid:1

launchd是系统启动是最主要的父进程,也是系统关闭时最后一个退出的进程


当launchd启动后，它会扫描/System/Library/LaunchDaemons和/Library/LaunchDaemons中的plist文件并加载它们；

当你输入密码登入系统后，launchd会扫描/System/Library/LaunchAgents、/Library/LaunchAgents、~/Library/LaunchAgents这三个目录中的plist文件并加载它们。

Daemond和Agent都是launchd管理的后台程序,它们的区别是agent属于当前登录用户的,它们是以登录用户的权限启动的,而Daemond是以登陆Root用户身份启动的, 当然因为它有root权限,也可以指定以某个用户的身份登录.

每一个plist文件，都叫一个“Job”(即一个任务)，当然加载了不代表会运行plist文件里所描述的服务，只有plist里设置了RunAtLoad为true或keepAlive为true时，才会在加载这些plist文件的同时启动plist所描述的服务。


## Launchd


是一套统一的开源服务管理框架，它用于启动、停止以及管理后台程序、应用程序、进程和脚本。

launchd管理的启动类型主要有两个类
- LaunchDaemons 后台任务
- LaunchAgents 启动代理

### LaunchDaemons

LaunchDaemons正常情况下都是以root身份来启动,不管用户是否登陆.

后台任务启动项目的配置文件在:

系统原生的启动进程目录
> /System/Library/LaunchDaemons

安装的第三方启动进程的目录
> /Library/LaunchDaemons

### LaunchAgents
LaunchAgents启动在是用户登陆的时候, 和LaunchDaemons不同,它这些启动的程序有权限访问用户界面并显示信息


启动代理的配置文件在
  
所有用户目录
> /Library/LaunchAgents

指定用户的的目录
> ~/Library/LaunchAgents

系统级
> /System/Library/LaunchAgents


## launchctl的使用

launchctl是launchd的管理工具,它用于管理plist文件对应的服务的启动,停止,重启等

```sh
#列出已加载的plist
$ launchctl list 
#加载配置
$ launchctl load ~/Library/LaunchAgents/xxx.app.plist 

#禁用配置
$ launchctl unload ~/Library/LaunchAgents/xxx.app.plist 

# 禁用服务
$ launchctl disable /Library/LaunchDaemons/com.simonkuang.macos.coredns.plist

# 启用服务
$ launchctl disable /Library/LaunchDaemons/com.simonkuang.macos.coredns.plist

# 杀死进程（不优雅地杀，直接杀进程）并重启服务。对一些停止响应的服务有效。
$ launchctl kickstart -k /Library/LaunchDaemons/com.simonkuang.macos.coredns.plist

# 在不修改 Disabled 配置的前提下启动服务
$ launchctl start /Library/LaunchDaemons/com.simonkuang.macos.coredns.plist

# 在不修改 Disabled 配置的前提下停止服务
$ launchctl stop /Library/LaunchDaemons/com.simonkuang.macos.coredns.plist
```
