---
title: Redis持久化存储
date: 2018-05-09 21:14:11
updated: 2018-05-09 21:14:11
categories: redis
tags: redis
description: AOF、RDB
copyright: true
comments: true
---
<!--more-->
## Redis中数据存储模式
> cache-only
persistence

```
cache-only：只做为“缓存”服务，不持久数据，数据在服务终止后将消失，
此模式下也将不存在“数据恢复”的手段，是一种安全性低/效率高/容易扩展的方式；
persistence：为内存中的数据持久备份到磁盘文件，在服务重启后可以恢复，
此模式下数据相对安全。
```

对于persistence持久化存储，Redis提供了两种持久化方法
```
Redis DataBase(简称RDB)
Append-only file (简称AOF)
```
> 如果同时开启两种持久化，会优先加载AOF进行恢复


## RDB持久化：默认开启
> 指定时间间隔进行快照存储

优点：使用单独子进程来进行持久化，主进程不会进行任何IO操作，保证了redis的高性能
缺点：RDB是间隔一段时间进行持久化，如果持久化之间redis发生故障，会发生数据丢失。所以这种方式更适合数据要求不严谨的时候
* save：服务器进程进行快照存储（阻塞）
* bgsave：进行异步快照存储（派生子进程处理，非阻塞）
* 异常恢复：redis-check-rdb
```
#dbfilename：持久化数据存储在本地的文件
dbfilename dump.rdb
#dir：持久化数据存储在本地的路径，如果是在/redis/redis-3.0.6/src下
启动的redis-cli，则数据会存储在当前src目录下
dir ./
##snapshot触发的时机，save <seconds> <changes>  
##如下为900秒后，至少有一个变更操作，才会snapshot  
##对于此值的设置，
需要谨慎，评估系统的变更操作密集程度  
##可以通过save “”来关闭snapshot功能  
#持久化(以快照的方式) 策略（默认）
save 900 1       （15分钟变更一次）
save 300 10     （5分钟变更10次）
save 60 10000  （1分钟变更1万次）
##当snapshot时出现错误无法继续时，是否阻塞客户端“变更操作”，
“错误”可能因为磁盘已满/磁盘故障/OS级别异常等  
stop-writes-on-bgsave-error yes  
##是否启用rdb文件压缩，默认为“yes”，压缩往往意味着“额外的cpu消耗”，
同时也意味这较小的文件尺寸以及较短的网络传输时间  
rdbcompression yes  
```

## AOF持久化：默认不开启
> 以日志的形式来记录每个写操作，将redis执行过的所有写指令记录下来（读操作不记录），只许追加但不可以改写文件，redis启动之初会读取改文件进行重新构建数据

* AOF通过保存所有修改数据库的写命令请求来记录服务器的数据库状态
* AOF文件中的所有命令都会以Redis命令请求协议的格式保存 
优点：可以保持更高的数据完整性，如果设置追加file的时间是1s，如果redis发生故障，最多会丢失1s的数据（appendfsync--->everysec）；且如果日志写入不完整支持redis-check-aof来进行日志修复；AOF文件没被rewrite之前（文件过大时会对命令进行合并重写），可以删除其中的某些命令（比如误操作的flushall）。 
缺点：AOF文件比RDB文件大，且恢复速度慢，运行效率也比rdb慢。
### 重写机制
> 当超过阈值，则启动内容压缩，只保留最小指令集，可使用命令：bgrewriteaof

* 定义：AOF采用文件追加的方式持久化数据，所以文件会越来越大，为了避免这种情况发生，增加了重写机制
当AOF文件的大小超过了配置所设置的阙值时，Redis就会启动AOF文件压缩，只保留可以恢复数据的最小指令集，可以使用命令bgrewriteaof
* 原理：当AOF增长过大时，会fork出一条新的进程将文件重写(也是先写临时文件最后rename)，遍历新进程的内存数据，每条记录有一条set语句。
 重写AOF文件并没有操作旧的AOF文件，而是将整个内存中的数据内容用命令的方式重写了一个新的aof文件（有点类似快照）
* 触发机制：Redis会记录上次重写时的AOF文件大小，默认配置时当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发
```
auto-aof-rewrite-percentage 100  （一倍）
auto-aof-rewrite-min-size 64mb
```

### 持久化策略
* appendfsync always (同步持久化，每次发生数据变更会被立即记录到磁盘，性能差但数据完整性比较好)
* appendfsync everysec (异步操作，每秒记录，如果一秒钟内宕机，有数据丢失)
* appendfsync no  （将缓存回写的策略交给系统，linux 默认是30秒将缓冲区的数据回写硬盘的）
```
##此选项为aof功能的开关，默认为“no”，可以通过“yes”来开启aof功能  
##只有在“yes”下，aof重写/文件同步等特性才会生效  
appendonly yes  

##指定aof文件名称  
appendfilename appendonly.aof  

##指定aof操作中文件同步策略，有三个合法值：always(记录立即同步，性能较差) everysec(每秒同步，官方推荐) no(将缓存回写的策略交给系统，linux 默认是30秒将缓冲区的数据回写硬盘的)，默认为everysec  
appendfsync everysec  
##在aof-rewrite期间，appendfsync是否暂缓文件同步，"no"表示“不暂缓”，“yes”表示“暂缓”，默认为“no”  
no-appendfsync-on-rewrite no  

##aof文件rewrite触发的最小文件尺寸(mb,gb),只有大于此aof文件大于此尺寸是才会触发rewrite，默认“64mb”  
auto-aof-rewrite-min-size 64mb  

##相对于“上一次”rewrite，本次rewrite触发时aof文件应该增长的百分比。  
##每一次rewrite之后，redis都会记录下此时“新aof”文件的大小(例如A)，那么当aof文件增长到A*(1 + p)之后  
##触发下一次rewrite，每一次aof记录的添加，都会检测当前aof文件的尺寸。  
auto-aof-rewrite-percentage 100
```

## 比较
* RDB与AOF同时开启  默认先加载AOF的配置文件
* 相同数据集，AOF文件要远大于RDB文件，恢复速度慢于RDB
* AOF运行效率慢于RDB,但是同步策略效率好，不同步效率和RDB相同