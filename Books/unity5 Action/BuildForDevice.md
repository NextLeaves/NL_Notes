title: 将游戏部署到设备
date: 2017-11-21 10:35:00
tag: unity3d

---

> 为不同的平台构建应用包
> 
> 配置游戏，构建游戏其他参数：应用图片、名称
> 
> 为了Web游戏与Web交互
> 
> 在移动平台为应用开发插件

<!--more-->

## Preface ##

* 切换平台将耗费大量的时间，已愿意等待耗时的损失；因为unity会重新压缩所有的资源（例如：贴图）

## 构建平台：windows、mac、linux ##

* 编辑构建后脚本：例如处理：帮助文档在应用安装后，移动到安装的相应的目录下

### 平台依赖的编译 ###

* unity一般来讲是：在不同的平台以相同的运行方式来运行，但是有些支持编译器指令（称为：平台定义）
* unity对每个平台都有单独的支持的指令，但是大部分代码不需要指令来处理，但有少部分需要来处理；一些代码只是存在一些平台，比如web播放器上访问

~

	class TestPlatform:Monobehaviour
	{
	    OnGUI()
	    {
	        #if platform_web
	        GUI.Label(new Rect(10,10,100,10),"Running in web");
	        #elif platform_standalone
	        GUI.Label(new Rect(10,10,100,10),"running in platform.");    
	    }
	}
	
	
## 构建IOS和Android应用 ##

1. Input，输入方式不同
2. IPA（IOS App package）

### 贴图压缩 ###

1. 因为资源会占用大量的空间，其中包括贴图等；所以如何使在画质没有太大变化的可能性下，尽可能的改善空间占用的问题，这就是我们在处理纹理所用到大部分压缩算法，来实现占有空间的问题
2. 在其他平台中，对贴图也有要求；但是对于移动设备的要求更加严格，因为移动设备性能的限制，对这部分内容更加的敏感
3. 这部分压缩是默认的，但是针对部分需要手动压缩的纹理进行处理
4. 安卓设备存储碎片化；而ios设备有统一的处理纹理压缩的方式---ios应用通过设备的GPU内获得贴图优化的操作；而Andorid无法获得统一的的优化方案，所以只能针对性的优化，使用最低的通用性标准
5. 针对平台的算法：
	* IOS：PowerVR GPUs；因此优化PVR贴图压缩
	* Android：PowerVR芯片（有些使用）；Adreno（大部分使用）；Mali GPUs（大部分使用）；从而Andorid设备支持**爱立信贴图压缩**（ETC ericsson Texture Compression）

### 开发插件（plugin） ###

* 特定处理的插件方式
* 待续···