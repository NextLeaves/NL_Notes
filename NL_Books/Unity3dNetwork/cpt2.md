title: Controller Unit for Tank
date: 2017-12-19 15:50:00
tag: unity3d

---

* 行走控制
* 相机跟随
* 旋转炮台
* 车辆行驶
* 轮子和履带
* 音效

<!--more-->

# 坦克的控制单元 #


## 车辆行驶 ##

* 介绍**rigidbody功能**、**collider&collision功能**
* 介绍**wheelCollider**
	* motorTorque
	* brakeTorque
	* steerAngle
* Rigidbody.centerOfMass
* 影响车辆的三个主要因素：
	* 重力
	* 悬挂系统
		* spring:值越高，弹性越好；反之，越硬
		* damper：阻尼器
		* targetPosition：静止状态下的弹簧位置，0充分伸展，1充分压缩
	* 轮胎摩擦力
		* （forward friction）
		* （sliderways friction）
		* Extremum
		* Asymptote
* 轮轴（axle）
* 旋转轮胎：
	* **WheelCollider**提供GetWorldPos(out positon,out rotation)，用于返回轮胎的状态
* 贴图的位移offset
* 声音效果