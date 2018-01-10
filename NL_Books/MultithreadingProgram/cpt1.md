title: Beginning Multithreading
date: 2018/1/7 15:21:54 
tag: csharp

---

* Theading Base
* create thread
* sleep in thread
* stop in thread
* break in thread
* wait in thread
* 线程优先级
* 前台线程和后台线程
* c#中的传参
* Monitor类锁定资源
* 处理异常

<!--more-->

# 线程基础 #

* **note：**
	* 线程会消耗大量的系统资源，所以在单处理器中使用大量的线程，会造成处理器忙于管理线程操作，而无法执行相应的程序
	* 正在执行中的程序实例可以成为一个进程，进程由一个或者多个线程组成；意味着在程序的执行过程中，始终有一个主线程在执行程序代码
* **线程的生命周期：**
	* 创建线程
	* 挂起线程
	* 线程等待
	* 终止线程
* **具体API：**
	* Start()
	* Sleep(),TimeSpan类
	* Join()
	* Abort()
		* 给线程注入ThreadAbortException，导致线程被终结；可能在任何适合摧毁应用程序，**另外并不能一定保证终止线程**
		* 目标线程可以通过处理异常，ResetAbort()来终止Abord的调用；**因此不推荐此方法**
		* **推荐方法：**提供一个**CanclelationToken方法**，取消线程的执行
	* ThreadState
		* Unstarted
		* Running
		* WaitSleepJoin
		* Stopped
		* 始终可以通过**Thread.CurrentThread静态属性**来获取当前Thread对象
	* Process.GetCurrentProcess().ProcessorAffinity = new IntPtr(1);
		* 实现在一个cpu上处理程序
		* Priority=ThreadPriority.Highest;
	* IsBackground
* **向线程传递参数：**
	1. 函数的参数为Object类型，在调用Start()的时候传入参数
	2. lambda表达式，嵌套函数
	3. 闭包的问题，如果引用同一个变量，那么可能造成输出相同的效果，即使不同时段改变变量值
* **C#的lock关键字：**
	* 竞争条件（race condition）：是多线程中最容易导致错误的产生的一个环节

## 使用Monitor类锁定资源 ##

* 常见的多线程错误：**死锁（dead lock）**
	* lock会遇到死锁的情况，使用**Monitor类**来处理
* **Monitor.TryEnter(Object lockObject,Timespan timespan);**
	* 使用if-else进行判断
	* 返回值true|false，如果超出时间，那么会跳出死锁，继续执行下面的语句
	* 若**造成死锁**，那么会返回值false，然后进行else语句的部分
* **lock(){}**其实是Monitor类的一个语法糖
	* try...catch
	* acquiredLock:bool
	* Monitor.Enter
	* Monitor.Exit

## 多线程中的异常处理 ##

* 多线程的异常处理，只能在线程中捕获，不能再主线程部分，辅助线程的异常
* 异常未被捕获，一般是导致应用程序无法崩溃
	* 但是在.NET1.0和1.1中，该行为是不一样的，未被捕获的异常不会强制应用程序关闭
	* 可以修改程序的配置文件

`

	<configuration>
		<runtime>
			<legacyUnhandledExceptionPolicy enabled="1" />
		</runtime>
	</configuration>