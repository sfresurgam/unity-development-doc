# Unity2D开发（08）TileMap 动画

这一章我们将为水体添加动态的Wave效果，而这一效果是在TileMap上实现的

mystric_wood素材包内有water1-6的素材，将它们全部导入并按照16*16的像素比进行切割

## 1.导入2D TileMap Extras

在Package Manager的Unity Register中搜索2D TileMap Extras并安装

注意：在新版本创建的2D项目已经内置了该扩展包

![01.导入Extras](F:\Unity\Unity Markdown File\Unity2D开发8\Picture\01.导入Extras.png)

## 2.新建Animated Tile

在【Assets】-【Animations】文件夹内新建文件夹【Tile Animation】用来保存Tile动画

进入文件夹，右键新建【Create】-【2D】-【Tile】-【Animated Tile】，复制12份，将它们分别命名为Water Wave Animated Tile 01 - 13

![01.新建Tile Animation](F:\Unity\Unity Markdown File\Unity2D开发8\Picture\01.新建Tile Animation.png)

水体的流动素材图片一共有6帧，所以在Number Of Animated Sprites这里设置为6，并把每一帧的左上角的素材拖入sprite1到6

下方Minimum Speed可以设置动画播放的速度

![03.拖拽素材](F:\Unity\Unity Markdown File\Unity2D开发8\Picture\03.拖拽素材.png)

将剩余的12个也按照相同的方式拖拽

## 3.加入Palette中

![04.将Tile Anim拖入Palette中](F:\Unity\Unity Markdown File\Unity2D开发8\Picture\04.将Tile Anim拖入Palette中.png)

然后就可以在地图上进行绘制了