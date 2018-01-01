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