# 任务计划及周期性任务执行

未来的某个时间点执行一次某任务: at, batch
周期性运行某任务:crontab
执行结果:会通过邮件发送用户

查看本地邮件服务是否开启,查看25号端口是否有监听
- netstat -tnlp
- ss -tnl 

本地电子邮件服务
- smtp : simple mail transmission protocol
- pop3: post office procotol
- imap4: internet mail access protocol


## mail命令
- mailx send and recerive internet mail
  - mailx [-s "subject"] username[@hostname]
  - 邮件正文生成
    - 交互式输入 单独成行可以表示正文结束,ctrl+d提交亦可
    - 通过输入重定向
    - 通过管道
- MUA: Mail User Agent 用户收发邮件的工具程序

## at命令

at [option] TIME

TIME:
- HH:MM [YYY-MM-DD]
- noon, midnight, teatime
- tomorrow
- now+# UNIT:minutes, hours, days,weeks

at作业有队列的概念,默认使用a队列

选项
- -l 查看执行队列 相当atq
- -f /path/from/file 从指定文件读取作业任务,而不用交互式输入
- -d # 删除指定作业 相当于atrm
- -c 查看指定作业的具体内容
- -q QUEUE 指明队列

注意: 作业执行结果是以邮件发送给提交作业的用户

## batch
batch会让系统自行选择在系统资源较空闲的显示效果去执行指定的任务

## crontab 周期性任务计划

服务程序:cronle 主程序包, 提供了crond守护进程及相关辅助工具

确保crond守护进程(daemon)处于运行状态
- centos7: systemctl status crond.service
- centos6: service crond status

向crond提交作业的方式不同于at,它需要使用专用的配置文件,此文件有固定格式,不建议使用文本编辑器直接编辑此文件,要使用crontab命令

cron任务分两类
- 系统cron任务: 主要用于实现系统自身的维护
  - 手动编辑:/etc/crontab文件
- 用户cron任务: 
  - 命令crontab命令

### 系统cron的配置格式

```sh

SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```
注意:每一行定义一个周期性任务
- *  *  *  *  * 定义周期性时间
- user-name 运行任务的用户身份
- command 任务

此处的环境变量不同于用户登陆后获得的环境变量,因此建议命令使用绝对路径,或者自定义PATH环境变量

执行结果邮件发送给MAILTO指定的用户


用户cron的配置格式:/var/spool/cron/USERNAME
```sh

SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  *  command to be executed
```

注意
- 每行定义的一个cron任务 共6个字段
- 此处的环境变量不同于用户登陆后获得的环境变量,因此建议命令使用绝对路径,或者自定义PATH环境变量
- 邮件发送给当前用户

时间表示法
- 特定值 给定时间点有效取值范围内的值
- * 给定显示时间点有效取值范围内的所有值: 表"每..."
- 离散取值: 在时间点上使用逗号分隔的多个值:#,#,#
- 连续取值: 在时间点上使用-连接开头和结束: #-#
- 在指定时间点上,定义步长: */# #即步长 例 */2
  - 指定的时间点不能被整除时,其意义将不复存在
  - 最小时间单位为分钟,想完成秒级任务,得需要额外借助于其他机制,定义成每分钟任务,而在利用脚本实现在每分钟之前,循环执行多次

例:
- 3 * * * * 每小时执行一次,每小时的3分钟
- 3 4 * * 5 每周执行一次,每周五四点三分
- 5 6 7 * * 每月执行一次 每月7号的6点5分
- 7 8 9 10 * 每年执行一次 每年10月9号8点7分
- 9 8 * * 3,7 每周三和周日
- 0 8,20 * * 3,7 
- 0 9-18 * * 1-5 工作时间内的每小时
- */5 * * * *  每5分钟

## crontab 命令

 crontab [-u user] [-l | -r | -e] [-i] [-s]

 常用选项
 - -e 编辑任务
 - -l 列出所有任务
 - -r 移除所有任务,即删除crontab文件 /var/spool/cron/USERNAME文件
 - -i 在使用-r选项移除所有任务时提示用户确认
 - -u user: root用户可为指定用户管理cron任务

注意: 运行结果以邮件通知给当前用户;如果拒绝接收邮件,
- COMMAND > /dev/null
- COMMAND &> /dev/null

注意: 定义COMMAND时,如果命令需要用到%, 需要对其转义,但放置于单引号中的%不用转义

如果期望某时间因故未能按时执行,下次开机后无论是否到了相应时间点都要执行一次,可使用anacron实现



