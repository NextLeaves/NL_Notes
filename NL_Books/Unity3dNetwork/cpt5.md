title: 代码分离的界面
date: 2017-12-26 9:30:00
tag: unity

---

* 什么是UGUI？
* 创建一套通用的代码分离的界面系统
* 创建适用于坦克游戏的界面

<!--more-->

# 代码分离的界面系统 #

* GUI：代码创建的老式界面系统
* NGUI：4.6版本中推出的可视化编辑界面

## 1. Unity UI系统 ##

* Canvas（画布）等一系列的UI组件，如：InputField

### 1.1 创建UI部件 ###

* 2D模式下编辑
* NGUI不可缺少的两个组件：
	* Canvas
	* EventSystem
* 通过R键，进行界面大小、轴心的调整

### 1.2 Canvas画布 ###

* 三种渲染方式（Render Mode）
	* Screen Space - Overlay
	* Screen Space - Camera
	* World Space
* 不同手机、不同电脑的分辨率、宽高比也不尽相同？
	* CanvasScaler组件：
		* 三种适配方式：
			1. 分辨率
			2. 宽高比
			3. DPI(Dot Per Inch)的屏幕；PPI(Pixel Per Inch)与DPI相似，一般可混用
	* CanvasScaler适配方法：
		* Constant Pixel Size：大小不变，位置改变
		* Scale with screen size：指定参考分辨率
			* 匹配宽高
			* 扩展：利用宽高中值小的一边进行判断，适配Canvas的宽高比和屏幕一样
			* 收缩：利用宽高中值大的一边进行判断，适配Canvas的宽高比和屏幕一样
			* （Tips）小的需要放大则是**扩展**；大的需要缩小则是**收缩**
		* Contant Physics Size:同Constant Pixel Size类似，此利用物理大小来调节维持缩放

### 1.3 EventSystem ###

* 负责监听用户的输入：
	* 键盘
	* 鼠标
	* 触摸屏幕
* 当需要屏蔽用户的时候，只要关闭此组件

### 1.4 RectTransform ###

* UGUI所特有的，必不可少的组件，继承自Transform
* 用于实现**界面的布局**和**层次控制**
* 相较于Transform，新增的内容：
	* pivot（轴心）
	* anchor（锚点）
	* sizeDelta:UI对象的尺寸变化量；即锚点固定UI对象的位置，等效于说锚点的位置
* （重点）**负责UI对象的层级关系**
	* 子级UI对象总是会覆盖父级UI对象
	* 同级UI对象，下面的UI对象总是覆盖上面的UI对象
	* 等效于：先出现的先渲染，后出现的后渲染，所以会覆盖

### 1.5 其他UGUI组件 ###

* （提示）：只有把**Texture Type**换成**sprite(2D and UI)**，方可用在NGUI上
* 常见属性：
	* Color
	* Source Image
	* Text

### 1.6 事件触发 ###

* 触发的函数条件：需要公开，**public修饰**
* OnClick()组件操作

### 1.7 简单的面板调用 ###

* 一个面板弹出另一个面板


## 2 制作界面素材 ##

### 2.1 标题面板和信息面板 ###

* 制作成显示内容就OK
* 添加相应的图片背景素材

### 2.2 保存为预设 ###

* 存储在Resources文件夹下


## 3. 面板基类PanelBase ##

### 3.1 代码与资源分离的优势 ###

* 美术人员与程序人员分工合作，互不干扰
* 利于代码复用；功能相同，外观不同的代码可无需更改
* 为游戏的热更新提供可能性

### 3.2 面板系统的设计 ###

* 基类PanleBase
	* 子类：TitlePanle
	* InfoPanel
	* 可扩展性面板

### 3.3 面板基类的设计要点 ###

* 面板的资源（skin）
* 面板资源的路径（skinpath）
* 面板的层级（panelLayer）枚举类型
* 定义可获取参数的方法，比如TitlePanel需要创建时生成相应的方法
* 面板应有的生命周期：
	1. Init
	2. OnShowing
	3. Onshowed
	4. Update
	5. OnClosing
	6. OnClosed

### 3.4 面板基类的实现 ###

* params 关键字的应用
* virtual 关键字的应用

## 4. 面板管理器PanelMgr ##

* 三项功能：
	* 层级管理
	* 打开面板
	* 关闭面板

### 4.1 层级管理 ###

1. 实现单例模式
2. 保存场景中canvas的引用
3. Dictionary<string,PanelBase>类型存储**已打开的面板信息**
4. Dictionary<PanelLayer,Transform>类型存储**各个层级所对应的父对象**
5. 初始化所有类型，并保存所有信息

### 4.2 打开面板 ###

* OpenPanel<T>()
* 使用泛型方法，进行面板的打开操作
* 操作流程：
	1. 判断是否打开面板
	2. 添加面板到dict
	3. 获取皮肤，加载皮肤
	4. 设置生成的对象，放置在相应的物体下面（**SetParent()函数**）

### 4.3 关闭面板 ###

* 操作流程：
	1. 判断是否存在面吧
	2. 从dict引用中移除
	3. 销毁皮肤对象
	4. 销毁面板对象

## 5. 面板逻辑 ##

### 5.1 标题面板TitlePanel ###

* button实现监听的两种方式：
	1. btn.onClick().AddListener(MethodName);
	2. 在NGUI的组件中，添加相应的对象组件
* 初始化后，获取相应的控制组件对象的引用，用于监听按钮，执行相应的方法

### 5.2 信息面板 ###

* 显示相应的信息
* 进行关闭

## 6. 调用界面系统 ##

### 6.1 界面系统的资源 ###

* prefab资源
* 脚本资源
* 面板资源
* canvas对象资源
	* panel
	* tips

### 6.2 界面系统的调用 ###

## 7. 胜负面板 ##

### 7.1 面板素材 ###

* 制作获胜面板素材
* 制作失败面板素材

### 7.2 面板逻辑 ###

* 创建WinPnael脚本
* 操作逻辑：
	1. 初始化面板
	2. 获取相应的资源
	3. 实时更新
	4. 关闭面板

### 7.3 面板调用 ###

* 调用获胜界面

## 8. 设置面板 ##

### 8.1 面板素材 ###

* DropDown的使用

### 8.2 面板逻辑 ###

* 创建战场的选择

### 8.3 面板调用 ###

* 调用战场类，生成战场