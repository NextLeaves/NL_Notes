title: Linux
date: 2017-12-10 8:00:00
tag: linux

---

* Apache搭建

<!--more-->

# Linux服务器 #

## Apache的搭建 ##

* Prefork MPM：应用程序生成多个子进程，一个进程维护一个线程，一个线程用于连接，实现监听；
	* 优点：效率高
	* 缺点：占用内存过大
* Worker MPM：应用程序生成多个子进程，但一个进程维护多个线程，多个线程用于多个连接，实现监听；
	* 优点：占用内存小；利于高并发下的处理
	* 缺点：效率相比下较低；一个线程崩溃，会导致该进程崩溃
* wget ：从网站下载资源
* yum install ：安装centOS为代表的固有的资源包
* rpm -ivh :安装rpm包
* tar xzf :解压文件
* Tar命令参数：
	* -c:建立一个压缩文件的参数指令
	* -x:解开一个压缩文件的参数指令
	* -t:查看tar file里面的文件
	* -z:是否同时具有gzip的属性，是否需要用gzip压缩
	* -j:是否具有bzip2的熟悉，是否需要bzip2压缩
	* -v：压缩的过程中显示文件，这个常用，但不建议用在背景执行过程
	* -f:使用档名，请注意，在f之后要立即接档名！不要再加参数
* 安装所需要的指令:
	1. wget http://archive.apache.org/dist/httpd/httpd-2.2.27.tar.gz
	2. yum install apr apr-util
	3. yum install apr-devel apr-util-devel
	4. tar httpd-2.2.27.tar.gz
	5. ./configure --prefix=/usr/local/apache
	6. make
	7. make install

## vi 编辑器 ##

