---
title: "Linux Command"
date: 2019-04-05
---

# 文件处理
## find
用于查找文件，比如 find / \-name \*.java 查找根目录下的所有java文件
### 常用参数
#### -name 按名称查找
`find /home/js/ -name test.jmx` 查找/home/js/目录下的test.jmx文件

#### -type 按类型查找(f(file)、d(directory)、l(link))
`find /home/js/ -type d` 查找/home/js/目录下类型是目录的文件

#### -atime/mtime 按访问时间/修改时间查找
`find /home/js -atime -7` 查找/home/js/目录下访问时间在7天以内的文件

#### -size 按大小查找(k(KB)、m(MB)、g(GB))
`find /home/js -size -1k` 查找/home/js/目录下小于1KB的文件

#### -perm 按权限查找(4(r)、2(w)、1(x))
`find /home/js -perm 444` 查找/home/js/目录下只读的文件

## grep
查找文件中包含的关键字，最终显示的是包含关键字的所有行，比如 grep "find" Command.md 在Command.md中查找包含"find"关键字的行
### 常用参数
#### -e 多个模式匹配
`grep -e "find" -e "test" Command.md` 在Command.md中查找包含"find"和"test"关键字的行
#### -i 忽略大小写
#### -c(count) 打印包含的关键字的数量
#### -n 显示关键字所在的行号
