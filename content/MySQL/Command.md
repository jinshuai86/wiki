---
title: "MySQL Command"
date: 2019-05-03
---

# 性能分析
## show profiles
查看执行SQL消耗的时间。  
首先通过`set profiling=1`提前打开`profiling`性能分析。执行完SQL语句以后，可以通过`show profiles`查看SQL耗时(Duration),单位是秒。

## 查询长事务
长事务会占用大量的回滚日志以及长时间持有锁资源。`information_schema.innodb_trx`记录了**正在执行**的事务信息。查询执行时长超过60s的事务。
`SELECT * FROM information_schema.innodb_trx where TIME_TO_SEC(timediff(now(),trx_started)) > 60` 

## 查看复制情况
`show slave status`。可以查看`Seconds_Behind_Master`列判断slave落后master几秒。