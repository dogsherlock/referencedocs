> 这是一些比较常用的命令, 大家可以复制后用typora做成pdf格式，方便快速查询
> 后续不定期更新


##### 文件描述符
> 在linux系统中0、1、2是一个文件描述符
> 0 标准输入(stdin)
> 1 标准输出(stdout)
> 2 错误输出(stderr)

* 2>&1的含义
> 将标准错误输出重定向到标准输出
> 符号>&是一个整体，不可分开，分开后就不是上述含义了

##### 目录即文件
* /dev/null
等价于只写文件.所有写入它的内容都会永远丢失,而尝试从它那儿读取内容则什么也读不到
禁止标准输出.   cat $filename >/dev/null   # 文件内容丢失，而不会输出到标准输出. 
禁止标准错误.   2>/dev/null 

* /dev/zero
一个永远输出0的设备文件,使用它作输入可以得到全为空的文件.因此可用来创建
新文件和以覆盖的方式清除旧文件
/dev/zero在类UNIX系统中是一个特殊的设备文件
/dev/zero在被读取时会提供无限的空字符（ASCII NUL, 0x00）

##### 换行
windows换行是\r\n(CRLF)-carriage return/line feed
linux换行是\n(LF)

##### 配置文件
* 所有用户的环境变量配置
/etc/profile

##### linux中各种文件和含义
```
绿色文件---------- 可执行文件，可执行的程序 
红色文件-----------压缩文件或者包文件
蓝色文件----------目录    
白色文件----------普通，如文本文件，配置文件，源码文件等 
浅蓝色文件----------链接文件，主要是使用ln命令建立的文件
红色闪烁----------表示链接的文件有问题
黄色文件----------表示设备文件
灰色文件----------表示其它文件
```


##### linux各种信号列表
kill -l
```
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX	
```
ctrl-c 发送 SIGINT 信号给前台进程组中的所有进程 常用于终止正在运行的程序
ctrl-z 发送 SIGTSTP 信号给前台进程组中的所有进程，常用于挂起一个进程
ctrl-d 不是发送信号，而是表示一个特殊的二进制值，表示 EOF
ctrl-\ 发送 SIGQUIT 信号给前台进程组中的所有进程，终止前台进程并生成 core 文件

Ctrl+Z强制当前进程转为后台，并使之停止 
Ctrl+c前台进程的终止
Ctrl+\表示退出
###### 列出所有资源的限制： ulimit -a

###### 什么是core dump文件
Coredump叫做核心转储，它是进程运行时在突然崩溃的那一刻的一个内存快照
操作系统在程序发生异常而异常在进程内部又没有被捕获的情况下，会把进程此刻内存
、寄存器状态、运行堆栈等信息转储保存在一个文件里

该文件也是二进制文件，可以使用gdb、elfdump、objdump或者windows下的windebug
、solaris下的mdb进行打开分析里面的具体内容

###### core文件的位置和配置
coredump文件默认存储位置与可执行文件在同一目录下，文件名为core
```
%p  出Core进程的PID
%u  出Core进程的UID
%s  造成Core的signal号
%t  出Core的时间，从1970-01-0100:00:00开始的秒数
%e  出Core进程对应的可执行文件名
```
可以通过此进行设置
```
[root@OSSM-1 log]# cat /proc/sys/kernel/core_pattern
/export/home/omc/var/logs/core.%e.%p
```

在每个进程下都有coredump_filter节点/proc/<pid>/coredump_filter
通过配置coredump_filter可以选择需在coredump的时候，将哪些内容dump到core文件中
```
  - (bit 0) anonymous private memory
  - (bit 1) anonymous shared memory
  - (bit 2) file-backed private memory
  - (bit 3) file-backed shared memory
  - (bit 4) ELF header pages in file-backed private memory areas (it is effective only if the bit 2 is cleared)
  - (bit 5) hugetlb private memory
  - (bit 6) hugetlb shared memory
  - (bit 7) DAX private memory
  - (bit 8) DAX shared memory
```


##### 设置core文件大小
core file size：
unlimited：core文件的大小不受限制
0：程序出错时不会产生core文件
1024：代表1024k，core文件超出该大小就不能生成了
设置core文件大小： ulimit -c fileSize
尽量将这个文件大小设置得大一些，程序崩溃时生成Core文件大小即为程序运行时占用的内存大小
可能发生堆栈溢出的时候，占用更大的内存


##### sysctl
用于运行时配置内核参数, 这些参数位于/proc/sys目录下
用户只需要编辑/etc/sysctl.conf文件，即可手工或自动执行由sysctl控制的功能

sysctl -a显示所有的系统参数
sysctl -p从指定的文件加载系统参数, 如不指定即从/etc/sysctl.conf中加载
sysctl -a | grep core_pattern显示coredump path


##### 常用linux命令

###### 简单命令
* 在空命令行的情况下退出终端 Ctrl+d
* 重定向子进程的错误输出，让错误输出重定向到标准输出（2>&1）
command 2>&1
* 将标准输入stdout和标准输出stderr都重定向到/dev/null
command >/dev/null 2>&1
* id -u <username> 获取指定用户的uid

代表重定向到哪里，例如：echo "123" > /home/123.txt 
1 表示stdout标准输出，系统默认值是1，所以">/dev/null"等同于"1>/dev/null" 
2 表示stderr标准错误 
& 表示等同于的意思，2>&1，表示2的输出重定向等同于1 
1>/dev/null 首先表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，说白了就是不显示任何信息。
2>&1 接着，标准错误输出重定向等同于标准输出，因为之前标准输出已经重定向到了空设备文件，
所以标准错误输出也重定向到空设备文件

* 设置linux系统的空闲等待时间
1. shell中直接设置TMOUT=0 
2. 配置/etc/profile文件
export TMOUT=900 # 设置900s内用户无操作就自动断开终端
readonly TMOUT # 将值设置为readonly防止用户更改

* 查看linux支持的编码
locale -a
-a, --all-locales Write names of available locales 
* 查看linux系统编码
locale
* 修改linux终端编码
```console
user@kwephis37764:~> export LC_ALL=zh_CN.gbk
user@kwephis37764:~> echo $LC_CTYPE
cp936
```

* 清空文件内容
> a.text 
echo "" > a.txt
cat /dev/null > a.txt

* 逐页阅读
more [options] filename
options: 
-n 定义屏幕大小为n行
+n 从第n行开始显示
常用操作:
space键往下一页，b键往回一页显示
/ 向下搜索"字符串"的功能
? 向上搜索"字符串"的功能
=输出当前行的行号
q退出more

* 显示档案的开头至标准输出中
head [options][filename]
-n 行数: 显示的行数
eg:
head -n 5 text.txt python.text 
-c 字节: 显示字节数
eg: head -c 100 /etc/profile

* 显示指定文件末尾内容,查看日志文件
tail [options] filename
options:
-f 循环读取
-q 不显示处理信息
-v 显示详细的处理信息
-c 字节 显示的字节数
-n 行数 显示行数



###### 系统相关命令
linux资源限制配置文件是/etc/security/limits.conf
limits.conf文件限制着用户可以使用的最大文件数,最大线程,最大内存等资源使用量

* 查看系统内存
[root@kwephis37773 6721]# free -g
              total        used        free      shared  buff/cache   available
Mem:             31          27           0           0           3           3
Swap:             3           3           0
total - This number represents the total amount of memory that can be used by the applications.
used - Used memory. It is calculated as: used = total - free - buffers - cache
free - Free / Unused memory.
shared - This column can be ignored as it has no meaning. It is here only for backward compatibility.
buff/cache - The combined memory used by the kernel buffers and page cache and slabs. This memory can be reclaimed at any time if needed by the applications. If you want buffers and cache to be displayed in two separate columns, use the -w option.
available - An estimate of the amount of memory that is available for starting new applications, without swapping

* 查看atime, ctime, mtime
1. mtime(modify time): 最后一次修改文件或目录的时间
2. ctime(change time) : 最后一次改变文件或目录( 改变的是原数据即: 属性) 的时间
如:记录该文件的inode节点被修改的时间。touch命令除了-d和-t选项外都会改变该时间。而且chmod,chown等命令也能改变该值
3. atime(access time):: 最后一次访问文件或目录的时间
stat filenmae 

* 获取当前内核的名称和信息概要
uname -a

* ulimit设置当前shell以及由它启动的进程的资源限制 
Provides control over the resources avaliable to the shell and to processes started by it, on systems
that allow such control. 
ulimit命令
-H 使用硬资源控制
-S 使用软资源控制
-a 查看所有的当前限制
-n 能打开的最大文件描述符数 默认是soft limit
-s 分配堆栈的最大大小
-u 用户可以开启进程/线程的最大数目max user processes
-t 限制最大的 CPU 占用时间（每秒）
-u 限制最大用户进程数
-v 限制虚拟内存大小（kB）
eg:
ulimit -n默认查看的是soft limit
[root@kwephis37773 6721]# ulimit -n
240000
查看hard limit
[root@kwephis37773 6721]# ulimit -Hn
240000
[root@kwephis37773 6721]# ulimit -a
core file size          (blocks, -c) unlimited
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 127849
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 240000
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 2048
cpu time               (seconds, -t) unlimited
max user processes              (-u) 240000
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
注意：设置nofile的hard limit还有一点要注意的就是hard limit不能大于/proc/sys/fs/nr_open，假如hard limit大于nr_open，注销后将无法正常登录。
例如，假设用户有 10,000 个块的soft limit和 12,000 个块的hard limit.如果用户的块使用量超过 10,000 个块并且也超过了计时器(超过 7 天)，则用户将无法在该文件系统上分配更多磁盘块，直到他或她的使用量降至软限制以下

* 设置当前shell和shell中创建的进程最大打开文件描述符数
临时设置
#通过ulimit -Sn设置最大打开文件描述符数的soft limit，注意soft limit必须小于hard limit
$ ulimit -Sn 160000
#同时设置soft limit和hard limit。对于非root用户只能设置比原来小的hard limit。
$ ulimit -n 180000
永久设置
#root权限下，在/etc/security/limits.conf中添加如下两行，表示所有用户最大打开文件描述符数的soft limit为102400，
hard limit为104800。重启生效
> soft nofile 102400
> hard nofile 104800
注意：设置nofile的hard limit还有一点要注意的就是hard limit不能大于/proc/sys/fs/nr_open，假如hard limit大于nr_open，注销后将无法正常登录。


* 单个进程的资源限制 
cat /proc/<pid>/limits
Limit                     Soft Limit           Hard Limit           Units     
Max cpu time              unlimited            unlimited            seconds   
Max file size             unlimited            unlimited            bytes     
Max data size             unlimited            unlimited            bytes     
Max stack size            8388608              unlimited            bytes     
Max core file size        0                    unlimited            bytes     
Max resident set          unlimited            unlimited            bytes     
Max processes             29397                29397                processes 
Max open files            1024                 4096                 files     
Max locked memory         65536                65536                bytes     
Max address space         unlimited            unlimited            bytes     
Max file locks            unlimited            unlimited            locks     
Max pending signals       29397                29397                signals   
Max msgqueue size         819200               819200               bytes     
Max nice priority         0                    0                    
Max realtime priority     0                    0                    
Max realtime timeout      unlimited            unlimited            us  

* 系统最大打开文件描述符数量
[root@kwephis37773 6721]# cat /proc/sys/fs/file-max
3245657

* 查看当前系统使用的打开文件描述符数
[root@kwephis37773 6721]# cat /proc/sys/fs/file-nr
39872 0 3245657
其中第一个数表示当前系统已分配使用的打开文件描述符数，第二个数为分配后已释放的（目前已不再使用），第三个数等于file-max



###### 文件校验
计算md5哈希值, echo会默认添加一个换行符 echo -n '123456' | [md5sum, sha1sum, sha256sum] -n可以忽略换行符

###### 进程前后台切换
使进程恢复运行(后台) bg  
fg将后台中的命令调至前台继续运行
jobs显示当前暂停的进程


###### 查看端口号被哪个进程占用
lsof list open files 需要root权限
lsof -i:8080：查看8080端口占用
lsof abc.txt：显示开启文件abc.txt的进程
lsof -c abc：显示abc进程现在打开的文件
lsof -p 1234：列出进程号为1234的进程所打开的文件
lsof -g gid：显示归属gid的进程情况
lsof +d /usr/local/：显示目录下被进程开启的文件
lsof +D /usr/local/：同上，但是会搜索目录下的目录，时间较长
lsof -d 4：显示使用fd为4的进程
lsof -i -U：显示所有打开的端口和UNIX domain文件


netstat 
netstat -tunlp 用于显示 tcp，udp 的端口和进程等相关情况
netstat -anp | grep 端口号
netstat -tunlp | grep 端口号
-a Show both listening and non-listening sockets
-t (tcp) 仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化为数字  Show numerical addresses instead of trying to determine symbolic host, port or user names. 
-l 仅列出在Listen(监听)的服务状态 Show only listening sockets.  (These are omitted by default.) 默认省略
-p Show the PID and name of the program to which each socket belongs.  A hyphen(连字符) is shown if the socket belongs to the kernel
(e.g. a kernel service, or the process has exited but the socket hasn't finished closing yet).


查看进程pid 
ps -ef | grep 进程名
通过pid查看占用端口
netstat -nap | grep 进程pid 

###### windows:
* hosts文件: C:\Windows\System32\drivers\etc\hosts(Linux: /etc/hosts)
查端口
netstat -aon|findstr port  
查进程
tasklist|findstr pid
查看进程的详细信息
查看所有运行中进程的命令行参数：
wmic process get caption,commandline /value
查询指定进程的命令行参数：
wmic process where caption="notepad.exe" get caption,commandline /value【精确查找】
wmic process where="caption like 'notepad%'" get caption,commandline /value【模糊查找】
netstat -nap | grep 
修改文件用户和所有者
chown [-R] 账号名称 文件或目录
chown [-R] 账号名称:用户组名称 文件或目录 
eg: chown ossuser:ossgroup install.log 文件拥有者为ossuser,用户组为ossgroup 

强制杀死进程
taskkill /F /PID 1304


* cmd控制台编码格式的设置
查看控制台的编码格式(默认是gbk, 即936): chcp

设置编码格式为utf8: chcp 65001

永久设置: 
1. regedit打开注册表->计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor->
新建字符串值->名称为AutoRun,数据为chcp 65001

2. windows10->cmd->intl.cpl->Administrative, 选择Beta: Use Unicode UTF-8 for worldwide language support然后重启


###### ps command
ps:
The ps command displays active processes


three style:
```
unix 
ps -aux

bsd
ps aux 

gnu: have two dashses in front of the option instead of just one

ps --quick-pid 2925 
```

ps without any options:
shows the processes that are running in your current session
```
[root@kwephisprg01362 ~]# ps
  PID TTY          TIME CMD
26748 pts/0    00:00:00 bash
28001 pts/0    00:00:00 ps

The four columns are labeled PID, TTY, TIME, and CMD.

PID - The process ID. Usually, when running the ps command, the most important information the user is looking for is the process PID.Knowing the PID allows you to kill a malfunctioning process .
TTY - The name of the controlling terminal for the process.
TIME - The cumulative CPU time of the process, shown in minutes and seconds.
TIME是CPU真正执行代码的时间，当进程正在等待网络或磁盘或只是休眠时, 此计数器不会增加
CMD - The name of the command that was used to start the process.
A TTY is a computer terminal. In the context of ps , it is the terminal that executed a particular command.
TPGID - refers to the terminal session id with which the process is associated
UID - user id that started that particular process,0 is root
```

* 查看进程的关系
ps -axjf

* ps命令获取指定进程的执行目录、环境变量、完整的命令行
```
w      Wide output.  Use this option twice for unlimited width.
e      Show the environment after the command.
```
ps eww -p <process_pid>

* 查看文件的方式获取信息: 执行目录、环境变量、完整的命令行
ll /proc/<process_pid>
cat /proc/1/environ | tr '\000' '\n'

```
cwd     符号链接的是进程运行目录
exe     符号连接就是执行程序的绝对路径
cmdline 就是程序运行时输入的命令行命令
environ 记录了进程运行时的环境变量
fd      目录下是进程打开或使用的文件的符号连接
cwd       链接的是启动进程的目录(也就是输入命令行所在的目录)
environ:  记录该进程当时所有环境变量(常用 tr '\0' '\n' < /proc/<process_pid>/environ 命令将环境变量一个属性一行的显示)
cmdline:  运行进程当时执行的完整命令
```

###### proc command
proc - process information pseudo-filesystem

```
The proc filesystem is a pseudo-filesystem which provides an
interface to kernel data structures.  It is commonly mounted at
/proc
```

/proc/[pid]/environ:
```
This file contains the initial environment that was set
when the currently executing program was started via
execve(2). The entries are separated by null bytes
('\0'), and there may be a null byte at the end.
cat /proc/1/environ | tr '\000' '\n'
```


###### find
find:
在当前目录下查找指定路径的文件, 可以使用正则表达式
find  . -ipath */zh_CN/Res.properties
查找目录
find /（查找范围） -name '查找关键字' -type d
find /  -name DesktopService -type d
若不指定查找类型
find / -name AnmyTest则会将目录和文件一同输出
若指定查找类型
find / -name AnmyTest -type d则只会将目录输出
查看前两行和后两行 
```
ls -al | head -2
ls -al | tail -2
```

###### scp command
复制目录 
从远处复制文件到本地目录
scp root@10.6.159.147:/opt/soft/demo.tar /opt/soft/
从远处复制到本地
scp -r root@10.6.159.147:/opt/soft/test /opt/soft/
上传本地文件到远程机器指定目录
scp /opt/soft/demo.tar root@10.6.159.147:/opt/soft/scptest
上传本地目录到远程机器指定目录
scp -r /opt/soft/test root@10.6.159.147:/opt/soft/scptest

###### grep command(global regular expression print)
$ grep --help
Usage: grep [OPTION]... PATTERN [FILE]...
Search for PATTERN in each FILE.
Example: grep -i 'hello world' menu.h main.c

Pattern selection and interpretation:
  -E, --extended-regexp     PATTERN is an extended regular expression
  -F, --fixed-strings       PATTERN is a set of newline-separated strings
  -G, --basic-regexp        PATTERN is a basic regular expression (default)
  -P, --perl-regexp         PATTERN is a Perl regular expression
  -e, --regexp=PATTERN      use PATTERN for matching
  -f, --file=FILE           obtain PATTERN from FILE
  -i, --ignore-case         ignore case distinctions
  -w, --word-regexp         force PATTERN to match only whole words
  -x, --line-regexp         force PATTERN to match only whole lines
  -z, --null-data           a data line ends in 0 byte, not newline

Miscellaneous:
  -s, --no-messages         suppress error messages
  -v, --invert-match        select non-matching lines
  -V, --version             display version information and exit
      --help                display this help text and exit

Output control:
  -m, --max-count=NUM       stop after NUM selected lines
  -b, --byte-offset         print the byte offset with output lines
  -n, --line-number         print line number with output lines
      --line-buffered       flush output on every line
  -H, --with-filename       print file name with output lines
  -h, --no-filename         suppress the file name prefix on output
      --label=LABEL         use LABEL as the standard input file name prefix
  -o, --only-matching       show only the part of a line matching PATTERN
  -q, --quiet, --silent     suppress all normal output
      --binary-files=TYPE   assume that binary files are TYPE;
                            TYPE is 'binary', 'text', or 'without-match'
  -a, --text                equivalent to --binary-files=text
  -I                        equivalent to --binary-files=without-match
  -d, --directories=ACTION  how to handle directories;
                            ACTION is 'read', 'recurse', or 'skip'
  -D, --devices=ACTION      how to handle devices, FIFOs and sockets;
                            ACTION is 'read' or 'skip'
  -r, --recursive           like --directories=recurse
  -R, --dereference-recursive  likewise, but follow all symlinks
      --include=FILE_PATTERN  search only files that match FILE_PATTERN
      --exclude=FILE_PATTERN  skip files and directories matching FILE_PATTERN
      --exclude-from=FILE   skip files matching any file pattern from FILE
      --exclude-dir=PATTERN  directories that match PATTERN will be skipped.
  -L, --files-without-match  print only names of FILEs with no selected lines
  -l, --files-with-matches  print only names of FILEs with selected lines
  -c, --count               print only a count of selected lines per FILE
  -T, --initial-tab         make tabs line up (if needed)
  -Z, --null                print 0 byte after FILE name

Context control:
  -B, --before-context=NUM  print NUM lines of leading context
  -A, --after-context=NUM   print NUM lines of trailing context
  -C, --context=NUM         print NUM lines of output context
  -NUM                      same as --context=NUM
      --color[=WHEN],
      --colour[=WHEN]       use markers to highlight the matching strings;
                            WHEN is 'always', 'never', or 'auto'
  -U, --binary              do not strip CR characters at EOL (MSDOS/Windows)

* 显示系统上的正则
$ grep -V
grep (GNU grep) 3.1
linux是GNU grep/mac是BSD grep

###### 搜索文本在特定的文件中
* 搜索文件中匹配的文件的行
grep "content" filename

* 扩展正则表达式 
-E, --extended-regexp     PATTERN is an extended regular expression
查询一组pid的进程信息
ps -ef | grep -E '67227|67413|67415|67416'
查看testcase.properties文件中有多少个LINUX_SF_AS_CMD
grep -o获取到所有匹配的列, wc -l获取有多少行
grep -o 'LINUX_SF_AS_CMD' testcase.properties |wc -l

* 完全匹配单词模式
-w, --word-regexp         force PATTERN to match only whole words

* 忽略大小写区分 默认是case sensitive
-i, --ignore-case         ignore case distinctions

* 附加显示匹配的行数
-n, --line-number         print line number with output lines
eg：
$ cat test.txt
clarence
clarences
Clarence

D:\coding\shellpractice
$ grep -win "clarence" test.txt
1:clarence
3:Clarence

* see the lines behind my match
-B, --before-context=NUM  print NUM lines of leading context
eg: 
grep -win -B 4 "clarence" test.txt

* see the lines after my match 
-A, --after-context=NUM

* 查看匹配的上下文(see the context serrounding our match)
-C, --context=NUM 

* show what files contain a match
-l, --files-with-matches

* 显示匹配字符串的文件以及每个文件匹配的个数
-c, --count               print only a count of selected lines per FILE

* 使用正则来匹配
eg:
-P 表示使用类似Perl的正则表达式 
grep -P "\d{3}-\d{3}-d{4}" names.txt

###### 搜索文本在多个文件中

* 在当前目录中搜索包含clarence单词的文件 -l只打印匹配的文件
`grep -winl "clarence" ./*.txt`

* 递归搜索(recursive search subdirectories)
-r, --recursive           like --directories=recurse
`grep -irn "APP_SHARE_DIR"`


###### sed command
eg:
$‘\r‘: command not found的解决方法
出现这样的错误，是因为Shell脚本在Windows系统编写时，每行结尾是\r\n，
而在Linux系统中行每行结尾是\n，所以在Linux系统中运行脚本时，
会认为\r是一个字符，导致运行错误
* 方法1 
去除Shell脚本的\r字符
sed -i 's/\r//' example.sh
* 方法2
yum install -y dos2unix
dos2unix example.sh
dos2unix: converting file example.sh to Unix format ...


* 展示进程树display a tree of processe 
pstree $$(当前shell的pid)
[sopuser@kwephis37773 ~]$ pstree $$
bash───pstree

pstree  用户名(当前用户所有运行的进程)
[sopuser@kwephis37773 ~]$ pstree sopuser
sshd───bash───pstree


##### tr command
a command line utility for translating or deleting characters.


##### 多个命令执行
* 按顺序执行多个命令,无论命令是否执行成功
command1;command2
* 按顺序执行，且如果前一个的命令执行发生错误,后一个命令则不会被执行
command1&&command2
* 按顺序执行,且当前一个命令执行成功时,后一个命令就不会执行
command1||command2



source a.sh 
当前shell, a.sh不需要有"执行权限"
可简写为: 
. a.sh 

sh/bash
打开一个subshell,a.sh不需要有"执行权限"
sh 遵循POSIX规范：“当某行代码出错时，不继续往下解释”bash 就算出错，也会继续向下执行
sh是bash的一种特殊的模式，sh就是开启了POSIX标准的bash， /bin/sh 相当于 /bin/bash --posix

./ 
打开一个subshell,a.sh需要有"执行权限",也可以直接运行可执行文件

a.run
```python
#!/usr/bin/python
print("This is Python script")
```
如果我直接运行./a.sh，首先你会查找脚本第一行是否指定了解释器，
如果没指定，那么就用当前系统默认的shell(大多数linux默认是bash)，如果指定了解释器，那么就将该脚本交给指定的解释器
那么你如果运行./a.run，结果就是输出一行文字，但是如果你运行sh a.run，会报错
因为这是一个python脚本，sh看不懂(注意，linux下后缀通常不是很严格，.run后缀是我随意命名的，和windows有点区别).

##### du command
查看文件或目录所占用的磁盘空间的大小(通过递归累加)
du -shm filepath



##### curl下载文件
使用linux的重定向功能保存
###### 基本用法
curl命令:
-o 下载文件并指定文件名保存
-O 下载文件与远程文件名相同
-L 跟随重定向请求
-H 设置头信息
-k 允许发起不安全的ssl请求
-b 设置cookie
-s 不显示无关信息
-v 显示连接的所有信息
-X 请求协议
-d POST内容
-i --include Include protocol headers in the output 显示携带响应头
-I --head Show document info only 


```shell
curl http://www.linux.com >> linux.html
```

下载的文件名默认是linux.zip
```shell
curl -O http://www.linux.com/linux.zip
```

获取外网IP
```shell
curl -L ip.tool.lu --proxy "http://username:password@proxyhk.huawei.com:8080/" -k
```

显示request header request body/response header response body
```shell
curl -I -v http://ipinfo.io
```

显示response header response body
```shell
curl -i http://ipinfo.io
````


发送POST请求
```shell
curl -H "Content-Type: application/json" -X POST -d "{"user_id": "123", "coin": 100, "success": 1, "msg": "OK!"}" http://192.168.0.1:8001/test
```
如需发送PUT请求 可以使用-X PUT指定

###### 配置代理
username和password需要urlencode编码(如果有特殊字符)
curl https://www.python.org/ -v --proxy "http://username:password@host:port/" -k
-k表示忽略证书认证, -v表示显示详情


##### 文件校验

###### windows计算哈希值
certutil -hashfile C:\Windows\SysWOW64\msvcrt.dll sha256

##### windows environment variables
* %USERPROFILE%
> user profile folder
> %USERPROFILE%\.ssh

* %HOME%
> 用户目录 C:\Users\username

* %APPDATA%
> C:\Users\username\AppData\Roaming

* %SystemRoot%
> 操作系统的系统目录或者是根目录
$ echo %SystemRoot%
C:\WINDOWS



##### 密码学相关
###### openssl
###### openssl genrsa

root@OSSSVR02-1:/tmp/username>openssl genrsa --help
Usage: genrsa [options]
Valid options are:
 -help               Display this summary
 -3                  Use 3 for the E value
 -F4                 Use F4 (0x10001) for the E value
 -f4                 Use F4 (0x10001) for the E value
 -out outfile        Output the key to specified file
 -rand val           Load the file(s) into the random number generator
 -writerand outfile  Write random data to the specified file
 -passout val        Output file pass phrase source
 -*                  Encrypt the output with any supported cipher
 -engine val         Use engine, possibly a hardware device
 -primes +int        Specify number of primes 
* 生成rsa私钥文件
openssl genrsa -out source.pem -f4 2048

* 查看私钥文件的模数n, -f4表示使用的是65537迭代次数
openssl rsa -text -noout -in source.pem | head -1


###### pkcs8
root@OSSM-1:/home/sopuser>openssl pkcs8 --help
Usage: pkcs8 [options]
Valid options are:
 -help               Display this summary
 -inform PEM|DER     Input format (DER or PEM)
 -outform PEM|DER    Output format (DER or PEM)
 -in infile          Input file
 -out outfile        Output file
 -topk8              Output PKCS8 file
 -noiter             Use 1 as iteration count
 -nocrypt            Use or expect unencrypted private key
 -rand val           Load the file(s) into the random number generator
 -writerand outfile  Write random data to the specified file
 -v2 val             Use PKCS#5 v2.0 and cipher
 -v1 val             Use PKCS#5 v1.5 and cipher
 -v2prf val          Set the PRF algorithm to use with PKCS#5 v2.0
 -iter +int          Specify the iteration count
 -passin val         Input file pass phrase source
 -passout val        Output file pass phrase source
 -traditional        use traditional format private key
 -engine val         Use engine, possibly a hardware device
 -scrypt             Use scrypt algorithm
 -scrypt_N val       Set scrypt N parameter
 -scrypt_r val       Set scrypt r parameter
 -scrypt_p val       Set scrypt p parameter

*  使用pcks#8格式来转换私钥
openssl pkcs8 -topk8 -v2 aes-256-cbc -in source.pem -passout pass:%s -out output.pem >/dev/null 2>&1
-topk8读取private key然后生成pkcs#8形式的key
-v2设置pkcs#5 v2.0算法(ase128/aes256/des3) 默认aes256
-in 指定要读取key的文件
-passout 输出文件密码源
-out 指定输出文件



##### 性能测试
###### ab command
* ab(ApacheBench)
> 压力测试工具，测试http服务器请求的性能情况
> [download location](https://www.apachehaus.com/cgi-bin/download.plx)
> apt get install apache2-util
> 安装后ab直接放在了apache安装目录下
> 如果不想安装apache, yum -y install httpd-tools(ab -V查看是否安装成功)

* options 
```
-n: 请求个数
-c: 并发量(模拟请求的客户端数量)
-t: 多少秒内进行并发
```
`Usage: ab [options] [http://]hostname[:port]/path`

##### 网络安全

###### nmap(Network Mapper)
* nmap实现网络发现的基本原理
nmap对目标主机进行端口扫描，找出监听的端口。构造并发送到目标主机探测包，
根据相应建立目标主机的nmap指纹,在指纹库中查找匹配,从而得出目标主机类型、
操作系统类型、版本以及运行服务等相关信息(不同厂家在编写自己的TCP/IP协议栈
有不同的解释，这些解释因具有独一无二的特性，故被称为"指纹")

有ping扫描
nmap -sP 192.168.1.1/24
无ping扫描
nmap -P0 192.168.1.1/24
系統版本探測
nmap -A 192.168.1.1/24


##### netcat(nc command)
> windows安装: 下载nmap后自带ncat.exe/从官网`https://eternallybored.org/misc/netcat/`安装
> netcat可用于端口扫描、端口重定向,启动端口监听器，它还可以用来打开远程连接和许多其他事情.此外，也可以使用它作为后门访问目标服务器.

* 展示详细信息,监听指定端口
nc -lvp port

* 访问nv开启的服务(端口扫描,port可以使用区间如500-505)
nc -zv ip port

* 连接服务器端口，连接成功后可以发送和接受数据
> nc ip port
> nc ip port < filename

eg:

```shell
λ ncat -lvp 8888
Ncat: Version 7.92 ( https://nmap.org/ncat )
Ncat: Listening on :::8888
Ncat: Listening on 0.0.0.0:8888
Ncat: Connection from ::1.
Ncat: Connection from ::1:7680.


λ ncat -zv [::1] 8888
Ncat: Version 7.92 ( https://nmap.org/ncat )
Ncat: Connected to ::1:8888.
Ncat: 0 bytes sent, 0 bytes received in 0.01 seconds.
```

##### telnet
> 远程登录的标准协议.是TCP/IP协议簇中的一员,为用户提供了在本地计算机上完成远程主机工作的能力
> windows使用telnet命令，需要在windows打开或关闭windows功能开启telnet客户端的功能

> 默认端口是23 
> telnet ip port
```shell
λ telnet 124.222.86.56 ctrl+]进入回显模式 实际上这里并没有用到telnet协议,此时还是tcp协议阶段
正在连接124.222.86.56...无法打开到主机的连接。 在端口 23: 连接失败
```
###### nslookup
* 正向解析: 通过域名查找ip
```
λ nslookup www.hdu.edu.cn
服务器:  192.168.1.1
Address:  192.168.1.1 ------DNS名字服务器信息

非权威应答:
名称:    www.split.hdu.edu.cn
Addresses:  2001:250:6402:106::102:34
          210.32.32.181
          210.32.32.182
Aliases:  www.hdu.edu.cn ------www.hdu.edu.cn的ip地址群
```
* 反向解析: 通过ip查找域名
PTR记录是邮件交换记录的一种，邮件交换记录中有A记录和PTR记录，A记录解析名字到地址，
而PTR记录解析地址到名字。通过对PRT记录的查询，达到反查的目的
`nslookup -qt=ptr yourIP`
```
C:\>nslookup -qt=ptr 74.125.128.106
Server:  zj-ns1.cableplus.com.cn
Address:  219.233.241.166                 -----DNS名字服务器信息

Non-authoritative answer:
106.128.125.74.in-addr.arpa     name = hg-in-f106.1e100.net

125.74.in-addr.arpa     nameserver = NS4.GOOGLE.COM
125.74.in-addr.arpa     nameserver = NS1.GOOGLE.COM
125.74.in-addr.arpa     nameserver = NS2.GOOGLE.COM
125.74.in-addr.arpa     nameserver = NS3.GOOGLE.COM    ---对应的域名
NS1.GOOGLE.COM  internet address = 216.239.32.10
NS2.GOOGLE.COM  internet address = 216.239.34.10
NS3.GOOGLE.COM  internet address = 216.239.36.10
NS4.GOOGLE.COM  internet address = 216.239.38.10
```




##### 查询ip信息
```shell
λ curl ip.gs
2408:8270:a52:81f0:21d0:ce5:fd1:841

F:\coding\Vue
λ curl ip.sb
2408:8270:a52:81f0:21d0:ce5:fd1:841

F:\coding\Vue
λ curl ipinfo.io
{
  "ip": "123.139.22.213",
  "city": "Xi’an",
  "region": "Shaanxi",
  "country": "CN",
  "loc": "34.2583,108.9286",
  "org": "AS4837 CHINA UNICOM China169 Backbone",
  "timezone": "Asia/Shanghai",
  "readme": "https://ipinfo.io/missingauth"
}

λ curl httpbin.org/get
{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.83.1",
    "X-Amzn-Trace-Id": "Root=1-630c901b-6f8704c06908c2984b5d054e"
  },
  "origin": "123.139.17.40",
  "url": "http://httpbin.org/get"
}
```






