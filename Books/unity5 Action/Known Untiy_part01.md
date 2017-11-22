title: 初识Unity
date: 2017-11-5 13:22:00
tag: Unity3D

---

* 为什么Unity如此优秀
* 如何使用Unity
* 开始使用Unity编程
* 小结

<!--more-->

## 为什么Unity如此优秀 ##

### Unity的优势 ###

* 两个相比其他游戏引擎极具优势的方面：
	1. 极度高效的可视化流
	2. 多维度的跨平台支持
	3. （微妙的优点）使用模块化组件系统构建游戏对象
* 缺点：
	1. 对于复杂的工程，无法找寻到具体哪个物体附加了什么组件；虽然有搜索，但是比较粗糙
	2. Unity不支持链接额外的代码库，必须手动复制到相应的项目中；而不是引用一个中心点来实现库的链接使用
	3. 必须使用预设（prefab），使用可编辑的工作流，编辑流程比较粗糙

### 使用Unity构建的案例 ###

* [http://www.unity3d.com/showcase/gallery](http://www.unity3d.com/showcase/gallery "Unity案例：")
* 开发类型
	1. 桌面（Window、mac、linux）
	2. 移动平台（ios、android）
	3. 主机（playstattion、xbox、wll）

## 如何使用Unity ##

* 界面的介绍

## 开始使用Unity编程 ##

* 为什么使用C#，而不是JavaScript
	1. 前者为强类型语言

* 脚本组件：
	* 短小、完整的实用程序
	* 更像是独立的OOP类，附加到游戏对象的是类的实例
	* 只有集成MonoBehaviour类的才是脚本组件
	* 名称空间：Using UnityEngine;

* 使用MonoDevelop，跨平台的IDE
	* 提供的功能只是提供编写的环境，执行脚本的代码，还是有Unity中的play来执行，而并不是IDE的play
	* IDE的play的功能是实现脚本debug的断点调试使用

### 打印Hello World ###

* 注意Console的作用就好了
* Debug类的使用

# 小结 #

1. Unity是一个多平台的开发工具
2. Unity的可视化编辑器可以协同工作的几个部分
3. 脚本是作为组件附加到对象上的
4. 代码是使用MonoBehaviour编写的内部脚本

