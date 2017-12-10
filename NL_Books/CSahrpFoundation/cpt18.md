title: File
date: 2017-10-31 7:45:00
tag: CSharp

---

* This chapter is talking about how to save information and open files.
	* File类、Directory类
	* .NET如果使用**流类**访问文件
	* 如何读写文件
	* 如何读写压缩数据
	* 如果**serialize And deseriialize**文件
	* 怎么监控文件修改及目录的改变

<!--more-->

## 用于输入输出的类 ##

1. 名称空间:**System.IO;**
2. 用于访问文件系统的类：
	1. **File：**静态实用类；用于移动、复制、删除文件
	2. **Directory：**静态实用类；用于移动、复制、删除目录
	3. **FileInfo：**表示磁盘上的物理文件，包含处理此文件的方法
	4. **DirectoryInfo：**表示磁盘上的物理目录，包含处理此文件的方法
	5. **Path：**实用类，用于处理路径名称
	6. **FileSystemInfo：***FileInfo和DirectoryInfo的基类，可以利用多态性，同时处理文件和目录*
	7. **FileSystemWatcher：***最复杂的类。用于监控文件和目录，提供发生变化时捕获事件*
3. **Using System.IO.Compression;**
	1. **DeflateStream：**使用Delfate算法来实现压缩
	2. **GZipStream：**使用GZIP(GNU ZIP)算法来实现压缩
4. 路径名和相对路径：
	* 绝对路径、相对路径

## 流 ##

1. Stream是序列化设备的抽象表示，序列化设备可以按照线性方式来存储数据，并且可以按照相同的方式进行读取，一次只读取一个字节
	1. 网路通道、磁盘文件、内容位置或者其他以线性方式存储的序列化的对象
	2. 使用文件流可以忽略每种设备的物理机制，不必担心刺头的位置或者内存分配位置
2. 把设备编程抽象的，可以隐藏流的底层目标和源，允许编程更加通用的例程
3. 流类：
	1. **FileStream：**可读可写文件
	2. **StreamReader：**从流中读取字符数据
	3. **StreamWriter：**向流中写入字符数据
		1. FileAccess:表示流的处理权限
		2. FileMode：用于写还是读
		3. Truncate：用于删除文本内容，保留创建日期，进行修改内容
4. File和FileInfo类更易于创建FileStream对象；如OpenRead(),OpenWrite();
5. Decoder类、Encoder类、Encoding类、DateTime类
6. StreamReader类的最简单的读取方法为函数：Read()，如果到结尾则返回值：-1；

## 异步文件访问 ##

* StreamReader.ReadLineAsync()方法，更多内容《C#高级编程》

## 读写压缩文件 ##

* 名称空间：Using System.IO.Compression.
	* GZIP算法
	* Deflate算法

`

	static void SaveCompressedFile(string filename, string data);
		FileStream类、GZipStream类、StreamWriter类	

	static string LoadCompressedFile(string filename);
		FileStream类、GZipStream类、StreamReader类

## 监控文件系统 ##

* 名称空间：
	* Using System.IO;
	* Using Microsoft.Win32;
* FileSystemWatcher类
	* 提供的事件类型：
		* Changed
		* Renamed
		* Deleted
		* Created