title: Unity with Web Service
date: 2017-12-10 10:25:00
tag: unity

---

* 天空盒的过渡效果
* 使用协程www来从web服务器下载资源
* 解析诸如XML和JSON格式的资源文件
* 下载下来的资源进行显示
* 将收集的数据返回到web服务器

<!--more-->

# 从web服务器下载数据 #

1. WWW类
	* error
	* text
2. 使用IGameManager进行接口的限制和访问
3. 统一开启管理器的接口
4. 使用协程进行数据的获取
5. 使用委托，进行该数据的获取

## 解析XML和JSON数据语言 ##

* 获取的长字符串，一般由xml和json两种不同的数据方式
* 通过using System.Xml;
* 获取其中的数据信息

## 添加一个网络布告栏 ##

* 下载图片数据
* 本地保存多次重复使用的图片数据
* 更新网络布告栏
····
## 将数据发送到Web服务器 ##

* WWWForm类用于http发送请求post
* WWW类用于http接受请求get