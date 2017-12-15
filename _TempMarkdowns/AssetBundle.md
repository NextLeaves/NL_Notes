title: Asset Bundle
date: 2017-12-15 22:50:00
tag: unity3d

---

* AssetBundle是什么？

<!--more-->

# Asset Bundle #

* 什么是AB？
* AB有什么用？
* AB怎么创建？
* AB怎么读取、加载？
* AB存储的方式？
* AB是否跨平台？

## 什么是AssetBundle ##

* 在unity3d中能够被开发者导出的游戏模型、特图、音频、或者开发者编写的以.bytes为后缀的二进制文件；unity特有的压缩方式，能够动态的加载Assetbundle里面的信息。
	* 可以动态的实现游戏的扩展
	* 可以重复的利用，加载资源

## 使用AssetBundle的开发流 ##

* 工作流：
	1. **开发阶段：**
		* 创建AssetBundle文件
		* 记录、存储信息
		* 导出到外部存储设备或服务器
		* （**BuildPipeline类**）：
			* BuildAssetBundle()：创建任意的资源
			* BuildStreamedSceneAssetBundle()：以流的方式存储场景信息
			* BuildAssetBundleExplicitAssetNames()：与第一个相似，多一个自定义名称的功能
	2. **运行阶段：**
		* WWW类下载资源，LoadFromCacheOrDownloaded()
		* WebPlayer缓存上限为50Mb、PC、Mac以及移动平台Ios、Android平台多大4G的上限
		* 加载资源的方式：(**AssetBundle类**)
			* LoadAsset()
			* LoadAssetAsync()
			* LoadAllAssets()
	3. **Selection**类：
		* 用于鼠标选中的对象
		* GetFiltered()用于获取指定类型的东西


## 如何使用本地的AssetBundle ##、

* 移动平台有一个特殊的文件夹不会处理：**SteamingAssets**
* 若存在在**StreamingAssets**文件夹下的是未经处理和压缩的assetbundle可以直接使用AssetBundle的静态类CreateFromFile()；
* 若存在处理和压缩的，只能通过LoadFromCacheOrDownload()进行加载

## 如何准确的识别资源 ##

* 默认方式：若一个资源的地址为："Assets/Textures/myTexture.png"，那么在识别AssetBundle只需要访问myTexture；
* unity的好处还有，正如刚刚提到的创建AssetBundle的最后一个静态方法，可以让开发者自定义创建的AssetBundle的名称