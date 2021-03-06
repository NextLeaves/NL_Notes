title: 计算机底层知识
date: 2017-11-5 21:25:00
tag: Computer Lecture

---

* 进程与线程
* 上下文切换
* 系统调用
* Semaphore/Mutex
* 死锁
* 生产者消费者
* 进程间通信
* 逻辑地址、物理地址、虚拟内存
* 文件系统
* 实时与分时操作系统
* 编译器

<!--more-->

## 进程与线程 ##

* 进程：
	1. 进程拥有自己的地址空间
	2. 该进程的线程对于其他进程为不可见；进程之间通信需要通过进程通讯(Inter-Process Communication,IPC)
	3. 进程作为操作系统中具有资源和独立调度的基本单元
	4. 一个程序一般为一个进程，进程内的线程切换，大大节约资源，并且效率高；
		* 从一个进程对另个进程的线程切换，需要切换线程，开销比较多；
		* 线程与进程的相互结合更能够提高系统的运行效率
* 线程：
	1. 用户级线程(user level thread)
		* 工作都有应用程序完成，内核意识不到线程的存在
		* 好处：不需要进入内核空间，非常高效；但是并发效率不高
	2. 内核级线程(kernel level thread)
		* 有关线程的管理内容由内核来管理
		* 应用程序没有管理内核线程的能力，只有调用内核开放的API
		* 内核维护进程和线程，调度由内核的线程架构完成
		* 内核可以分配给不同的CPU，实现真正的并行计算
	3. 组合方式，实现多线程；即线程的创建完全在用户空间中完成，并且一个应用程序分配给内核处理
## 上下文切换(context switch) ##

* 上下文切换：是一种将CPU资源从一个进程分配给另一个进程的机制（对于单核单线程CPU）
* 先存储(内存空间地址的指针，当前指令的完成程度等)，再进行切换

## 系统调用(system call) ##

* 系统调用（System Call）：是一个程序向系统内核请求服务的方式。
* 硬件相关的服务（如：访问硬盘），或者创建新进程，调度其他进程
* 系统调用是程序与操作系统之间的重要接口

## Semophore/Mutex ##

* 用户创建多个线程和进程时，如果同时进行同一资源的访问，那么可能会导致读取的数据不一，或者读取错误；此时，要进行加锁的方式，控制核心区域（Criticle section）的访问权限

## 死锁 ##

* 死锁（dead lock）：两个或者多个线程、进程的相互阻塞，以致于一个都不能继续运行，因此也不能进行其他线程、进程。
	* 例子：A占有lockA资源，B占有lockB资源，A需要lockB，B需要lockA；则无法进行资源互换
* 产生死锁的原因：
	1. 资源不够
	2. 线程、进程推送的方式不对
	3. 资源分配的不合理
* 死锁的四个条件：
	1. Mutual Exclusion:Only one process use a resource at a time.
	2. Hold and Wait:Process holds resource while waiting another.
	3. No Preeemption:Can't take a resource away from a process.
	4. Circular Wait:The waiting processes from a circle.

## 生产者消费者 ##

* 生产者消费者是一种常见的通信模型：生产者和消费者通用一个数据通道，生产者将数据写入buffer，消费者读取buffer，确定是否为空或者溢出
* 通常需要对此内存资源进行Mutex锁
* 只有一个消费者和一个生产者的情况下，可以设计无锁队列(lockless queue),线程安全地读取数据

## 进程间通信 ##

* 需要实现**数据交换**和**数据同步**两大功能
* 两种实现通信方式：
	* 内存共享(shared memory)+semaphore
		* 不同的进程通过读写操作系统中特殊的共享内存进行数据交换
		* 不同进程间使用semaphore实现同步
	* 信息传递(message passing)
		* 进程在操作系统中注册一个port，监测有没有数据；其他进程直接对该port进行数据写入
		* 该通信方式更加接近于网络通信方式；实际上网络通信也是一种IPC，只是进程分布在不同的电脑上而已

## 逻辑地址、物理地址、虚拟内存 ##

* 逻辑地址：所谓逻辑地址指的是（编程时），编程所看到的连续的地址位置，假如一个int类型的数组，因为int一个单元所占的空间为4个字节，第二个元素的地址就在源地址加4,以此类推；并非真实的地址
* 物理地址：即真实的地址，在内存条中所在的地址，并不是连续的，只不过是操作系统的映射，将逻辑地址映射成连续地址，这样更符合人们的直观思维
* 虚拟内存：操作系统读取内存的速度比读取硬盘的速度快的多；但是内存价格也相对较高，不能大规模扩展。
	* 把不常用的数据存储到硬盘中，实现腾出更多的空间处理，需要处理的数据内容；当需要处理存储的数据时，再加载回内存继续处理
	* 唯一慢的可能就是内存条中没有的数据，就需要从硬盘读取，而且减慢系统效率；这就是为什么会有缓存的存在，加快计算机处理效率
* 现在操作系统中有一个专门**转译缓冲区(Translation Lookaside Buffer,TLB)**：处理逻辑地址快速转换为物理地址的机制
* 关于**内存**、**虚拟内存**相关的两个概念
	* Resident Set（常驻内存）
		* 当一个进程运行时，不会全部数据加载到内存，只会一部分，这一部分称为Resident Set
		* 其他的数据，存储在虚拟内存、交换区、硬盘文件系统
	* Thrashing
		* 当运行进程需要数据的时候，理想情况下，内存会逐步的加载预读取的数据；但是事实并非如此，若无法读取到数据，则会从虚拟内存和硬盘上进行数据读取，这个过程被称为**内存页面错误(page faults)**，当操作系统需要大量时间去处理内存页面错误时，称为thrashing

## 文件系统 ##

* UNIX风格的文件系统运用的是树形结构管理文件
	* 每个节点有多个指针，指向下一个结点或者文件的磁盘存储位置
	* 文件结点还包含文件的信息（metadata），包括文件的创建时间，修改时间，权限等
* 用户的权限通过
	* **访问控制表(Access Control List)**:以文件出发，每个用户对文件的访问权限内容
	* **能力表(Capabillity List)**：从用户角度触发，每个用户可以对什么文件进行操作
* UNIX的文件权限：
	* 读
	* 写
	* 执行
* 用户组分为：
	* 文件拥有者
	* 组
	* 所有用户
* 可以通过命令对权限进行分配

## 实时和分时操作系统 ##

* 实时操作系统(real-time system)：软件和程序需要严格的遵守deadline标准，超过时限的进程可能直接被终止；每次加锁都需要考虑（因为加锁存在等待时间）
* 分时操作系统(sharing time system)：多数采用分时操作系统；即多个用户、多个程序共享CPU资源，从形式上实现多任务；如果一个程序被锁住，可以分配更多的时间，继续执行

## 编译器 ##

* 是一种高级语言编译成二进制文件需要的工具，通过编译器(Compiler)实现
* 编译器需要处理的事情：
	* 词法分析(Lexical Analysis)
	* 解析(Parsing)
	* 过渡代码生成（Intermediate Code Generation），CIL（common intermediate language，通用中间语言）
* 编译器的好坏决定最终代码的执行效率