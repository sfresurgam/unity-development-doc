# Unity2D开发（一）项目初始化

## 1.新建项目

通过`UnityHub`下载`Unity`编辑器，版本为`2021.3.16f1`

选择新建项目，选择`2D核心模板`，名称为`Unity2dDemo01`

![01.创建项目](E:\Up\TyporaDoc\Unity\01.pic\01.创建项目.png)

## 2.移除Component

在主界面点击上方的`windows`，选择`PackageManager`，确保打开的是`Packages:In Project`

![02.打开PackageManager](E:\Up\TyporaDoc\Unity\01.pic\02.打开PackageManager.png)

我们这里移除掉下面四个自带的`Component`，分别为：

`Version Control`: `SCM`代码协作工具，由于我自己是单人开发，不需要该组件

`Visual Scripting`: 可视化开发工具，该项目打算使用代码编辑的方式完成游戏逻辑，不需要该组件

`Visual Studio Code Editor`: 该项目使用`Jetbrains`的`Rider`编辑器来进行`C#`脚本的开发，如果有用`VSCode`进行开发的小伙伴可以保留此选项

`Visual Studio Editor`: 同上，如果你是用`VSCode`进行开发则保留

![03.移除无用组件](E:\Up\TyporaDoc\Unity\01.pic\03.移除无用组件.png)

## 3.设置默认编辑器

在`Edit` => `Preferences` => `External Tools`里面设置项目的默认编辑器选择与路径

![04.配置默认编辑器](E:\Up\TyporaDoc\Unity\01.pic\04.配置默认编辑器.png)

## 4.导入人物素材

进入免费素材网站`itch.io`

网址为：`https://itch.io/game-assets/free`，选择`Mystic Woods - 16x16 Pixel Art Asset Pack`素材包下载

![05.itchio素材页面](E:\Up\TyporaDoc\Unity\01.pic\05.itchio素材页面.png)

下载完成后解压，可以看到里面有一个名为`sprites`的文件夹，所有的图片素材都在里面

进入`sprites` => `characters`，找到`player.png`，这个就是我们要用的人物素材

回到`Unity`编辑器，在`Project`窗口里面的`Assets`下，创建`Source`文件夹，用来存放所有的素材。再创建一层`Player`文件夹，存放人物`sprite`

在`Player`文件夹上右键，点击`Import New Asset`

![06.文件夹结构与导入素材](E:\Up\TyporaDoc\Unity\01.pic\06.文件夹结构与导入素材.png)

选中刚才的人物素材，点击导入![07.素材路径](E:\Up\TyporaDoc\Unity\01.pic\07.素材路径.png)

导入完成后可以在窗口中看到该素材，选中后可以在右边的`inspector`窗口看到详细信息

![08.素材详情](E:\Up\TyporaDoc\Unity\01.pic\08.素材详情.png)

## 5、切割精灵图

在`Inpsector`窗口中可以看到有非常多的选项，其中几个我们要用到的：

`Sprite Mode`：切割模式，分为`Single`、`Multiple`、`Polygon`，分别代表着这个图片里是有一个人物模型还是多个人物模型，如果是多个模型，按矩阵进行排列的就用`Multiple`，使用更加紧致的空白空间排列的模型就用`Polygon`模式。细心的小伙伴可以发现我们导入的这张素材是按矩阵进行切割的,所以`Sprite Mode`应该选择`Multiple`模式。

![09.Sprite Mode](E:\Up\TyporaDoc\Unity\01.pic\09.Sprite Mode.png)

`Pixels Per Unit`：每个网格可承载的像素点数，我们下载的这个素材包写的像素比为16*16，所以应该把这个参数修改为16，如果按照默认100的话，会导致人物在画面中非常的小，即每个网格能渲染出来的像素比这个人物当前的像素要大，显示的图像就对应的变小了

![10.Pixels Per Unit](E:\Up\TyporaDoc\Unity\01.pic\10.Pixels Per Unit.png)

修改完成后点击`Apply`保存，点击`Sprite Editor`打开图像切割器

![11.Sprite Editor](E:\Up\TyporaDoc\Unity\01.pic\11.Sprite Editor.png)

选择左上方的`Slice`，`Typo`为`Automatic`，`Pivot`锚点为底部，后面我们的人物和景观层进行判断的时候就是用底部的锚点进行判断的

模式改为`Smart`，点击切割，会发现每一个小人都被白框包围住了，代表已经切割完毕，点击`Apply`并关闭这个面板

在下方的视图中点击`>`按钮可以看到人物已经变成单个的小`sprite`素材了

![12.切割完成](E:\Up\TyporaDoc\Unity\01.pic\12.切割完成.png)

到这一步我们的项目前期准备工作已经完成，下一节将会开始写代码，让人物通过键盘操作移动起来



