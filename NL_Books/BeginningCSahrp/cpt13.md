title: High Techology
date: 2017-10-24 22:58:00
tag: CSharp

---

* The High Level

<!--more-->

## ::运算符和全局名称空间限定符 ##

`

	using targetSpace=BaseSpaceName.SecondSpace;
	
	namespace BaseSpaceName
	{
		namespace FirstSpace
		{
			class MyClass
			{}
		}
		
		namespace SecondSpace
		{
			class MyClass
			{}
		}
		
	}

	//访问第一个FirstSpace的类名，顺序优先级高
	targetSpace.MyClass myClass=new targetSpace.MyClass();
	//::访问方式,则为别名（alias）选中的名称空间的类型
	targetSpace::MyClass myClass=new targetSpace.MyClass();
	
	//顶级根名称空间：global
	global::System.Collections.Generic.List<int>
	//肯定不是访问以下的内容：
	namespace MySpace
	{
		namespace System
		{
			namespace Collections
			{
				namespace Generic
				{
					class List<>
				}
			}
		}
	}
	
## 定制异常 ##

* 基类：System.Exception;
	* 基本的异常类：ApplicationException和SystemException;它们都派生自Exception类
	* 最初的做法是前者是编程自定义的异常；后者为.NET框架使用的异常；现在常用Exception来派生程序员自定义的异常

`

	using System.Exception;
	
	class CardsOutOfRangeException:Exception
	{
		private Cards deckCards;
		
		public Cards DeckCards
		{
			get{deckCards;}
			private set{deckCards=value;}
		}
		
		public CardsOutOfRangeException(Cards sourceCards):base("This deck only has 54 cards.")
		{
			deckCards=sourceCards;
		}
		
	}	


## 事件 ##

1. 应用程序-（创建）-可以引发事件的对象；事件再调用响应观察的对象
2. 与事件匹配的**返回值和对应的参数类型**，也是委托定义的类型

### 处理事件 ###

1. 创建事件处理的函数
2. 订阅事件类型
3. 执行操作

### 定义事件 ###

1. 选择相应的委托，或者定义新的委托；作为事件的约定类型；
	* 一般以···Handler命名（如：public delegate void MessageHandler(string message)）
2. 定义事件，触发事件代理名；
	* 即：public event MessageHandler MessageArrived;

### 多用途的事件处理程序 ###

`

	//定义事件传参类型
	public class MessageArrivedEventArgs
	{}

	//定义通用委托
	public event EventHandler<MessageArrivedEventArgs> MessageArrived;

	//使用事件
	MessageArrived?.Invoke(this,new MessageArrivedEventArgs(name));


## Anonymous Method ##

`

	public delegate void SayHi(string message);
	SayHi+=delegate(string message)
		{
			cw($"Hi, my name is {message}");
		}

## 特性Attribute ##

* 为所创建的代码提供额外信息的方法：添加特性
* 例子1：
	* 对代码的检查，跳过代码的检查，对函数修饰[DebuggerStepThrough]
	* 有对此修饰多个特性：用逗号间隔；或者多个[]中括号修饰
* 反射（Reflection）：
	* 可以动态的检查对象的类型信息
	* 最初的方式可以通过 typeof或者类型的信息，或者通过对象.GetType()获取类型的信息
	* 通过反射，可以在Using System.Reflection名称空间中通过各种类型来获取不同的类型信息
* 获取特性：
	* Type targetType =typeof(对应的类);
	* object[] targetAttributes = targetType.GetCustomAttributes(true);
	* foreach(object x in targetAttributes){}
* 创建特性：
	* 两个选择：
		* 其应用到什么类型的目标（类、属性或者其他）
		* 是否可以对同一个目标进行多次的应用
	* 逻辑顺序：
		* 继承System.Attribute类
		* 通过专用特性来约束自定义的特性,AttributeUsageAttribute
			* 其中一个参数为：AttributeTargets枚举，组合需要应用到的什么类型目标
			* AllowMultiple:可否多次应用

## 初始化器 ##

`

	Curry curry=new Curry
	{
		Name="Xiaoming";
		Property=new TargetClass
		{
			age=10;
		}
	}

	* 嵌套的初始化器时，首先创建的是顶级类Curry的对象，然后创建嵌套的对象TargetClass
	* 而是用构造函数，则是相反的

## 集合初始化器 ##

`

	List<Farm> animal = new List<Farm>
	{
		Animals =
		{		 
		new Cow{Name="Lea"},
		new Chicken{Name="Noa"}
		}
		
	}

* 或者继承IEnumerable<T>类，然后实现Add方法

## 类型推断 ##

* var用于IDE来推断所初始化的类型，前提是若是var声明，则必须初始化，即：var x; 前者为错误的输入方法
* 推断数组：var array=new[] {1,2,3};
	* 相同的类型
	* 相同的引用类型或者空
	* 所有元素的类型都可以隐式的转化为一个类型
* var如果在作用域类声明的是类类型，则不能再出现其他不同的以var判断的其他类类型

## 匿名类型 ##

* 匿名类型一旦实现，只能读取不能修改
* **'a** 代表匿名类型

`

	var curry=new
	{
		Name="Lea",
		Style="dance",
		age=5
	};

	var curry=new[]
	{
		new {Name="Xiaoming"},
		new {Name="xiaohua"}

	};

## 动态查找 ##

`

	dynamic myDynamicVal;

* dynamic类型只会在编译期间存在，在运行期间会被替换成System.Object类型替代
* 若调用的动态类型的对象，没有存在的方法，则抛出**RuntimeBinderException**异常，在名称空间**Using Mircorst.CSharp.RuntimeBinder**

## 高级方法参数 ##

1. void PrintMessage(string message, bool fullInfo = false);
	* 在定义函数的时候，给出值（必须是字面值，常量、默认值类型值）
	* 可选参数的位置在非可选参数的后面，且必须是最后
2. 使用[Optional]特性，描述参数，可以不用给出初始值；名称空间:**Using System.Runtime.InteropServices**
3. 自定义选择需要修改的参数位置：
	* 定义函数：void PrintMessage(int x1, int x2=0, int x3=0);
	* 调用函数：PrintMessage(10, x2:10, x3:20);

## Lanmbda表达式 ##

1. 复习：匿名方法，即提供的内联（inline）方法
	* 对于快速实现委托的方面：
	* timer.Elapsed+=delegate(object s, ElapedEventArgs a){---};
	* delegate:两重用法
		* 定义委托的类型
		* 实现匿名方法
2. delegate比较混乱，在实现匿名函数时，可以使用Lambda表达式
	* 推断参数的类型格式： (s,a)=>cw($"{s.ToString()},and {a}");
	* 确定参数类型格式：(int s, int a)=>cw($"{s.ToString()},and {a}");**不能一部分隐式、一部分显式，无法编译**
3. 泛型委托：
	* Action<>:至多8个参数，返回值类型void
	* Func<>：至多9个参数，最后一个参数为返回值类型，返回值类型可自定义
	* 