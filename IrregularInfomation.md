title: Irrigular Information
date: 2018-1-1 0:00:00
tag: review

---

* 对于复习、或者学习知识的时候，对于所看内容的扩展信息，进行记录的过程
* 为了加深对于脑海中灵光闪过的知识点，进行详细的百度或者Google寻找细节来认证自己所回忆的知识点
* 收集自己在学习或者复习中所遇到的困惑，并进行解决的记录过程

<!--more-->

# 无矩信息 #

## 1. 自动属性的存在意义 ##

* 适用的范围和技巧：
	1. 在接口中，进行声明属性；因接口中无法定义字段，使用自动属性来表示
	2. 当field不限制访问权限，又不想打破规则（即field声明为private，property声明为public），则使用**自动属性**，省去烦恼

## 2. C#预处理器指令 ##

* 用途：
	* 计划发布多种版本的情况下（例如：基本版本、更多功能的企业版本）
* 预处理器指令都以**#**符号开头
* C#不像C++一样有单独的预处理器，所谓的预处理器实际上是由编译器处理的
* **注意：**
	* 语法的变化，不需要使用分号结尾，默认预处理语句为一行，下一句才是执行命令
* **条件编译(conditional compilation)**：即使用声明选择编译部分
* **类型集：**
	* define
	* undef
	* error
	* warning
	* region
	* endregion
	* if
	* elif
	* else
	* endif
	* **line**
		* #line default
		* #line 30
	* **pragma**:抑制或者还原指定的编译警告
		* #pragma warning disabled 169
		* #pragma warning resotre 169