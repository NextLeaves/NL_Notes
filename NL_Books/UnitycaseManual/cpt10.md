title: Third-Person Game
date: 2018/1/11 17:09:41 
tag: unity3d

---

* 第三人称游戏

<!--more-->

# 第三人称射击游戏 #

## 1. 加载场景 ##

* 导入资源包
* 创建相应的文件夹保存
	* _Scripts
	* _Scenes

## 2 添加场景元素 ##

### 2.1 添加背景元素 ###

* 创建地形
* 防止主角跨界，设置碰撞体
* 设置灯光特效
* 调整Camera位置

### 2.2 添加主角模型 ###

* 主角模型放在空物体的下面
* 为主角添加pointlight灯光特效，环绕主角周围
* 为Player对象添加CapsuleCollider和Rigidbody组件，非模型对象上

### 2.3 添加NPC模型 ###

* 类似于主角的添加方式，去除灯光效果
* 实现动画的特效，Idle

## 3. 第三人称角色运动控制 ##

### 3.1 添加控制角色运动的功能 ###

* PlayerController.cs，放置在Player对象上，而非模型上
	* 重力驱动：
		* 对rigidbody的引用
		* groundDrag的设置
	* 速度：
		* 移动速度speed
		* 旋转速度turnSpeed
		* 鼠标速度mouseTurnSpeed
		* 跳跃速度jumpSpeed
	* 奔跑和行走：
		* bool walking
		* float walkSpeedDownScale
	* 是否在地上：
		* isGrounded
		* Physics.Raycast()
		* groundedCheckOffset
		* LayerMask
		* groundedDistance
	* 绘制射线：
		* showGizmos
	* 鼠标状态：
		* requiredLock
		* controlLock
	* Update()：
		* 处理旋转操作
	* FixedUpdate():
		* 处理落地检测
		* 处理跳跃操作
		* 处理移动操作
	* OnDrawGizmos():
		* 处理画线操作

## 4. 摄像机运动控制 ##

* 