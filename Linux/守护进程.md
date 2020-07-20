# 守护进程

在终端执行的命令，或启动的程序，在退出或关闭终端时就会退出
怎么让一个执行的程序，变成系统的守护进程(daemon)，成为一种服务（service）,一直在哪里运行？


## 前台任务与后台任务
前台任务(foreground job)：独占命令行窗口，运行完了或手动中止后，才能执行其他命令
后台任务(backgroud job)： 
只要在命令的尾部加上符号&，启动的进程就会成为"后台任务"。如果要让正在运行的"前台任务"变为"后台任务"，可以先按ctrl + z，然后执行bg命令（让最近一个暂停的"后台任务"继续执行）。

"后台任务"与"前台任务"的本质区别只有一个：是否继承标准输入


## SIGHUP信号

变成守护进程，用户退出，进程如何？

用户准备退出 session
系统向该 session 发出SIGHUP信号
session 将SIGHUP信号发给所有子进程
子进程收到SIGHUP信号后，自动退出


"后台任务"是否也会收到SIGHUP信号？ 这由 Shell 的huponexit参数决定的 
```bash
$ shopt | grep huponexit
```
## 启动守护进程
- &
- ctrl+z  命令bg
- disown
- nohup
- screen
- tmux
- Node应用工具 forever nodemon pm2
- Systemd


## 参考
[守护进程](http://www.ruanyifeng.com/blog/2016/02/linux-daemon.html)
