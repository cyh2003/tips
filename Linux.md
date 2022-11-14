指令技巧
--------

### man

1.  通过执行`man`命令可以查看`man`的用法，常见的用法有当因用户命令和`C`库函数重名而无法看到`C`库函数的用法时，设名称为`x`，则可以通过`man 2 x`来查看名为`x`的库函数的用法

### grep

1.  加`-E`选项后，可以在使用正则匹配时不用给括号转义

### gcc/g++

1.  加`-E`选项后仅执行到预处理，文件后缀`.i`

2.  加`-S`选项后仅执行到编译，文件后缀`.s`

3.  加`-c`选项后仅执行到汇编，文件后缀`.o`

### git/github

1.  当因为`token`的原因（一般存在于报错）无法`clone`时，可尝试设置一个在网站上设置一个具有权限的`token`，复制之，（对于`Windows`）然后在`Windows`凭据管理器上新建/修改一个普通凭据，注意密码应为`token`

2.  `git`工作流（`github`）一般分为`6`步，即`fork`（拷贝远程的原始代码并保存为一个自己的远程`repo`，可在`github`网站点击完成），`git clone`（如果是小改动的话可以直接在远程`repo`更改，此时不需要克隆远程`repo`到本地），`git add`（无需赘述），`git commit`（无需赘述），`git push`（把本地更改同步到自己的远程仓库），`pull request`（这里指的是维护者`pull`我自己的`request`，通过在自己的仓库点击`pull request`来发出请求，然后按提示操作）
    
3.  可以通过直接写一个`issue`提醒维护者自己更改

### screen

1.  `screen`是一种相比于`tmux`较为简单的终端复用器

### strace

1.  `strace`用于跟踪并输出程序执行时进行的系统调用

### time

1.  可使用`time`命令计算程序的用户态和内核态运行时间

### nslookup

1.  `nslookup`通过访问`DNS`服务器查询域名对应的`IP`地址

### telnet

1.  `telnet`命令用于向某一地址+端口请求连接并可以发送请求，可用于网络测试

### pandoc

1. `pandoc`命令用于文件格式转换

### 网络资源下载

1.  `wget`命令通过`URL`下载各种类型的网络资源

2.  `lux`命令可以从网页中解析出可下载的视频资源，由用户挑选并下载

### 重定向

1.  使用重定向输入输出时可能会遇到`\r\n`和`\n`的问题，利用`vscode`右下角行尾序列，更改即可

基本概念
--------

### 进程管理

1.  挂起，一般通过按`ctrl+z`实现，效果为暂停执行（前台或后台程序均可以），但可用`fg`或`bg`恢复执行

2.  后台运行，一般通过在命令行末尾加"&"符号实现，也可以通过挂起+后台恢复间接实现，效果为以不占用终端的方式运行

3.  由于后台运行不能让`shell`以阻塞方式等待，所以不能直接用`waitpid`的方式等待，而是要先使用信号通知`shell`某个子进程的结束，再进行`waitpid`

4.  单纯用户态和内核态之间的切换不一定涉及上下文切换，所做的工作可能只是将寄存器保存在内核栈中以及其他关于状态（用户态/内核态）、程序计数器、栈指针的调整

5.  可重入=线程安全+信号中断安全

6.  多级页表是一种链表+向量的数据结构，借此实现高速的访问和内存空间的节省

### ECF

1.  `Exception`分为`Interrupt`（`async`）、`Trap`（`sync`）、`Fault`（`sync`）、`Abort`（`sync`）

2.  `Signal`是软件层级的`ECF`，位于软件层面，用于向进程发送通知

3.  信号处理程序中不能使用`printf`，原因是`printf`在更改缓冲区时会加锁，若主程序调用`printf`时，控制权离开并返回主程序，且返回主程序时发现有`pending`且非`blocked`的信号，进入信号处理程序时也调用`printf`，则信号程序中的`printf`由于主程序的`printf`的锁不得不等待，又由于主程序和信号处理程序处在同一个进程/线程中，因此主程序`printf`的锁总是无法解除，从而导致死锁

4.  每个线程有自己独享的信号处理

### 并发与并行

1.  在多线程中经常有加锁与解锁操作来保证二者之间操作的原子性，由于对于同一个锁，在任一时刻`P`操作数量多于`V`操作数量，不会有连续的两个`P`操作，由数学归纳法可知两种一定为形如`PVPVPVPV···`的序列

2.  由于多线程、`cache`机制与多核同时出现会造成`cache`不一致的问题，所以需要缓存一致性协议，但是要做到这一点较为复杂，这也是限制核心数量的原因之一

WSL2
----

### 网络

1.  宿主机可以用`127.0.0.1`访问`WSL2`，反之则不行

2.  当代理软件（`Clash`）位于`Windows`上时，`Windows`配置代理仅需要`set
    http(s)_proxy="127.0.0.1:7890"`，而`WSL2`在使用`export
    http(s)_proxy="宿主机IP:7890"`之前，要先用`cat /etc/resolv.conf |
    grep nameserver | awk ' print $2 '`获取宿主机`IP`（此外，用`hostname
    -I | awk 'print $1'`获取`WSL2`自身`IP`）

### 安全

1.  从宿主机复制而来的文件有时无法作为重定向输入输出文件被使用