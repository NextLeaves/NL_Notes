title: 构建一个让你置身3D空间的演示
date: 2017-11-5 14:35:00
tag: Unity3D

---

* 了解3D坐标空间
* 在场景中放置一个玩家
* 编写移动对象脚本
* 实现FPS控件

<!--more-->

# 构建一个让你置身3D空间的演示 #

## 对项目做计划 ##

* 策划内容：
	1. 构建场景
	2. 摆放灯光和摄像机
	3. 创建玩家对象
	4. 编写移动脚本：使用鼠标旋转、键盘移动

* 了解3D坐标空间
	* 笛卡尔坐标系：有一个故事内容
		* 左手坐标系
		* 右手坐标系
		* 通常**X指向右**，**Y指向上**
	* unity和很多3D艺术应用程序都是用**左手坐标系**
	* 3dmax、openGL等软件使用的是**右手坐标系**

## 构建项目 ##

* 什么是GameObject：
	* 场景中所有的游戏对象，都是GameObject类的实例
	* 相当于一个容器，把拥有MonoBehaviour的功能组件添加在上面
* 玩家的碰撞器和视口：添加Collider
* 让东西移动：应用变换的脚本
	* **航向偏角**：沿着X轴旋转
	* **偏航**：沿着Y轴旋转
	* **侧滚**：沿着Z轴旋转
* 本地和全局坐标空间
	* 一般情况下，脚本函数基于本地坐标进行调用处理

## 用于观察周围的组件脚本：MouseLook ##

1. 枚举确定旋转的方向：垂直方向、水平方向、垂直水平方向
2. 跟踪鼠标运动的水平旋转
	* transform.Rotate(0,Input.GetAxis("Mouse X")*SensitivityHor,0);
3. 跟踪鼠标运动的垂直旋转
	* 限制旋转的值:_rotationY=Mathf.Clamp(目标值，最小值，最大值);
	* 启动旋转：transform.localEulerAngles=new Vector3(_rotationY,selfRotationY,0);
	* 为什么不适用Rotate()进行，Y轴的旋转；因为Y轴需要限制最小值和最大值

`

	//注意内容：旋转的值是进行加减，而不是一直是同步值
	public class MouseLook : MonoBehaviour
	{
	    public enum LookAxis : byte
	    {
	        RotateX,
	        RotateY,
	        RotateXY
	    }
	
	    float _rotateX = 0f;
	
	    public LookAxis axis = LookAxis.RotateX;
	
	    void Update()
	    {
	        if (axis == LookAxis.RotateX)
	        {
	            transform.Rotate(0, Input.GetAxis("Mouse X") * Time.deltaTime * 10.0f, 0);
	        }
	        else if (axis == LookAxis.RotateY)
	        {
	            //进行值的加减，而不是只是赋值
	            _rotateX -= Input.GetAxis("Mouse Y") * Time.deltaTime * 10.0f;
	            _rotateX = Mathf.Clamp(_rotateX, -45f, 45f);
	            transform.localEulerAngles = new Vector3(_rotateX, transform.localEulerAngles.y, 0);
	        }
	        else if (axis == LookAxis.RotateXY)
	        {
	
	        }
	    }
	}

* 欧拉角(Euler angle)和四元数(Quaternion)
	* 表述旋转的方法是欧拉角
	* 四元数是高等数学中一个比较晦涩的概念
	* 为什么使用四元数进行旋转的一个理由是：
		* 四元数在旋转值之间插值，看起来更加平滑和自然
	* 所有游戏中的对象都要受到物理系统的影响，所以禁止摄像头的旋转与移动,gameobject.freezeRotation;

## 键盘输入组件：第一人称控件 ##

1. tranform.Translate()函数
2. 设置独立于计算机运行速度的运动速率
	* 不同电脑上面的帧率不同，调用的次数就不同：帧率依赖(farme rate dependent);Time.deltaTime;
3. 为了碰撞检测，CharacterCOntroller组件：
	* 使用cc组件移动，而不是tranform
	* Move():通过外力来移动控制器，并沿着碰撞体滑动，且该外力只受限于碰撞
	* SimpleMove()：使角色按照一定的速度向前移动，忽略Y轴上面的速度；重力会被自动调用
	* transform.TransformDirection()，从本地坐标转换为世界坐标
4. 属性：
	* [RequireComponent(typeof(Rigibody))]:保证添加Rigidbody到基本指定的对象身上
	* [AddComponentMenu("Control/Menu Item Name")]：添加到快捷方式菜单Add