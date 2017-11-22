title: 为3D游戏添加敌人和子弹
date: 2017-11-5 17:00:00
tag: Unity3D

---

* 讨论瞄准和开火，适用于玩家和敌人
* 碰撞检测和反馈
* 让敌人四处走动
* 在场景中生成对象

<!--more-->

# 为3D游戏添加敌人和子弹  #

* 射击游戏是教授射线方法的好例子 

## 通过射线设计 ##

* 什么是射线发射：射线是一个虚拟的、或者说看不见的线，从一个原点开始往一个方向发射过去
* 网格对象：是一些连接的线和网状结构构成的3D可视化结构

* 使用命令：ScreenPointToRay来发射
	* 通过一条射线进行物品的拾取(point picking)行为的特例
	* 从而传入Physics.Raycast()方法中，进行判断
	* RaycastHit类，接受碰撞的点的信息
* 创建对象(GameObject.CreatePrimitive()PrimitiveType.Sphere)
* **渲染**是计算机绘制3D场景像素的行为；真正显示到显示器的是2D颜色的像素各自，通过计算机计算每个像素的值，实现3D的效果的算法成为渲染

## 脚本化反应的目标 ##

* 实现击中对象，实现反馈内容

`

	using System;
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class Shooter : MonoBehaviour
	{
	    private Camera _camera;
	    private Vector3 centerPos;
	
	    void Start()
	    {
	        _camera = GetComponent<Camera>();
	        centerPos = new Vector3(_camera.pixelWidth / 2, _camera.pixelHeight / 2, 0);
	
	        HideCursor();
	    }
	
	    void Update()
	    {
	        Shooting();
	    }
	
	    void OnGUI()
	    {
	        float size = 24.0f;
	        float height = _camera.pixelHeight / 2 - size / 4;
	        float width = _camera.pixelWidth / 2 - size / 2;
	
	        GUIStyle crossHair = new GUIStyle();
	        GUIStyleState crossHairState = new GUIStyleState();        
	
	        crossHairState.textColor = Color.blue;
	        crossHair.hover = crossHairState;
	
	        GUI.Label(new Rect(width, height, size, size), "*", crossHair);
	    }
	
	    void HideCursor()
	    {
	        Cursor.lockState = CursorLockMode.Locked;
	    }
	
	    void Shooting()
	    {
	        if (Input.GetMouseButtonDown(0))
	        {
	            Ray centerRay = Camera.main.ScreenPointToRay(centerPos);
	            RaycastHit centerHit;
	            if (Physics.Raycast(centerRay, out centerHit))
	            {
	                ReactingHit enemy = centerHit.transform.gameObject.GetComponent<ReactingHit>();
	                if (enemy != null)
	                {
	                    enemy.OnHit();
	                }
	                else
	                {
	                    StartCoroutine(SphereIndicator(centerHit.point));
	                }
	                
	            }
	        }
	
	    }
	
	    IEnumerator SphereIndicator(Vector3 point)
	    {
	        GameObject sphere = GameObject.CreatePrimitive(PrimitiveType.Sphere);
	        sphere.transform.position = point;
	
	        yield return new WaitForSeconds(1f);
	
	        Destroy(sphere);
	    }
	
	
	
	
	}


`

	public class ReactingHit : MonoBehaviour
	{
	
	    public void OnHit()
	    {
	        StartCoroutine(ReadyToDie());
	    }
	
	    IEnumerator ReadyToDie()
	    {
	        Vector3 targetRotation = this.transform.position - new Vector3(0, 0, -45.0f);
	        targetRotation = Vector3.Lerp(this.transform.position, targetRotation, 0.5f);
	        transform.rotation = Quaternion.LookRotation(targetRotation);
	
	        yield return new WaitForSeconds(1.5f);
	
	        Destroy(this.gameObject);
	    }
	
	
	
	}

## 基本漫游AI ##