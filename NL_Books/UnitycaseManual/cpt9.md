title: Character Customization
date: 2018/1/10 18:07:46 
tag: unity3d

---

* 人物换装
* Asset Bundle 

<!--more-->

# 1. 加载场景 #

* 创建场景
	* 删除脚本错误
* 添加场景元素
	* 添加模型
	* 创建反射的镜面
	* 调整画面到合适的角度
	* 创建ambientLight
	* 创建generalLight

## 2. AssetBundle相关API介绍 ##

* AssetDatabase类:资源数据库
	* CreateAsset(Object asset,string path)
	* DeleteAsset(Object asset)
	* GetAssetPath()
	* LoadAssetAtPath()
* PrefabUtility类：
	* CreateEmptyPrefab(string path)
	* ReplacePrefab()
	* InstatiatePrefab(Object target)
* BuildPipelien类
	* BuildAssetBundle
* WWW类
	* LoadFromCacheOrDownload()
	* assetbundle.mainasset访问获取的资源
* ScriptableObject类
	* 用于扩展数据、图像存储的资源
* **其他知识：**
	* Application.datapath：获取文件路径
		* @"file://+Application.datapath+/target.asset";