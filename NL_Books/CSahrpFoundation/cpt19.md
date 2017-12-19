title: XML解析
date: 2017-12-10 19:00:00
tag: csharp

---

* 第19章
* XML和JSON

<!--more-->

# XML和JSON #

* XML和JSON都是数据语言，以简单的文本模式存储数据
* W3C标准格式，万维网联盟（又称W3C理事会）
* MS office中的数据存储也是xml，本身office不是.NET开发的应用程序

## 2. JSON基础 ##

* JSON（javascript object notation）
* 使用**中括号符号**和**大括号符号**所修饰

## 3. XML pattern ##

* XML文档可以用**模式类**来描述
* C#中的XML格式是XSD（XML Schema Definition）
* 经常使用的XML模式，可以存储在：C:\Program Files\Microsoft Visual Studio 14.0\Xml\Schema(在自己安装的visual stuido文件夹下)
* <?xml version="1.0" encoding="utf-8"?>；**<? ?>**处理命令需要用的符号
* 使用响应的模式时候，**select schema**会被选中相应符合的**xml schema**

## 4. XML文档对象模型 ##

* XML文档对象模型（DOM）是一种访问和处理XML的类
* 名称空间：Using System.Xml;
	* XmlNode
	* XmlDocument：用于加载XML文档，首先步骤
	* XmlElement：派生于XmlLinkedNode；XmlLinkedNode派生于XmlNode
	* XmlAttribute
	* XmlText
	* XmlComment
	* XmlNodeList

### 4.1 XmlDocument类 ###

* 创建、删除、修改**结点**

### 4.2 XmlElement类 ###

* FirstChild
* LastChild
* ParentNode
* NextSibling
* HasChildNodes

### 迭代XML中所有结点（案例） ###

### 4.3 修改结点的值 ###

* 继承自XmlNode类的子类都具有Value值，有时候可能无法返回有用的值
* <title>C# Program</title>
	* <title></title>：这个属于XmlElement
	* C# Program：这个属于XmlText
	* 可以通过XmlElement访问：InnerText方法，或者InnerXml方法
	* 可以通过XmlText访问：Value属性
* 1. 插入节点：
	* XmlDocument类，用于创建相应的XmlNode、XmlElement、XmlAttribute、XmlTextNode
	* XmlNode类或XmlDocument类，用于插入创建的节点，AppendChild、InsertAfter、InsertBefore
* 2. 删除节点：
	* RemoveAll()
	* RemoveChild()
* 3. 选择节点：
	* SelectSingleNode()：返回第一个找到的匹配的信息
	* SelectNodes()：返回一个集合

## 5. Xml转换为Json ##

*  Nuget Package Manager下的**NewtonsoftJSON.NET**包
*  详细API：www.json.net
*  Newtonsoft类-JSON类-JSONConverter类-SerializeXMLCode()

## 6. xPath语言搜索XML ##

* xPath语言用于查找Xml；就像SQL是关系型数据库的查询语言
* 有个大概了解就可以，因为语言方式与C#和SQL完全不同