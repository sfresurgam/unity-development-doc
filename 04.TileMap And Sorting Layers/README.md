# Unity2D开发（四）地图创建与分层

## 1.导入地图素材

将itch.io上下载的mytsic_woods素材包中的grass.png和plains.png导入到Unity工程的【Assets】-【Source】-【Maps】中

![01.导入地图素材](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/01.%E5%AF%BC%E5%85%A5%E5%9C%B0%E5%9B%BE%E7%B4%A0%E6%9D%90.png)

修改grass属性如下

![02.设置Grass属性](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/02.%E8%AE%BE%E7%BD%AEGrass%E5%B1%9E%E6%80%A7.png)

修改plains属性如下

![03.设置Plains属性](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/03.%E8%AE%BE%E7%BD%AEPlains%E5%B1%9E%E6%80%A7.png)

切割Plains，将Slice Type设置为Grid By Cell Size，按照16个像素点进行切割，Pivot设置为Center，Method设置为Smart

![04.切割Plains](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/04.%E5%88%87%E5%89%B2Plains.png)

## 2.创建Tile Map及分层

在Hierarchy窗口中通过【Create】-【2D Object】-【TileMap】-【Rectangular】进行创建，并将Grid改名为TileMaps

在TileMaps内创建多个层级，方便之后绘制和展示不同层级间的地图及物体，各个层级的描述和作用如下

Ground Bottom：地图最底层，用于土地、草地、耕地的绘制

Ground Middle：地图中间层，用于水域、高原、道路的绘制

Ground Top：地图最上层，用于花草、岩石、灌木、暗礁、水草的绘制

Stuff：装饰层，用于树木，家具，路灯的绘制，可进行交互

Instance：实体层，用于人物、敌人、NPC、物品的绘制，可进行交互

Collision：碰撞层，用户绘制碰撞关系

![05.创建TileMaps](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/05.%E5%88%9B%E5%BB%BATileMaps.png)

同时在Inspector窗口Layers的Sorting Layers中添加与上面一致名字的层级

![06.创建Sorting Layers](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/06.%E5%88%9B%E5%BB%BASorting%20Layers.png)

将TileMap中的Sorting Layer修改至和上面的名字保持一致

例如，Hierarchy窗口Ground Bottom中TileMap Renderer的属性Additional Settings的Sorting Layer应修改为Ground Bottom

![07.修改Sorting Layer](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/07.%E4%BF%AE%E6%94%B9%E4%BA%BA%E7%89%A9%E7%9A%84Sorting%20Layer.png)

别忘了，将人物Player的Sprite Renderer中Sorting Layer也修改为Instance

![07.修改人物的Sorting Layer](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/07.%E4%BF%AE%E6%94%B9%E4%BA%BA%E7%89%A9%E7%9A%84Sorting%20Layer.png)

## 3.创建主场景与地图场景

在整个的游戏中会存在多个场景，我们想要实现每个场景在切换的时候，只切换地图的部分，有些东西是保留而不切换的，比如GameManager，InventoryManager等管理数据的工具

首先将当前的SampleScene更改名字为Persistent Scene，并新建一个场景起名为01.Courtyard，代表庭院场景

![08.添加场景Courtyard](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/08.%E6%B7%BB%E5%8A%A0%E5%9C%BA%E6%99%AFCourtyard.png)

将这个新创建的场景拖拽到Hierarchy窗口中，这样就出现了两个叠加的场景，并且出现了2个摄像机，Courtyard的摄像机是用不到的，移除掉这个摄像机即可

然后将TileMaps整个拖拽到Courtyard场景中，主摄像机就会根据游戏加载不同的场景切换对应的地图了

![09.场景Courtyard添加TileMaps](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/09.%E5%9C%BA%E6%99%AFCourtyard%E6%B7%BB%E5%8A%A0TileMaps.png)

后续也可以按照当前这个空场景进行绘制地图，所以把Courtyard复制一份，改名为Template留作之后使用

## 4.创建Tile Palette

选中Ground Bottom，在Scene窗口的右上角会出现一个紫色的方格Icon，点击它就会显示Tile Palette的界面

![10.Tile Palette Button](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/10.Tile%20Palette%20Button.png)

选择Create New Palette，命名为Ground Map，点击Create，保存到Assets文件夹下的Tile Palette中（没有该文件夹新建即可）

然后将Source中的grass.png和plains拖入Ground Map中，保存至Assets文件夹Tile Palette中的Ground Map Tiles（没有该文件夹新建即可）

![11.创建Ground Map Palette](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/11.%E5%88%9B%E5%BB%BAGround%20Map%20Palette.png)

## 5.绘制地图

在Tile Palette窗口中，选择激活的Active TileMap与素材方格，在Scene窗口中进行绘制

如果你想使用其他的素材，按照之前的方式去itch.io网站进行download、import、slice和draw即可

绘制好的地图如下

![11.绘制初始地图完成](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/11.%E7%BB%98%E5%88%B6%E5%88%9D%E5%A7%8B%E5%9C%B0%E5%9B%BE%E5%AE%8C%E6%88%90.png)

## 6.处理Tiles间的缝隙

在绘制瓦片地图的过程中，可以发现部分素材之间会存在缝隙，可以通过Sprite Atlas来解决

在Assets文件夹中创建Sprite Atlas文件夹，点击右键选择创建【2D】-【Sprite Atlas】

这个图集可以将所有的图片素材，打包成一张完整的图片

我们将Source下用到的图片素材拖拽到Objects for  Packing中，将Filter Mode改为Point，Compression改为None，点击Pack Preview即可

![12.使用Sprite Atlas](https://github.com/sfresurgam/unity-development-doc/blob/main/04.TileMap%20And%20Sorting%20Layers/source%20image/12.%E4%BD%BF%E7%94%A8Sprite%20Atlas.png)

