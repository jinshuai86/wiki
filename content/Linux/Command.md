---
title: "Linux Command"
date: 2019-04-05
---

# 文件处理
## find
用于查找文件，比如 find / \-name \*.java 查找根目录下的所有java文件
### 常用参数
#### -name 按名称查找
`find /home/js/ \-name test.jmx` 查找/home/js/目录下的test.jmx文件

#### -type 按类型查找(f(file)、d(directory)、l(link))
`find /home/js/ \-type d` 查找/home/js/目录下的类型是**目录**的文件

#### -atime/mtime 按访问时间/修改时间查找
`find /home/js -atime -7` 查找/home/js/目录下访问时间在7天以内的文件

#### -size 按大小查找(k(KB)、m(MB)、g(GB))
`find /home/js -size -1k` 查找/home/js/目录下大小小于1KB的文件

#### -perm 按权限查找(4(r)、2(w)、1(x))
`find /home/js -perm 444` 查找/home/js/目录下只读的文件