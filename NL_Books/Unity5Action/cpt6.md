title: Unity 2D Function
date: 2017-11-22 19:00:00
tag: unity3d

---

> 在Unity中显示2D图形
> 
> 通过程序显示图形
> 
> 让对象可以点击
> 
> UI文本的管理和显示状态
> 
> 设置开始游戏和重置游戏

<!--more-->

## 设置2D图形 ##

* Edit | Project Settings | Editor | Default Behaviour Mode：可以通过这个设置，默认导入的图像变成**Sprite**格式
* piexls to units可以设置为1：1；不过一般设置100：1的情况比较好，因为物理引擎更好的操作，同时代码更好协调
* 精灵动画：一般是多个帧图片的连续播放，合并在一起成为精灵表，术语为图集
* 优点：1.减少图片中空白部分资源的浪费；2.减少draw call的机制（每加载一张图片，会增加显示卡的调用）

## 摄像机的处理 ##

* 设置camera的projection属性
* camera.size属性：camera视图中心点到screen顶端的距离；即设置屏幕高度的一半，就是piexl perfect（完美像素）
* 