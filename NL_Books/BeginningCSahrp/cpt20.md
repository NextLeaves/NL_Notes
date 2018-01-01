title: Linq control
date: 2017/12/29 20:48:28 
tag: csharp

---

* 使用Linq to xml
* Linq提供程序
* Linq查询语法
* Linq方法语法

<!--more-->

# LINQ集锦 #

* **Linq to xml**
* **Linq提供程序**
* **Linq语法**
	* **Linq查询语法**
	* **Linq方法语法**
		* linq排序
		* linq查询大型数据集
		* 使用聚合函数
		* 单值选择查询
		* 多级排序
		* 组合查询
		* Join查询

## 1. Linq to xml ##

* **namespace :**
	* Using System.Xml.Linq;
* **class :**
	* XDocument
	* XElement 
	* XAttribute 
	*  **unusual class :**
		*  XComment :注释
		*  XDeclaration :声明
		*  XProcessingInstruction：处理指令
*  XContainer类（SuperClass）
	*  XElement（SubClass）
	*  XDocument （SubClass）
	*  实现相关功能：
		*  Load()
		*  Save()
		*  Parse()
*  Save->Load->Write
*  **以上方法为：**
	*  函数构建（Functional Contruction）
	*  之前用**XmlDocument类**，属于**DOM模式**，构建Xml

## 2. Linq 提供程序 ##

* **Linq to Objects：**
	* 对于任何类型且存在于c#内存中的对象提供查询操作，比如：数组、列表或者集合类型
* **Linq to Xml：**
	* 利用Linq的特性，与其说是和其他扩展方法一样的方式，来操纵xml文档
* **Linq to Entities：**
	* Entity Framework是MS中最新的一种数据接口，推荐使用。可以用此操作数据
* **Linq to Data Set：**
	* DataSet是.NET第一版就引入的对象，Linq支持对旧数据的操纵
* **Linq to SQL：**
	* 这是另一个Linq接口，这个取代了Linq to Entities
* **PLinq：**
	* PLinq是并行Linq
	* 并行编程扩展了Linq的操作方式
	* 可以拆分查询，利用多核多资源的方式，提高查询效率
* **Linq to Json：**
	* Newtonsoft包的使用，进行Xml到Json的相互转换
	* 可以操作Json数据类型的Linq机制

## 3. Linq查询 ##

* Linq查询语法
* Linq方法语法

### 3.1 Linq查询语法 ###

* namespace:System.Linq;

`

	string[] names = { "sakura", "jim", "lady", "andy", "sundy", "adner" };
	            var queryResults =
	                from name in names
	                where name.StartsWith("s")
	                orderby name.Substring(name.Length - 1)
	                select name;
	
	            foreach(var name in queryResults)
	            {
	                Console.Write(name + " ");
	            }

* 经验：
	1. var很适用于Linq的查询机制，不需要考虑返回值的类型，由编译器来判断
	2. 格式内容：
		* 指定数据源
		* 指定条件
		* 选择元素（必定操作）
	* 延迟执行、迟缓执行：
		* 查询结果仅仅保存了查询的逻辑计划，在输出的时候才进行查询操作


### 3.2 Linq方法语法 ###

* Linq的实现，基于扩展方法，根据Intellisense来查看和使用方法
* 有些操作，只能使用**方法语法**而不能使用**查询语法**，但是**查询方法**更容易理解，推荐在能使用**查询方法**的时候，尽量使用**查询方法**

`

	var query = names.Where<string>(s => s.StartsWith("s"));
	
	            foreach (var item in query)
	            {
	                Console.WriteLine(item);
	            }

### 3.3 Linq排序 ###

* 关键字：
	* orderby
	* descending（默认为ascending）

`

	var queryResultsTwo = from c in customs
	                     orderby c.Age, c.Region
	                     select new { c.Id, c.Name, c.Region, c.Age };

### 3.4 查询大型数据集 ###

* 只要知道**源数据**就可以进行判断，不需要特定的规范查询的变量名

### 3.5 聚合函数的使用 ###

* 关键函数名：
	* Count()
	* Sum()
	* Average()
	* Min()
	* Max()
	* 自定义类型：Aggregate()
* 聚合函数所返回的值是单值操作，所以会**立即操作**，而不是**延迟操作**
* 经验：
	1. Sum()返回的值一般为int类型，所值太大会导致溢出，需要进行转换**Sum(v=>(long)v)**;
	2. LongCount()可以返回**long类型的值**
		* 前提，满足源数据为64位数据
		* 若源数据为64位数据，则其他**聚合函数**需要转换**值类型**

### 3.6 单值选择查询 ###

* 关键词：
	* Distinct()：获取唯一，未重复值
	* Select()：选择
* 只能利用方法函数之后进行操作
* 查询语法间接的利用方法函数的调用，由编译器转换

### 3.7 多级排序 ###

`

	var queryResultsTwo = from c in customs
                          orderby c.Age, c.Region
                          select new { c.Id, c.Name, c.Region, c.Age };

### 3.8 组合查询 ###

`

	var query =
                from c in customs
                group c by c.Region into cg
                select new { Total = cg.Sum(a => a.Age), Ages = cg.Key };

* 通过组合之后的键值（Key）属性来进行判断**分组原则，分组依据**
* 其实现了**IGrouping接口**，其包含**Key属性**

### 3.9 Join查询 ###

* 关键词：
	* Join
	* on
	* equals
* 用法：用于连接两个数据结构类似的不同数组或者集合

`

	var queryJoin =
                from c in customs
                join o in orders on c.Id equals o.Id
                select new { c.Id, c.Name, c.Region, c.Age, o.Count };