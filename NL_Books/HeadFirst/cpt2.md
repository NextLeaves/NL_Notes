title: Observer Pattern
date: 2017-12-25 16:40:00
tag: pattern

---

* 观察者模式：
	* 定义一种一对多的依赖关系，这样一来，当对象的状态发生改变的时候，能影响依赖的对象获取通知并且相应的进行更新变化

<!--more-->

# 观察者模式(Observer Pattern) #

* 一纸合约：【工作合约】设计开发天气显示系统
	* 实现：温度、压力、湿度的显示，以不同的显示方式呈现，同时具有可扩展的能力
	* WeatherData类作为检测物理数据的实物
* 订阅报纸：【演示示例】
	* 出版者+订阅者=观察者模式
* 分析**观察者模式**	：
	* 以动物的角度来（示例）
	* 以找工作的角度来分析（五分钟短句）
* Queation：
	1. 为什么保存WeatherData的引用，后续并没有什么用？
	2. 这和一对多的关系有何关联？
* **松耦合的威力：**
	* 对于两个相互作用的对象，可以相互的调用彼此的方法，但是却对彼此的细节不清楚，可以随时的进行改变

---

* 松耦合的目的在于创建一个具有弹性的OO程序，可以动态的改变；是因为使得对象之间的依赖性降到最低
* 【办公室对话】：如何选择最佳的模式应对项目
* 是否直接把**值**传给**观察者**

---

* 增加新的值，**酷热指数**

---
* 【围炉夜谈】：针对于主题只能推给观察者，何不让观察者可以自主的索取想要的信息数据

---

* Java内置的观察者模式：
	* Observable类
	* Obaserver接口
* 缺点：
	* 对于观察者变化的顺序无法确定
	* **主题者**是一个类，并非是一个接口，限制我们向其他地方扩展
	* 既然继承了**类**，则无法继承**超类**的特性，行为java和C#都不支持多继承的方式