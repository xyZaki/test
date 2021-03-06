----
多人同时使用一台主机的解决方案：

1. 主机配置多显卡、多接口，以让每个用户的键盘、鼠标、显示器连接，然后系统为每个用户设置账户

2. 使用“瘦客户机“，NComputing 是瘦客户机的代表，瘦客户机是具备USB、网卡、显卡等类似主机接口的单片机，可以连接主机。主机同样为每个瘦客户机设置账户，主机通过路由器连接网络，
所有瘦客户机连接该路由器，每个的瘦客户机连接各自的显示器、键盘和鼠标等设备，瘦客户机通过输入主机为其配置的用户名和密码就可以登入共享主机资源（含网络）

使用瘦客户机主要是为了节约独立主机的成本，其次可以统一管理计算机资源，瘦客户机的方案也是计算机安全应用策略，集中管理也使得易维护和升级等等。

瘦客户机不适用于高性能设计开发等工作(除非主机及其强悍)，可以运行一般软件，如浏览器、聊天工具等轻应用

----



# 1 Unix Basic

## 1.1 Introduction 

## 1.2 Architecture

## 1.3 Standardization and Implementations

# 2 I/O

## 2.1 Standard I/O

## 2.2 File I/O

## 2.3 Terminal I/O

# 3 Process

## 3.1 Process Environment

## 3.2 Process Relationships

### 3.2.1 Daemon Processes

## 3.3 Process Control

## 3.4 Inter process Communication 

### 3.4.1 Signals

# 4 Threads

## 4.1 Threads Control

# 5 Network IPC : Socket

# 6 File system


Linux  操作系统的 实时性 分析

首先UNIX和类Unix操作系统 都是通用操作系统

通用操作系统是 批处理、分时、实时操作系统的集合体，主要性能表现在多用户+分时上

实时意味着需要 **极快的响应速度** ， 要在极短的时间内，无视任何情况，去响应请求。

专门的实时系统 在响应速度上比通用操作系统快，这仅在极致需求时才具有对比性，可能在90%~99%的情况几乎是没有差别的，甚至通用操作系统会更优秀，

因为通用操作系统 一般利用带有MMU的更快速、更高性能的CPU，足够快的分时轮询 足够小的时间片 比低速CPU的实时处理还要好。

所以实时性 除了处理方式 也要看硬件资源情况， 可以认为在足够好的硬件资源上使用通用操作系统，再加上多进程多线程的合理设计 其性能要优于 实时操作系统

https://blog.csdn.net/luckydarcy/article/details/53572620



















### ioctl(int fd, int request, ...)

\#include <sys/ioctl.h>

ioctl()是IO操作的万能函数，《UNIX高级环境编程》将其比喻IO操作的杂物箱，更多用于终端IO操作.

参数:
- fd 必须是一个打开的文件描述符
- request 请求码（一般表示与设备相关的请求码）
- ... 多参，但一般是一个结构体或者一个char *

第三个参数是为第二个参数准备的，因为C语言没有重载功能，所以就利用fun(ID，...)这样的函数形式来
提供任意的功能. 所以第三个参数是否存在，存在几个，是入参还是出参，完全由第二个参数决定

如果ioctl用于操作终端IO，则还需另外头文件 <termios.h>


在BSP中的设备驱动环节，是可以对ioctl进行注册的，于是在用户层就可以使用对应的命令码和参数来使用ioctl().








# 1 Unix Basic

# 2 Process 

## 2.1 Process Environment

## 2.2 Process Control

## 2.3 Process Relationships

## 2.4 Deamon Processes

## 2.5 Interprocess communication

### 2.5.1 Signals 

Signal is a 'soft interrupt', which used to handle asynchronous events.

#### 2.5.1.2 Send Signals 

- 终端发送
	- 快捷键：$stty -a 查看发送终端信号快捷键集合
	- 命令行：'kill [-s SigName/-n SigNum] PID/GID'. or 'Kill -L/-l' (显示系统SigName列表)

- 函数发送
	- 给指定进程发送指定信号：kill(PID, SIG)，当PID大于0时表示给指定进程发送信号，等于0时表示给当前进程发送信号，等于-1时表示给所有进程发送信号，小于-1时表示给进程组发送信号
	- 给当前进程发送指定信号：raise(SIG)
	- 给当前进程发送终止信号：abort();
	- 给当前进程发送闹钟信号：alarm(second); 超时后将发送SIGALRM信号，默认动作是杀死进程，重复调用该函数将重载设定时间并返回上次剩余时间，当参数为0时表示取消之前的闹钟，该函数一定要放置在捕捉函数之后
	- 给指定进程发送并附加值：sigqueue(PID, SIG, VAL); 使用sigaction()获取VAL值(Union).

#### 2.5.1.3 Processing Signals

- 忽略信号：被忽略的信号默认动作是忽略/终止进程
- 捕捉信号：
	- signal(SIG, CALLBACK); 同一个信号注册多次则最后一个回调函数有效，该函数适合单信号处理环境
	- sigaction(SIG, ACT, OLDACT); 


## 3 I/O

	I/O矩阵
			
			阻塞								不阻塞
	同步	1. R/W								2. R/W+O_NONBLOCK
	异步	3. 多路复用(可配置为不阻塞)			4. AIO

	1. 同步阻塞IO
		用户空间的应用程序执行一个系统调用，这会导致应用程序阻塞。这意味着应用程序会一直阻塞，直到系统调用完成为止（数据传输完成或发生错误）
		。调用应用程序处于一种不再消费 CPU 而只是简单等待响应的状态，因此从处理的角度来看，这是非常有效的

		以read举例:
		阻塞-》切换到内核态-》读完成-》切换到用户态（数据移动到用户缓冲区）

		使同步阻塞IO并发的方式是利用进程和线程，所有阻塞IO各一个进程/线程，并且如果使用线程则需做好数据同步.

		同步IO不一定阻塞，阻塞的也不一定是同步IO，同步IO表示 用户触发IO操作之后 要去阻塞等待或轮询IO结果，有了结果再同步执行其它指令，这就是同步的意思

		异步IO不一定不阻塞，不阻塞的也不一定是异步IO，异步IO表示 用户触发IO操作之后 不用阻塞等待或轮询IO结果 这些都交给内核来完成 用户可以继续执行自己的指令，
		等IO有结果之后内核会通知用户，这可以通过信号或回调函数来完成IO触发的既定任务。

		异步IO好比到某机构登记注册，然后等电话通知再去取东西一样，这段时间你什么都行。而同步就是你要一直排队等待看看结果出来没有。

	2. 同步不阻塞IO
		这种情况下表示IO不会立即完成，为检测是否完成要一直轮询，并适当的加入延时，当没有获取到数据时errno会被设置成相应错误码，如EAGAIN(再来一次)或其它错误，所以可以通过函数返回值和错误码确定是否继续下一次循环。

	3. 异步阻塞IO - 多路复用 
		这种模式相比“同步阻塞IO”，可以同时阻塞多个IO，并且阻塞的时长可以配置。
		调用select、poll、epoll（linux）可以实现这个功能。

		**当用这种模式只阻塞一个IO时，与同步IO的区别就只有 可以设置阻塞时长。**

		实际上 同步阻塞IO 可以利用信号signal达到超时的设置。

		另外，如果使用了 IO多路复用，那么I/O本身就不应该设置为阻塞了，比如使用了select/poll/epoll() ，read/write就不要再设置成阻塞了
		，显然 如果这样做 select、poll、epoll就失去或损坏多路复用的功能了。

		特别的，如果多路复用函数解除阻塞后，要对该文件描述符做耗时操作，那么仍然需要开启一个进程或线程，以便快速进入下一个循环

		Tips：多路复用是对事件阻塞，而不是对IO阻塞.

		多路复用也不是完全的异步， 只能说在事件通知上是异步的，而实际读写函数还是同步完成的，因为仍然需要阻塞或轮询

	4. 异步非阻塞IO - AIO
		这种模式利用了处理速度与I/O速度之间的差异，处理器速度被认为是极快的，I/O速度被认为是慢操作。

		在运行IO代码时，只对该IO进行注册登记，而不阻塞等待它的返回、也不对其轮询，而是继续下面的其它代码执行，对该IO的监控交给内核完成，
		当IO响应达到时，会产生一个信号或或执行一个基于线程的回调函数来完成这次 I/O 处理过程

		这类似于为未来要发生的事件准备好资源，当事件发生时自动触发和利用该资源，整个过程不用用户监控，用户要做的就是注册登记并准备资源。


任何的“同步”字样，可以想象你和别人跑步，跑的很快，然后到一个位置X停下来等待其它的人，其它的人中的某些到了X也停下来等待剩余的人，直到所有的人到达了X，然后继续往后走，这就是同步
。

异步可以描述为，你在开始跑之前就告诉其它人我不等人，但是到了X给我打电话我去接你们



### 2.1 阻塞式I/O
### 2.2 非阻塞式I/O
### 2.3 多路复用
### 2.4 异步I/O
	异步I/O是不阻塞的，设定好事件和事件触发时调用的函数，然后就可以做其它的事情了。当事件发生时去执行回调函数。

	所以注册的概念对异步I/O非常重要，你只管登记，然后就可以做其他的事，事件发生了就通知你回去做登记的事，于是你不必阻塞等待，
	也不必循环轮询。

	类似于注册了一个信号，然后按序继续执行其它代码，当信号到达时直接转去执行信号注册的函数

## 3 Asyn I/O (AIO)


## 3.1 I/O Multiplexing 

	背景：
	对于文件描述符的状态（如socket的文件描述符、open的文件描述符等等，系统并不主动告诉我们任何信息，文件描述符关联的文件发生的任何变化都需要我们主动去查询，

	有“阻塞+多进程/多线程”、“非阻塞查询"和”异步I/O“可选，但这些方法都不理想

	阻塞式IO ：很明显，如果在一个进程/线程当中，被阻塞的下一条语句将一直无法执行，直到被阻塞函数不阻塞，这种方式适合非并发态的情况。但是大多数编程都是需要并发的，这也是操作系统存在的意义之一。所以就要为每一个IO开启一个进程和线程，进程资源很宝贵，而使用线程也需要解决锁和同步等问题，进程或线程+阻塞模式是可行的，这要评估项目实际的使用场景，如果只有很少量的IO是没有问题的，如果有类似TCP/IP-HTTP高并发服务器这种的，那很快就会崩溃的。因为进程和线程都不能无限制的增加，进程线程切换开销也很大。当进程、线程过多时，CPU大部分时间都用来切换了。

	非阻塞模式，那就需要循环检查，也就是轮询，对于一个进程无限轮询，可以看到当前的CPU核接近100%，这与RTOS中的一个死循环任务中不加延时或调度产生的结果是一样的，也就是其它进程都无法执行了，CPU（如单核）无法给其它进程分配时间片，那就需要加延时，比如sleep睡眠，但睡眠设置多久呢？如果设置长了，那么就不能及时检查并处理已经发生事件的IO，如果事件发生的过快，很可能在被检查之前覆盖掉了上次的事件，导致漏检，更慢的周期还可能导致不及时处理产生的其它问题。如果设置的时间过短就产生其它进程无法分配时间片的问题。所以延时的时间很难把控。此外，频繁的调用I/O处理函数也是错误的表现，理论上任何函数都不应该非常频繁的被调用，特别是无意义的调用。 


	多路复用，相当于在一个进程/线程下达到阻塞多个I/O的目的，并且，这里的阻塞是有时间配置的，可以为0不阻塞，可以一直阻塞，也可以设定固定时长。

	I/O多路复用是单进程下的 信号、轮询等多种方式的组合体.

	I/O多路复用，就是在单进程下使用内核机制检查描述符列表中的各个成员的状态，这相当于对每个文件描述符开一个进程，然后阻塞检查。


	I/O多路复用 - 构造一个描述符列表，调用特定函数查询这些列表，直到这些描述符中有一个已经准备好进行I/O时才返回

	select, pselect, 和 poll 是posix标准内的，eselect在Linux中存在

- select

	int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);

	timeout - 在描述符列表中没有可读、可写、异常时，函数阻塞的时长，或描述为“没有捕捉到信号”等待的时长。 
			  NULL为永久等待，即阻塞模式
			  timeout->tv_sec==0 && timeout->tvu_sec=0 不等待，轮询一次所有的文件描述符就返回，即非阻塞模式
			  对上述成员赋值即表示等待固定时长，如果超时后返回值为0表示没有可用fd

			  文件描述符是否是阻塞的，不会影响select，它的超时只由timeout决定

	readfds/writefds/exceptfds 分别为读、写、异常文件描述符集的指针，POSIX对于fd_set类型的实现是可选择的，因为实际应用中并不会
	直接使用该结构体，而是调用如下函数宏：

	int  FD_ISSET(int fd, fd_set *set); //判断文件描述符是否属于该集合
	void FD_SET(int fd, fd_set *set);	//添加文件描述符
	void FD_CLR(int fd, fd_set *set);   //删除文件描述符
	void FD_ZERO(fd_set *set);			//删除所有文件描述符

	nfds - 最大文件描述符编号+1, 因为文件描述符编号从0开始。该参数是一个范围限制，如果设置为N，则上述三个集合的文件描述符都必须小于N才能被检测，
	非常保险的做法是设置FD_SIZE，该宏表示支持的最大文件描述符值。通常通过打开的文件描述符做比较，来确定哪一个才是最大的。

	返回值：

	-1 出错，返回0（超时没有fd准备好），>0表示已经准备好了的文件描述符的数量，如果同一个描述符同时准备好读和写则返回值对其计数两次

	准备好的含义是，select返回之后（大于0），可以read/write，且不会阻塞。另外对于普通文件，read、write和异常文件描述符总是准备好的


	注意：select 遇到文件尾端时仍然认为是可读的，直到读完该尾端。select返回后，调用read返回0，因为到达了尾端

- pselect

	int pselect(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, const struct timespec *timeout, const sigset_t *sigmask);

	pselect 将超时类型改为timspec，能精确到纳秒， 并增加了sigset_t类型，即信号量屏蔽集，
			当sigmask为NULL时，pselect与select表现相同
			信号屏蔽字，即信号屏蔽集，被屏蔽的信号不能发送给pselect所在进程（实际上是阻塞信号，因为恢复后信号仍然可用，被阻塞的信号仍然被发送），返回时恢复屏蔽信号。

- poll 
	
	poll支持任何类型的文件描述符（而select只支持读、写和异常的文件描述符），

	int poll(struct pollfd *fds, nfds_t nfds, int timeout);

	fds应该是一个数组，pollfd结构体如下：

	struct pollfd {
		int fd;		//要检测的文件描述符，当设置小于0时，poll将忽略
		short event;	//告诉用户感兴趣的事件
		short revents; //内核返回给用户当前发生的事件
	};

   nfds表示数组元素个数

   events可选值为POLLIN/.....以及它们的或值
   reevents另外可返回POLLERR/POLLHUP/POLLNVAL异常，这三个值events不能设置

   POLLUP表示挂断，此时只能读文件描述符，而不能写


   timeout = -1 表示永久等待，当有文件描述符准备好时返回一个+值，如果收到信号中断则返回-1, errno = EINTR。

   = 0 不等待

   >0 毫秒值，如果超时未准备好则poll返回0

   与select一样，文件描述符是否阻塞不影响poll，只由timeout决定




TIPs : poll与inotify函数组合
poll 使用

	struct			pollfd fds = { .fd = queue_fd, .events = POLLIN };

关联inotify_init（）返回的fd，也就是队列文件描述符，事件就是可读

如果队列可读，就表示queue_fd中的watch_fd可读了

同时发生在watch_fd的事件可能不止一个，所以使用read来循环读，一直读到没有。

然后继续poll循环，观察下一个watch_fd，


	







---

# <span id = "Appendix-A"> Appendix-A：UNIX Standards and Implementation </span>

## Posix - ISO/IEC 9945

## ISO/IEC 9945-1 : BASE DEFINITIONS
## ISO/IEC 9945-2 : SYSTEM INTERFACES
## ISO/IEC 9945-3 : SHELL AND ULTILITIES 
## ISO/IEC 9945-4 : RATIONALE 

---

## 散装知识点

### setpgid(pid_t pid, pid_t pgid)

将pid所在的组ID设置为pgid

如果pid=0 ，表示选择该函数所在的进程组
如果pgid=0，表示用该函数所在的进程的PID作为进程组ID 

所以，setpgid(0, 0); 的含义为：将当前进程的进程组ID设置为当前进程的PID

注意：因为进程仅能修改进程本身组ID或其子进程组ID，所以参数pid必须其函数所在进程的PID或者它的子进程PID。


------------

以下不是Posix接口，是Linux-specific



# 1 Inotify

The inotify API provides a mechanism for monitoring and returning filesystem (file and directory) events.

## 1.1 API

`#include <sys/inotify.h>`

### 1.1.1 int inotify_init(void) & int inotify_init1(int flags)

Initializes a new inotify instance and returns a file descriptor associated with a new inotify event queue, `inotify_init()` is same as `inotify_init1()` when param flags = 0

#### 1.1.1.1 Parameters 

IN_NONBLOCK/IN_CLOEXEC can be set for *flags* to perform block and close-on-exec behavior (for inotify_init1()). 

#### 1.1.1.2 Return value

On success, return a new file descriptor. On error, -1 is returned, and errno is set to indicate the error :

- EINVAL - (inotify_init1()) An invalid value was specified in flags.
- EMFILE - The user limit on the total number of inotify instances has been reached.
- EMFILE - The per-process limit on the number of open file descriptors has been reached.
- ENFILE - The system-wide limit on the total number of open files has been reached.
- ENOMEM - Insufficient kernel memory is available.

#### 1.1.1.3 Note

Minimum kernel/glibc version requirements is V2.3.13/V2.4 for `inotify_init()` and V2.6.27/V2.9 for `linotify_init1()`.

### 1.1.2 int inotify_add_watch(int fd, const char \*pathname, uint32_t mask)

Add a new watch or modifies an existing watch for the file or directory whose location is specified in param *pathname* to the monitor queue.

#### 1.1.2.1 Parameters 

*fd* is referring to which has been initialized instance. The events to be monitored for *pathname* are specified in the *mask* bit-mask argument.

#### 1.1.2.2 Return value 

On success, return a nonnegative watch descriptor.  On error, -1 is returned and errno is set appropriately :

- EACCES - Read access to the given file is not permitted.
- EBADF  - The given file descriptor is not valid.
- EFAULT - *pathname* points outside of the process's accessible address space.
- EINVAL - The given event mask contains no valid events; or fd is not an inotify file descriptor.
- ENAMETOOLONG - *pathname* is too long.
- ENOENT - A directory component in *pathname* does not exist or is a dangling symbolic link.
- ENOMEM - Insufficient kernel memory was available.
- ENOSPC - The user limit on the total number of inotify watches was reached or the kernel failed to allocate a needed resource.

#### 1.1.2.3 read() 

`ssize_t read(int fd, void *buf, size_t count)` SHALL called to featch filesystem events from structure `struct inotify_event{}` :  

```
struct inotify_event{  
	int      wd;       /* Watch descriptor */ 
	uint32_t mask;     /* Bits, mask describing event */  
	uint32_t cookie;   /* Unique cookie associating related events only for IN_MOVED_FROM and IN_MOVED_TO */  
	uint32_t len;      /* Size of name field (including '\0') */  
	char     name[];   /* Optional terminated with '\0', exist only when file events watched and reaturn, and may have multi '\0' endings to alignment content */  
}  
```

Object		| Macro				| Events description						| Note 
:-:			| :-:				| :-:										| :-:
file		| IN_ACCESS			| access									| +
file		| IN_CLOSE_WRITE	| open for writing was closed				| +
file		| IN_MODIFY			| modify									| +
file		| IN_MOVED_FROM		| rename an old name						| +
file		| IN_MOVED_TO		| rename a new name							| +
file		| IN_MOVE			| = IN_MOVED_FROM &#124 IN_MOVED_TO			| +
file & dir	| IN_CREATE			| create									| +
file & dir  | IN_DELETE			| delete									| + 
file & dir  | IN_OPEN			| open										| *
file & dir	| IN_CLOSE_NOWRITE	| opened for non-writing was closed			| * 
file & dir  | IN_CLOSE			| = IN_CLOSE_WRITE &#124 IN_CLOSE_NOWRITE	| *
file & dir	| IN_ATTRIB			| attributes were changed					| *
file & dir  | IN_MOVE_SELF		| -											| * 
file & dir  | IN_DELETE_SELF	| -											| *
\-			| IN_ALL_EVENTS		| monitor all events						| for add
\-			| IN_EXCL_UNLINK	| except unlink events (since Linux 2.6.36) | for add
\-			| IN_DONT_FOLLOW	| - (since Linux 2.6.15)					| for add
\-			| IN_MASK_ADD		| append instead cover for the same pathname| for add
\-			| IN_ONESHOT		| remove it once occure (since Linux 2.6.16)| for add
\-			| IN_ONLYDIR		| monitor only if it's directory			| for add
\-			| IN_IGNORED		| objcet which monitored has been removed	| for read
\-			| IN_ISDIR			| about directory							| for read
\-			| IN_Q_OVERFLOW		| wd == -1									| for read
\-			| IN_UNMOUNT		| filesystem unmount						| for read  

<br><center> <font color=gray> mask of structure inotify_event and for inotify_add_watch() param </font> </center><br>

Tips : 
1. or_ed can be used for the macro to create personnalized events.
2. \* refer that both for the directory itself and for objects inside the directory and + refer that only for objects inside the directory.

#### 1.1.2.4 Note 

The param *buf* of read() should be set bigger enough for the reason of the member of structure inotify_event <u>name</u>, otherwise 0 will be return for the kernel version 2.6.21and EINVAL will be set for errno for the kernel version 2.6.21. 

### 1.1.3 inotify_rm_watch(int fd, int wd)

Remove an existing watch (*wd*) from an inotify instance which associated with the file descriptor *fd*.

#### 1.1.3.1 Parameters

-

#### 1.1.3.2 Return value 

On success, inotify_rm_watch() returns zero.  On error, -1 is returned and errno is set to indicate the cause of the error:

- EBADF - fd is not a valid file descriptor.
- EINVAL - The watch descriptor wd is not valid; or fd is not an inotify file descriptor.

#### 1.1.3.3 Note

Removing a watch causes an IN_IGNORED event to be generated for this watch descriptor.

### 1.1.4 Config 

- /proc/sys/fs/inotify/max_queued_events, config the max queue events
- /proc/sys/fs/inotify/max_user_instances, config the max user instance
- /proc/sys/fs/inotify/max_user_watches, config the max user watch

### 1.1.5 Bug & Note

- **Inotify is based on inode**, so *pathname* movement does not affect any changes 
- Inotify do not report events that trigs by mmap()/msync()/munmap() 
- Release inotify resource by close(inotify_fd) instead of close(wd) 
- The 'watch filedescriptor' isn't a file system object or a file descriptor, it's same like an ID which refer to a *pathname* resource, and only used by 'inotify_rm_watch()'.

### 1.1.6 Code example

```
#include <errno.h>
#include <poll.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/inotify.h>
#include <unistd.h>

/* Read all available inotify events from the file descriptor 'fd'.
   wd is the table of watch descriptors for the directories in argv.
   argc is the length of wd and argv.
   argv is the list of watched directories.
   Entry 0 of wd and argv is unused. */

static void handle_events(int fd, int *wd, int argc, char* argv[])
{
	/* Some systems cannot read integer variables if they are not
	   properly aligned. On other systems, incorrect alignment may
	   decrease performance. Hence, the buffer used for reading from
	   the inotify file descriptor should have the same alignment as
	   struct inotify_event. */

	char buf[4096]
		__attribute__ ((aligned(__alignof__(struct inotify_event))));
	const struct inotify_event *event;
	int i;
	ssize_t len;
	char *ptr;

	/* Loop while events can be read from inotify file descriptor. */

	for (;;) {

		/* Read some events. */

		len = read(fd, buf, sizeof buf);
		if (len == -1 && errno != EAGAIN) {
			perror("read");
			exit(EXIT_FAILURE);

		}

		/* If the nonblocking read() found no events to read, then
		   it returns -1 with errno set to EAGAIN. In that case,
		   we exit the loop. */

		if (len <= 0)
			break;

		/* Loop over all events in the buffer */

		for (ptr = buf; ptr < buf + len;
				ptr += sizeof(struct inotify_event) + event->len) {

			event = (const struct inotify_event *) ptr;

			/* Print event type */

			if (event->mask & IN_OPEN)
				printf("IN_OPEN: ");
			if (event->mask & IN_CLOSE_NOWRITE)
				printf("IN_CLOSE_NOWRITE: ");
			if (event->mask & IN_CLOSE_WRITE)
				printf("IN_CLOSE_WRITE: ");

			/* Print the name of the watched directory */

			for (i = 1; i < argc; ++i) {
				if (wd[i] == event->wd) {
					printf("%s/", argv[i]);
					break;
				}
			}

			/* Print the name of the file */

			if (event->len)
				printf("%s", event->name);

			/* Print type of filesystem object */

			if (event->mask & IN_ISDIR)
				printf(" [directory]\n");
			else
				printf(" [file]\n");
		}
	}
}

int main(int argc, char* argv[])
{
	char buf;
	int fd, i, poll_num;
	int *wd;
	nfds_t nfds;
	struct pollfd fds[2];

	if (argc < 2) {
		printf("Usage: %s PATH [PATH ...]\n", argv[0]);
		exit(EXIT_FAILURE);
	}

	printf("Press ENTER key to terminate.\n");

	/* Create the file descriptor for accessing the inotify API */

	fd = inotify_init1(IN_NONBLOCK);
	if (fd == -1) {
		perror("inotify_init1");
		exit(EXIT_FAILURE);
	}

	/* Allocate memory for watch descriptors */

	wd = calloc(argc, sizeof(int));
	if (wd == NULL) {
		perror("calloc");
		exit(EXIT_FAILURE);
	}

	/* Mark directories for events

	   - file was opened
	   - file was closed */

	for (i = 1; i < argc; i++) {
		wd[i] = inotify_add_watch(fd, argv[i],
				IN_OPEN | IN_CLOSE);
		if (wd[i] == -1) {
			fprintf(stderr, "Cannot watch '%s'\n", argv[i]);
			perror("inotify_add_watch");
			exit(EXIT_FAILURE);
		}
	}

	/* Prepare for polling */

	nfds = 2;

	/* Console input */

	fds[0].fd = STDIN_FILENO;
	fds[0].events = POLLIN;

	/* Inotify input */

	fds[1].fd = fd;
	fds[1].events = POLLIN;

	/* Wait for events and/or terminal input */

	printf("Listening for events.\n");
	while (1) {
		poll_num = poll(fds, nfds, -1);
		if (poll_num == -1) {
			if (errno == EINTR)
				continue;
			perror("poll");
			exit(EXIT_FAILURE);
		}

		if (poll_num > 0) {

			if (fds[0].revents & POLLIN) {

				/* Console input is available. Empty stdin and quit */

				while (read(STDIN_FILENO, &buf, 1) > 0 && buf != '\n')
					continue;
				break;
			}

			if (fds[1].revents & POLLIN) {

				/* Inotify events are available */

				handle_events(fd, wd, argc, argv);
			}
		}
	}

	printf("Listening for events stopped.\n");

	/* Close inotify file descriptor */

	close(fd);

	free(wd);
	exit(EXIT_SUCCESS);
}
```

Tips : function `select()`, `poll()` and `epoll()` can be used for inotify.





-------------


多线程编程与（动态）内存清零的重要性！！

在socket编程时发现使用多线程时，上次访问的数据总是在下次出现，这个数据是包含在函数的局部变量中的，只是是动态

不管是动态还是静态，函数结束总是释放的，况且自己也手动释放了

没错，是释放了

但是多线程访问时，有可能就是访问那个空间，也就是下次这个函数还开辟一样的动态空间，而每次都没有给这个空间清空

所以保留了上次操作的数据

引申一下， 一个内存被使用，作用域之外被释放，虽然释放了，但内容还存在那里，如果下次开辟空间刚好撞上了这个空间，那就
有很大的几率出错（比如这个空间没有被合理的覆盖）

所以，无论怎么开辟的，清空一下总是没错的！！！

