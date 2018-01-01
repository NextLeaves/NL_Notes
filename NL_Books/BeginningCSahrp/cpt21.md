title: Entity Framework
date: 2018/1/1 10:44:05 
tag: csharp

---

* 使用数据库
* 理解EntityFramework
* 使用CodeFirst创建数据
* Linq to sql
* 导航数据库关系
* 在数据中创建和查询XML

<!--more-->

# C# 之 数据库 #

## 1. 使用数据库 ##

* 数据库是**永久性**的，**结构化**的数据集合
* 最常见的存储和查询业务数据的类型是**关系型数据库**
* 在C#2015中，可以使用**CodeFirst**创建对象，存储到数据库，并使用**LinqToSql**查询数据；而不用使用**SQL语言**
* **安装SQL**
	* 轻量级**Sql Server Express**，免费版本
	* **LocalDB**，无需地址端口，可以通过VS直接连接，创建数据

## 2. Entity Framework ##

* 支持Code First的类库是Entity Framework
* **实体关系模型**，**实体**即数据对象的抽象表现
* Entity Framewordk讲Code First的对象映射到数据存储中的实体上
* EntityFramework基于**ADO.NET(ActiveX Data Object)**的结构，而ADO.NET是基于访问.NET底层的数据访问库，ADO.NET需要基础的SQL语言的知识来进行操作，而EntityFramework的封装，就解决了这个问题
* **EF**即EntityFramework的缩写；**EF**的全称为**ADO.NET EntityFramework**
* EntityFramework还支持Linq to EF，是支持的，通过此访问数据

## 3. Code First数据库 ##

* 安装EntityFramework，在NuGet中查找、安装
* 引入头文件：
	* using System.Data.Entity;
	* using System.ComponentModel.DataAnnotations;
* 特性：**[key]**标志递增序列的标记，且必须持有该**属性**
* DbSet<>：用于存放数据组
* DbContext：集合表所需继承的类

## 4. 数据库存放存放的位置 ##

* 默认存储位置：**C:\Users\wp8**
* 数据库的服务器名：**(localdb)\MSSQLLocalDB**

## 5. 导航数据库关系 ##

* Entity Framework最强大的功能是能够自动的创建**Linq对象**，实现发现对应的数据表之间的关系
* 操作步骤：
	1. [key]：修饰标识列
	2. virtual 修饰需要存在关系的属性
	3. 先生成数据表**using(var db = new BookContext())**
	4. 然后对相应的数据生成对象，然后存储在数据表中
	5. 相应的存储在对应的关系集合中

## 5. 处理迁移 ##

* 目的：若对之前设计的数据格式、类型、关系不满意，可以通过迁移的方式，重新重建一个新的数据库格式
* 若直接改变CodeFirst的格式，会出现以下报错
	* System.InvalidOperatorException
* 只有通过迁移，才能实现体系的改变
* 使数据库与改变的类保持一直是很复杂的，有了EntityFramework相对步骤较少
	* 添加NuGet包，**CodeFirstMigrations**
	* Tools|NuGet Package Manger|Package Manager Console
	* Enable-Migrations -EnableAutomaticMigrations
	* update-Database

## 6. Entity中创建和查询XML ##

1. using System.Xml.Linq;
2. 添加Entity Framework，NuGet工具
3. Linq to Entity，然后使用Xml相关的语句，创建Xml数据语言