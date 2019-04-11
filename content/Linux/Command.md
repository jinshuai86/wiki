---
title: "Linux Command"
date: 2019-04-05
---

# 文件处理
涉及到查找文件、将文件内容加载到内存中进行操作。
## find
用于查找文件，比如 `find / -name *.java` 查找根目录下的所有java文件
### 参数
- \-name 按名称查找
`find /home/js/ -name test` 查找/home/js/目录下的test文件

- \-type 按类型查找(f(file)、d(directory)、l(link))
`find /home/js/ -type d` 查找/home/js/目录下的类型是**目录**的文件

- \-atime/mtime 按访问时间/修改时间查找
`find /home/js -atime -7` 查找/home/js/目录下访问时间在7天以内的文件

- \-size 按大小查找(k(KB)、m(MB)、g(GB))
`find /home/js -size -1k` 查找/home/js/目录下大小小于1KB的文件

- \-perm 按权限查找(4(r)、2(w)、1(x))
`find /home/js -perm 444` 查找/home/js/目录下只读的文件

## grep
查找文件中包含的关键字，最终显示的是包含关键字的所有行，比如 `grep "find" Command.md` 在Command.md中查找包含"find"关键字的行
### 参数
- \-e 多个模式匹配
`grep -e "find" -e "test" Command.md` 在Command.md中查找包含"find"和"test"关键字的行
- \-i 忽略大小写
- \-c(count) 打印包含的关键字的数量
- \-n 显示关键字所在的行号

## sort
对文件中的内容进行排序，排序在内存中进行，不会修改源文件。 比如 `sort /home/test` 默认是按字典序排序(-d)
### 参数
- \-n 按数字排序(前提是test里的文本是数字，且要排序的多个数字不能在同一行)
`sort -n /home/js/test` 对test里的每行内容按数字顺序排序 

- \-d 按字典序(要排序的多个字符串不能在同一行)
`sort -d /home/js/test` 对test里的每行内容按字典顺序排序 

- \-k N 表示每行按第N列排序
`sort -k 1 /home/js/test` 对test里的每行按第 1 列排序

- \-r 和其它参数组合实现逆序排序
`sort -r -k 1 /home/js/test` 对test里的每行按第 1 列逆序排序

## tr(transfer)
转换文本中的内容，转换在内存中进行，不会修改源文件。 需要将其它程序的输出作为输入进行处理。 `less test | tr a b > out` 将less获取的输出作为输入，然后将其中的a转换成b输出到out中。

### 参数
- \-s 将**连续的**字符或者字符串转换成指定字符或者字符串
`less test | tr -s 'ab' 'b'` 将test中连续的ab替换成b的内容作为输出

- \-d 删除字符
`less test | tr -d a` 删除test中出现的a字符的内容作为输出

## cut
截取每一行中指定的字节/字符/列，截取在内存中进行，不会修改源文件。比如：`cut -c 1-3 test`，截取test中每一行的第一字符**至**第三字符。
### 参数
- \-b 按字节截取
`cut -b 1-3 test` 截取test中每一行的第一字节**至**第三字节。

-  \-d 按指定分隔符截取，\-f 按字段截取。每一行的列与列之间通过`-d`区分，否则会当成一个字段
`cut -d ' ' -f 1-3 test` 截取test中每一行中的第一列**至**第三列,列与列之间以空格作为间隔

## paste
将多个文件中的内容水平拼接在一块，文件与文件之间的内容默认是以制表符作为间隔。比如：`paste test test2` 将test和test2中的内容拼接在一起
### 参数
- \-d 自定义文件之间的间隔符
`paste -d '|' test test2` 用 `|`作为test和test2的间隔符。

- \-s 一次性读取完一个文件作为一行，文件之间垂直拼接
`paste -s test test2` 先从左到右，从上到下读取完test中的内容放到第一行，然后再从左到右，从上到下读取test2中的内容放到下一行。

## wc
统计文件中的字节数/字符数/单词数量/行数。 比如 `wc -w test`统计test中的单词数量
### 参数
- \-c 统计字节数量
- \-m 统计字符数量
- \-w 统计单词数量
- \-l 统计行数

# 系统信息
查看机器的运行状态，比如进程快照信息，IO信息，内存/磁盘使用情况。
## ps(process status)
查看进程状态**快照**，比如`ps -aux|grep java` 打印关于Java进程的快照
### 参数
- \-aux/-ef 查看系统正在运行的**所有**进程快照
标准的语法： `ps -ef` 基于BSD的语法 `ps -aux`

## top
实时监控进程状态信息。并且可以交互(按P则会按照CPU使用率排序，按M则会按内存使用率排序)。比如 `top -u js` 实时查看用户`js`开启的进程信息
### 参数
- \-p pid 实时监控进程号为`pid`的进程信息
`ps -p 20348` 监控进程号为`20348`的进程信息
- \-Hp pid 实时监控进程号为`pid`的**线程**状态
`ps -Hp 20348` 监控进程号为`20348`的进程内部的线程状态
- \-d seconds 每seconds秒刷新一次

## lsof(list open files)
查看打开的文件，比如 `lsof -p 23478` 查看进程号为`23478`打开的文件
### 参数
- \-i [46][protocol][@hostname|hostaddr][:service|port] 列出具有指定网络地址的文件(网络地址包括网络层协议版本IPV6/4、运输层协议类型TCP/UDP、主机名、端口号。这几个是可选的，如果不写就默认所有)
`lsof -i :80` 列出网络地址中网络层是IPV6或IPV4并且运输层是TCP或UDP并且主机名是任意的，**但是占用的端口号是80的文件**。 意思就是查看占用80端口的进程。
- \-u user 查看指定用户打开的文件
`lsof -u js` 查看用户`js`打开的文件
- \-p pid 查看指定**进程号**打开的文件
`lsof -p 23478` 查看进程号为`23478`打开的文件
- \-c command 查看指定**进程**
`lsof -p java` 查看命令为`java`的打开的文件
- `+`d directory 查看指定目录下被打开的文件 `+`D加上递归
`lsof +d /home/js/workspace` 查看/home/js/workspace下被打开的文件

## free
查看内存及交换区使用情况
### 参数
- \-b 以字节(B)形式显示
- \-k 以千字节(KB)形式显示 
- \-m 以兆字节(MB)形式显示
- \-g 以吉字节(GB)形式显示
# 参考
- [https://www.geeksforgeeks.org/linux-commands/](https://www.geeksforgeeks.org/linux-commands/)