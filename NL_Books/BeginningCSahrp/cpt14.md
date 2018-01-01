title: Base WPF
date: 2017-10-23 12:30:00
tag: CSharp

---

* The foundation of WPF
<!--more-->

## XAML Foundation ##

1. WPF支持**DirectX的高级功能**
	* **浮点坐标**和**适量图形**的操作（旋转/放大等）
	* 2D和3D渲染功能
	* 高级字体的处理
	* UI对象支持纯色/渐变/alpha值
	* 动画分镜设计
	* 可使用重用资源来动态设置控件的样式
2. “代码隐藏文件”：动态链接到XAML文件的C#文件
3. 一个**<>**内的东西称为：元素
	* Windows元素 是XAML的根元素
4. 两个值得注意的名称空间：
	* WPF的引用：**http://schemas.microsoft.com/winfx/2006/xaml/presentation**
	* 声明XAML本身:**http://schemas.microsft.com/winfx/2006/xaml**
	* 系统名称空间：**xmlns:sys="clr-namespace:System;assembly=mscorlib"**
	* 名称空间看起来像URL，其实为URI(Uniform Resource Identifiers)

## 动手实践 ##

1. 依赖属性(dependency property):自身修改的行为，通知观察者。
	* 好处：
		1. 根据样式修改依赖属性的值
		2. 可通过资源或数据绑定来设置依赖属性的值
		3. 可在动画中更改依赖属性的值
		4. ···
2. 附加属性(Attached Property):定义该属性的类实例的每个子对象上都可用的属性
3. 事件(Event)：完成用户的操作方式
	1. 路由事件(Routed Event):与用户操作关联的路由事件;也可以和普通事件一样，只发送给操作控件
		* 冒泡事件(bubbling event)：向父元素传递事件
		* 下钻事件(tunneling event)：向子元素传递事件（一般事件名称添加Preview前缀，总是发生在冒泡事件的前面；若要终止时间的传送，则e.Handle=true;）
	2. 处理事件：为事件添加处理程序
	3. 路由命令(routed command):在同一处编写的代码，可以让多处控件执行；（相比路由事件，其只能操作一处控件执行）
4. 控件类型：（分为两类）
	* 内容控件：只能在其中放置一个控件（如button中的content属性）
	* 项控件：可以在其中放置多个控件(如grid中放置多个button)
	* 其他控件：不能放置控件在其中(如image中只能放置图片)

## 控件布局 ##

1. 所有内容布局控件（项控件）都继承自抽象的Panel类
	* Panel类仅说明布局控件能放置UIElement的对象集合
	* 继承自Panel类的**项控件**：
		* Canvas：不限制其内容的布局
		* DockPanel：以紧贴的方式布局，类似于MS的flatten（扁平化）模式
		* Grid：以表格的形式布局
		* StackPanel：可以使用以行、以列的方式进行排序布局
		* WrapPanel：根据可用空间，进行多行多列的排序方式布局
2. 所有控件继承自UIElement类

### 堆叠顺序 ###

* 当一个项控件里面有多个控件时，会进行堆叠顺序的安排

### 对齐、边距、填充和尺寸 ###

* HorizontalAlignment
* VerticalAlignment
* Padding:控件边缘内侧的留白
* Margin：控件边缘外侧的留白
	* 也可以指定一个值（Thickness值）
* 注意事项：空间的width/height值受到其他属性的影响；比如horizontalalignment若为stretch

* **Border控件：**只能在其中添加一个子对象，实现margin、padding等操作
* **Canvas控件：**指定位置，使用Canvas.top/Canvas.Left···（其中top/left优先级高于bottom/right）
* **DockPanel控件：**前面控件（先创建的控件）所占的位置，后面的控件（后面创建的控件）就只能在未使用的位置显示，若有重复的位置，则前面空间会遮挡后面空间（与堆叠的方式不同）
* **StackPanel控件：**以列和行进行排序排列,Orientation值为horizontal/vertical
* **WrapPanel控件：**依次的填充空间，填充不下的换行或者换列继续填充；创建动态布局的好方法，可以使用户精确的摆放物体的位置
* **Grid控件：**以多行多列的方式，按照网格的形式进行布局摆放；
	* RowDefinitions/ColumnDefinitions属性；
	* 也可以通过Grid边缘来选择；
	* 可以通过设置**width="*"**，代表其他值算好，占满剩余空间
	* 还可以设置为Auto，根据内容来布局

## 控件说明 ##

* Textbox:允许用户输入一段信息
* Label:一行文本信息
* TextBlock:显示多行信息
* CheckBox:选择功能
* RadioButton：多个选项中，进行选择
* ComboBox：提供下拉选项的功能；还可以让用户自行输入内容
* TabControl：对内容进行分组显示
* DataContext：定义一个数据源，该数据源能够绑定某个元素的所有子元素
* ListBox：和ComboBox类似，但是此可以选择多项；同时ListBox是显示所有的选项，相对的占用的空间比ComboBox占用的多
* Slider：滑条，有minimum,maximum,value值
* ProgressBar：进度条类型，有同上参数，可以相互搭配使用

## 数据绑定 ##

* 数据绑定：以声明的方式将控件和数据绑定在一起的方法
* 一个绑定关系由4种组件构成：
	1. 绑定目标：指定要绑定要应用到的对象
	2. 目标属性：指定要设置的属性
	3. 绑定源：指定绑定的对象(Binding ElementName="")
	4. 源属性：指定存储该数据的属性（Path=""）

### 绑定本地对象 ###

* 一个控件的属性，影响另一个控件的属性（即：选中绑定目标(ComboBox)，同时选择目标属性(ComboBox.IsEnabled)；再编写以下代码，默认为绑定目标为所绑定的目标；ElementName为绑定源（CheckBox）,Path为源属性(IsChecked)）

`
	<!--说明CheckBox的IsChecked属性 影响 ComboBox的IsEnabled值 -->
	<ComboBox.IsEnabled>
		<Binding ElementName="PlayAgainstComputerCheck"，Path="IsChecked" />
	</ComboBox.IsEnabled>

	<ComboBox IsEnabled="{Binding ElementName=PlayAgainstCOmputerCheck， Path=IsChecked}" />
	<!--标记扩展语言-->（**注意逗号的使用**）

### 静态绑定到外部对象 ###

1. 在xaml中添加响应的名称空间
2. 然后再xaml中声明该类为资源（可在希望绑定目标的父元素上创建资源的引用）

`

	<!--引用外部资源-->
	<Canvas.Resource>
		<local:类名 x:Key="属性名">
	</Canvas.Resource>
	
	<!--需要引用的位置添加属性-->
	<控件名 ItemSource="{Binding Source={StaticResource 属性名} }" />
	

### 动态绑定到外部对象 ###

1. 将DataContext属性的值设置为此实例（需要绑定的目标数据），其所在的窗口的xmal.cs中添加目标对象字段就可以引用此对象的属性
	* DataContext=_gameOptions;（在构造函数中添加.InitializeComponent()之前）
2. 在xaml中就可以引用该对象的值
	* IsChecked="{Binding Path=PlayAgainstComputer}"
	* SelectedValue="{Binding Path=NumberOfPlayers}"