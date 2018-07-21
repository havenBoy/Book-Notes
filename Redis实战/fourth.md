# 数据安全与性能保障
> 数据的持久化
> 数据复制到其他的机器
> 如何处理数据系统的故障
> Redis的事务
> Redis性能诊断

## 持久化选项
> Redis提供2种持久化方式把数据写入磁盘，一种是快照(snapshotting)，一种是只追加文件(append-only file)
  * 快照的方式是把某个时刻的所有数据存储的硬盘，防止系统的故障以便于恢复
  * 追加文件是把所有的写命令存储在文件中，系统数据只要统一执行文件中的命令即可恢复系统

## Redis提供的持久化选项
  * 快照  对于时间级别较短备份具有较低的准确性
    - save 60 1000   执行快照备份的频率，表示在60秒之内如果有1000次写入操作即执行bgsave,可以有多个频率的设置，表示只要有一个条件满足，即执行
      fork一个子进程，子进程用来备份文件，父进程继续进行命令请求，在有执行关机shutdown或者TRAM命令时即执行save操作
    - stop-writes-on-bgsave-error on  是否在快照执行操作失败后进行写操作   
    - rdbcompression yes   是否对快照备份文件进行压缩
    - dbfilename dump.rdb  定义快照文件的名称
  * 追加文件  经常的追加文件使得恢复文件变得越来越大
    - appendonly yes  是否使用追加文件的策略
    - appendfsnync everysec  (alaways  everysec no)同步到硬盘的频率  即每秒
    - appendfsnync-on-write no 
    - auto-aof-rewrite-percetent 100
    - auto-aof-rewrite-min-size 64m
