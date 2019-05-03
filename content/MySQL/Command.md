---
title: "MySQL Command"
date: 2019-05-03
---

# 性能分析
## show profiles
查看执行SQL消耗的时间。  
首先通过`set profiling=1`提前打开`profiling`性能分析。执行完SQL语句以后，可以通过`show profiles`查看SQL耗时(Duration),单位是秒。