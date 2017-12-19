title: Reflection Metadata Dynamic Program
date: 2017-12-16 14:50:00
tag: csharp

---

* 使用自定义特性
* 运行期间，使用反射检查元数据
* 从支持反射的类中构建访问点
* 理解动态语言运行库
* 使用动态类型
* 托管DLR ScriptRuntime
* 用**Dynamic Object**和**Expando Object**创建动态类型（Expando：自定义类型）

<!--more-->

---

# 反射、元数据、动态编程 #

## 1. 在运行期间检查代码和动态编程 ##

* 自定义特性：可以用于创建元数据，运行期间创建数据，并嵌入到程序集中
* expando、reflection用于软件的升级，且记录升级的信息
* expando方式实现自动匹配**数据库**的**表、列**信息，对应**编程方式**的**类、属性**
* 动态编程基于**ExpandoObject框架**

## 2. 自定义特性 ##

* 实现创建数据的数据，也就是说：数据的位置信息、存储大小、信息域等
* **反射**可以获取程序的**自定义特性**的数据，决定了代码的执行方式
	* 可以在实现有扩展框架的代码中，实现加载插件或模块

### 2.1 编写自定义特性 ###

* 所编写的特性类，直接或者间接继承自**System.Attribute；**
* 若某个类、接口、属性等被特性修饰，则若有**Attribute**后缀，则不添加，反之则添加；然后再相应的所有using空间中去寻找该类，然后判断该**自定义特性**是否正确的加载，最后执行相应的特性方法
* **自定义特性**通常需要考虑的问题：
	1. 能够应用用什么、比如类、接口、属性，限制特性的使用范围
	2. 是否能够被多次使用
	3. 是否默认自动应用于派生类、接口继承
	4. 特性所具有可选和必选参数内容

#### 2.1.1 AttributeUsage ####

* System.AttributeUsage;是元属性，只能用于修饰自定义属性
* 其中**AttributeTargets枚举**中的asembly和modul可以应用于程序集和模块，可以放置在任何位置，但是需要声明表示：如：
	* [assembly:SomeAssemblyAttribute(parameters)]
	* [module:SomeModuleAttribute(parameters)]
* 同时可以使用 **|** 运算符，进行逻辑或操作
* AllowMultiple:是否可重用

#### 2.1.2 指定特性参数 ####

* 使用构造函数来确定即可

#### 2.1.3 指定特性的可选参数 ####

* 使用公共属性，进行访问值
* 如：**[FieldName("Name",comment="This is a primary key field")]**
* 对应的自定义属性类，需要增添一个public string comment；

## 3. Reflection ##

* System.Type类
* System.Reflection.Assembly类

### 3.1 System.Type类 ###

* 获取类型的三个方式：
	1. typeof()
	2. object.GetType() (成员方法)
	3. Type.GetType() (静态方法)
* Type是许多反射功能的入口
* Type中许多可用属性是只读属性，不能进行修改类型

#### 3.1.1 Type的属性 ####

* 返回与类相关的**字符串信息**的属性
	* Name
	* FullName
	* Namespace
* 返回Type的直接引用类型
	* BaseType
	* UnderlyingSystemType
* 许多返回Bool类型
	* IsClass
	* IsAbstract
	* IsEnum
	* IsPrimitive
	* IsValueType
* 也可以将类型存储为Assembly实例的一个引用用来返回
	* Type t = typeof(Vector);
	* Assembly containAssembly=new Assembly(t);
* Type类型中的方法：（更多的是用于返回类型的具体**成员信息**）
	* GetMethod()、GetMethods()：返回方法的细节，返回System.Reflection.MethodInfo对象的引用
	* 其中包含的枚举**BindingFlags**枚举值：用于选择获取的成员信息，比如公共方法，私有方法，静态方法
	* MemberInfo对象无法获取对象类型的**函数签名**，要获取**函数签名**，需要MemberInfo和更特殊的对象，即需要分别获取每一种类型的成员的详细信息

### 3.3 Assembly类 ###

* 位于System.Reflection；名称空间下
* 可以读取程序集的元数据、以及访问程序集中的方法（前提程序集是可加载的）
* 加载**程序集**的方法
	* Load()：从本地文件或者全局程序集缓存中寻找相应的字符串内容
	* LoadFrom()：从指定的位置找出程序集
* 获取**程序集中的所有类型**
	* GetTypes()
* 获取**程序集中的自定义类型**
	* 为什么会费经周折的为自定义类型编写类？原因在于，继承自Attribute类的子类都可以通过以下方式保存
	* Attribute[] attributes=Attribute.GetCustomAttributes(assembly);
* 获取**类型下的属性、方法等的自定义类型**
	* 需要获取MemberInfo、MethodInfo对象，调用GetAttribute()方法
	* MemberInfo、MethodInfo调用Type类对象的GetMemberInfo()

---

## 总结经验 ##

1. 在验证程序集的元数据的时候，先判断**属性的范围AttributeTarget**，其他的操作基于此
2. 项目测试步骤：
	1. 加载程序集
	2. 获取相应程序集的元数据（判断是否是所访问的程序集信息-这是**SupportWhatsNewAttribute存在的原因**）
	3. 获取相应的所有的类型（只针对**类**类型进行筛选{Type.GetTypeinfo()}）
	4. 首先处理类类型所具有的**属性**
	5. 后着手处理类中**成员(Member)**中包含的**函数（Method）**进行**属性**的获取，以及显示