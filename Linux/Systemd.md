# Systemd


## 解决了什么问题
历史上，Linux启动一直采用init进程

命令如：
```bash
$ sudo /etc/init.d/apache2 start
$ service apache2 start
```

这种方法两个缺点：

一是启动时间长。init进程是串行启动，只有前一个进程启动完，才会启动下一个进程。

二是启动脚本复杂。init进程只是执行启动脚本，不管其他事情。脚本需要自己处理各种情况，这往往使得脚本变得很长。


Systemd 就是为了解决这些问题而诞生的。它的设计目标是，为系统的启动和管理提供一套完整的解决方案。



## 可以做什么

### 管理系统
### 查看启动耗时
### 查看当前主机信息
### 查看本地化设置
### 查看当前时区设置
### 查看当前登陆用户


##参考
[Systemd 入门教程：命令篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)
[Systemd 入门教程：实战篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)