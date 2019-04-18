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
---

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
查看内存及交换区使用情况 比如 `free -b` 以字节的形式显示内存使用情况
### 参数
- \-b 以字节(B)形式显示
- \-k 以千字节(KB)形式显示 
- \-m 以兆字节(MB)形式显示
- \-g 以吉字节(GB)形式显示

## sar
监控CPU使用率、内存使用率、磁盘IO情况、网卡工作情况 比如 `sar -b 1 2` 查看磁盘IO情况，每一秒显示一次，共显示两次。
### 参数
- \-q 查看运行队列中进程数量、进程总数量、系统平均负载(单位时间内(等待队列中进程数量+运行队列中进程数量) / CPU最高能处理的进程数量)。单核负载=负载/CPU总核数。每个核的负载在0.7左右比较正常。
- \-u 查看CPU的使用率
- \-r 内存使用情况

# 网络工具
查看网络信息，比如查看处于不同连接状态(运输层是TCP/UDP、已经建立或者正在监听)的连接。发送网络请求
## netstat
查看网络连接状态信息。比如 `netstat -t` 查看已经建立的TCP连接信息
### 参数
- \-a 查看所有状态的连接(已经建立、正在监听)
- \-t 查看已经建立的TCP连接信息
- \-u 查看已经建立的UDP连接信息
- \-l 查看处于监听状态的连接
- \-n 不解析名称，加上后，端口号以数字显示
- \-p 查看处于当前连接的进程名/进程ID 
>`netstat -tp` 查看协议是TCP的进程名  
`netstat -np | grep nginx` 查看和`nginx`进程有关的端口号(本地端口、远程端口)  
`netstat -np | grep 80` 查看和`80`端口号有关的连接(**不一定是占用**，可能请求的远程地址对应的端口也是80)  
`netstat -ntlp | grep ` `nginx`进程正在监听的端口号

## curl
重点是模拟发送网络请求(HTTP、FTP、SCP、POP3、SMTP...)，不是下载数据。 默认重定向到标准输出。
### 参数
- \-o/O 指定保存的文件名/使用请求的文件的文件名
`curl -o test.html jinshuai86.github.io/wiki/Linux/Command.html` 将`Command.html`中的内容保存到`test.html`中  
`curl -O jinshuai86.github.io/wiki/Linux/Command.html` 将`Command.html`中的内容保存到本地文件，本地文件的文件名为`Command.html`。  
- \-L 重定向。比如遇到`301`、`302`时，`curl`默认不会重定向到`location header`中的`URL`，加上`-L`就会重定向到`location header`中的`URL`。
`curl jinshuai86.github.io/about`请求的内容是`301`页面，加上`-L`即可重定向到新的页面。
- \-C 对大文件的下载支持断点续传。 注意下载文件要用 `-o/O`，`C`和`o/O`之间有`-`分隔
`curl -C - -O http://www.gnu.org/software/gettext/manual/gettext.html`
- \-d/--data 模拟发送HTTP POST请求 加上`-urlencode`会对特殊字符进行转义比如`空格=>%20`
`curl -d-urlencode "username=jin shuai&password=123" http://jinshuai86.github.io`
- \-D 获取服务器要在本地设置的`cookie`。 比如 `curl -D cookie https://www.github.com`将要设置的`cookie`保存到`cookie`文件中
- \-b 发送请求时，使用cookie。可以使用通过`-D`设置的cookie。比如 `curl -b cookie https://www.github.com`

## wget
下载数据，相对`curl`，`wget`支持递归下载
### 参数
- \-r 进行递归下载，将当前页面内包含的其它链接内容也一并下载下来
- \-\-reject=filetype,filetype 不下载执行类型的文件。多个类型用`,`隔开。 比如`wget --reject=css,js jinshuai86.github.io/about` 不下载`css`和`js`类型的文件  
- \-\-accept=filetype,filetype 只下载指定类型的文件。多个类型用`,`隔开。

## scp
远程传输文件，基于`SSH`协议。`scp`相对于`ftp`、`rsync`，传输速度较快，对系统影响不大。  
比如 `scp localfile remoteIP:folder` 将**本地文件**`localfile`传输到IP地址为`remoteIP`的`foler`文件夹中  
`Windows`下有图形界面的`winscp`工具支持`scp`
## 参数
- \-r 递归传输整个目录

## 常用
### 上传本地文件到远程服务器
`scp -r upload 114.114.114.114:~/download` 将本地文件夹`upload`传输到`114.114.114.114`的`~/download`文件夹  

### 复制远程服务器文件到本地(远程需要开SSH服务)
`scp 114.114.114.114:~/upload/note.md ~/download` 将远程`114.114.114.114`的`node.md`下载到本地的`~/download`目录  


# 其它
- cd 切换目录
- ls 列出目录中的文件
- cp 拷贝文件
- mkdir 创建文件夹
- touch 创建文件
- rm 删除文件/文件夹
- mv 移动文件/文件夹、修改文件名
- less 分页查看文件内容。其它还有cat、tac、
- head 查看文件首部内容，默认前10行。
- tail 查看文件尾部内容，默认后10行。一般加`-f`可以实时查看
- chmod 修改文件/文件夹权限 `ugo rwx=421`。 一般加 `-R` 递归修改
- kill 杀死进程。 一般加\-9杀死某进程相关进程

# 参考
- [https://www.geeksforgeeks.org/linux-commands/](https://www.geeksforgeeks.org/linux-commands/)
- [https://linuxtools-rst.readthedocs.io/zh_CN/latest/index.html](https://linuxtools-rst.readthedocs.io/zh_CN/latest/index.html)