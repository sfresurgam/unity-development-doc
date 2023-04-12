# Unity2D开发（06）碰撞层Collision Layer

## 1.添加Collision碰撞素材

将Collision.png素材添加到项目中，并进行切割，导入到一个新的调色盘【Collision Palette】中

![01.添加碰撞素材](https://github.com/sfresurgam/unity-development-doc/blob/main/06.Collision%20Layer/source%20image/01.%E6%B7%BB%E5%8A%A0%E7%A2%B0%E6%92%9E%E7%B4%A0%E6%9D%90.png)

## 2.添加碰撞属性

在Hierarchy窗口的Collision层中，添加TileMap Collider 2D和Composite Collider 2D

在自动创建的Rigidbody2d中，将Body Type设置为Static，将TileMap Collider 2D的Used By Composite设置为True，这样就能把所有绘制的Collision方格看做一个整体

![07.添加碰撞属性](https://github.com/sfresurgam/unity-development-doc/blob/main/06.Collision%20Layer/source%20image/07.%E6%B7%BB%E5%8A%A0%E7%A2%B0%E6%92%9E%E5%B1%9E%E6%80%A7.png)

## 3.绘制碰撞层地图

选择Collision Palette进行绘制

绘制完成后的效果图

![02.碰撞层绘制](https://github.com/sfresurgam/unity-development-doc/blob/main/06.Collision%20Layer/source%20image/02.%E7%A2%B0%E6%92%9E%E5%B1%82%E7%BB%98%E5%88%B6.png)

在之后的制作过程中，红色的碰撞层可能会影响测试与美观，所以我们可以将Collision层的TileMap Renderer前面的对号取消

## 4.为人物添加碰撞体

找到Player，添加Box Collider 2D，点击Edit Collider，在人物的脚部位置绘制一个四方体，代表人物的碰撞面积

![03.人物碰撞体](https://github.com/sfresurgam/unity-development-doc/blob/main/06.Collision%20Layer/source%20image/03.%E4%BA%BA%E7%89%A9%E7%A2%B0%E6%92%9E%E4%BD%93.png)

将【Edit】【Project Settings】【Graphic】中的Transparency Sort Mode修改为Custom Axis，Transparency Sort Axis的Y改为1，Z改为0，修改之后摄像机就会正确的处理人物和物品的叠层关系

![04.修改叠层关系](https://github.com/sfresurgam/unity-development-doc/blob/main/06.Collision%20Layer/source%20image/04.%E4%BF%AE%E6%94%B9%E5%8F%A0%E5%B1%82%E5%85%B3%E7%B3%BB.png)

## 5.解决碰撞后角色会旋转

通过为Rigidbody 2D 添加约束实现，冻结z轴

![05.解决碰撞旋转](https://github.com/sfresurgam/unity-development-doc/blob/main/06.Collision%20Layer/source%20image/05.%E8%A7%A3%E5%86%B3%E7%A2%B0%E6%92%9E%E6%97%8B%E8%BD%AC.png)

## 6.解决碰撞时卡进缝隙

将人物Rigidbody 2D的碰撞检测修改为连续检测

![06.解决碰撞卡墙](https://github.com/sfresurgam/unity-development-doc/blob/main/06.Collision%20Layer/source%20image/06.%E8%A7%A3%E5%86%B3%E7%A2%B0%E6%92%9E%E5%8D%A1%E5%A2%99.png)

