# Limit



## OOME (out of memory exception)
--oom-kill-disable               Disable OOM Killer
--oom-score-adj int              Tune host's OOM preferences (-1000 to 1000)


## CPU Limit
--cpu-period int                 Limit CPU CFS (Completely Fair Scheduler) period
--cpu-quota int                  Limit CPU CFS (Completely Fair Scheduler) quota
--cpu-rt-period int              Limit CPU real-time period in microseconds
--cpu-rt-runtime int             Limit CPU real-time runtime in microseconds
-c, --cpu-shares int                 CPU shares (relative weight) 按比例占用， 空闲的不分配
--cpus decimal                   Number of CPUs 限制数量
--cpuset-cpus string             CPUs in which to allow execution (0-3, 0,1) 限制用在哪个cpu
--cpuset-mems string             MEMs in which to allow execution (0-3, 0,1)


## Memory
-m, --memory bytes                   Memory limit
--memory-reservation bytes       Memory soft limit
--memory-swap bytes              Swap limit equal to memory plus swap: '-1' to enable unlimited swap
--memory-swappiness int          Tune container memory swappiness (0 to 100) (default -1)