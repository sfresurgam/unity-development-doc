# Unity2D开发（05）摄像机CineMachine

## 1.安装摄像机

在Package Manager中搜索Cinemachine，点击Install进行安装

![01.安装Cinemachine](https://github.com/sfresurgam/unity-development-doc/blob/main/05.CineMachine/source%20image/01.%E5%AE%89%E8%A3%85Cinemachine.png)

然后在Persistent Scene窗口中右键添加【GameObjetc】-【CineMachine】-【2D Camera】，并修改名称为Cinemachine

![02.添加2D Camera](https://github.com/sfresurgam/unity-development-doc/blob/main/05.CineMachine/source%20image/02.%E6%B7%BB%E5%8A%A02D%20Camera.png)

## 2.摄像机跟随与属性

在Inspector窗口中给定Cinemachine跟随和观察的对象：Player

在Game窗口中可以看到摄像机的焦距比较大，将屏幕的分辨率修改为4K UHD（3840 X 2160），并将Cinemachine的Lens Ortho Size修改为9

![03.摄像机设置](https://github.com/sfresurgam/unity-development-doc/blob/main/05.CineMachine/source%20image/03.%E6%91%84%E5%83%8F%E6%9C%BA%E8%AE%BE%E7%BD%AE.png)

在CineMachine的Body属性中找到Dead Zone Width和Dead Zone Height，为它们赋值0.08，这样摄像机就有了缓冲区，在一定范围内移动时整个场景不会跟着移动

![04.设置摄像机的缓冲区](https://github.com/sfresurgam/unity-development-doc/blob/main/05.CineMachine/source%20image/04.%E8%AE%BE%E7%BD%AE%E6%91%84%E5%83%8F%E6%9C%BA%E7%9A%84%E7%BC%93%E5%86%B2%E5%8C%BA.png)

## 3.设置摄像机边界

在Cinemachine的Inspector窗口中，找到Add Extension，为摄像机添加Cinemachine Confiner

注意：不要添加Cinemachine Confiner 2D，目前的游戏项目无论是2D还是3D都是添加Cinemachine Confiner

![05.添加Confiner](https://github.com/sfresurgam/unity-development-doc/blob/main/05.CineMachine/source%20image/05.%E6%B7%BB%E5%8A%A0Confiner.png)

可以看到在Bounding Shape 2D中需要一个Collider 2D，所以需要新建一个Polygon Collider 2D

因为边界是和场景绑定到一起的，不同场景所对应的边界也不一致，我们为除了Persistent Scene外的每一个场景都新建一个Game Object命名为Bounds，给Bounds添加Polygon Collider 2D进行边界的绘制

如果需要删除掉多余的线段，可以将鼠标移到需要移除的线条上，按住Ctrl+鼠标右键即可删除

![06.添加PolygonCollider2D](https://github.com/sfresurgam/unity-development-doc/blob/main/05.CineMachine/source%20image/06.%E6%B7%BB%E5%8A%A0PolygonCollider2D.png)

## 4.摄像机绑定边界

摄像机和Bounds如果在同一个场景中时，是可以通过拖拽的方式进行快速匹配的，但是跨场景会报错，比如Cinemachine在Persistent Scene中，而其它的Bounds都在分场景中，这时候需要使用代码的方式，先查找再匹配

在【Assets】【Scripts】文件夹下创建文件夹【Utils】和脚本SwitchBoundsUtil.cs，将脚本挂载到Cinemachine摄像机上

![07.添加脚本](https://github.com/sfresurgam/unity-development-doc/blob/main/05.CineMachine/source%20image/07.%E6%B7%BB%E5%8A%A0%E8%84%9A%E6%9C%AC.png)

此时如果想要在运行的时候找到场景中的Polygon Collider 2D，可以使用常见的标签查找法

为Bounds添加标签BoundsConfiner，并选中该标签

编写脚本SwitchBoundsUtil.cs

```c#
namespace Utils
{
    public class SwitchBoundsUtil : MonoBehaviour
    {
        // TODO 切换场景时会调用
        private void Start()
        {
            SwitchBoundsConfiner();
        }

        private void SwitchBoundsConfiner()
        {
            var boundsConfiner =
                GameObject.FindGameObjectWithTag("BoundsConfiner").GetComponent<PolygonCollider2D>();
            var cinemachineConfiner = GetComponent<CinemachineConfiner>();
            cinemachineConfiner.m_BoundingShape2D = boundsConfiner;
            cinemachineConfiner.InvalidatePathCache();
        }
    }
}
```

启动游戏，会发现Inspector窗口中摄像机能成功找到Polygon Collider 2D，但是人物被卡出地图了

勾选Bounds的属性Is Trigger，将它设置为一个触发器解决该问题

![08.游戏测试](https://github.com/sfresurgam/unity-development-doc/blob/main/05.CineMachine/source%20image/08.%E6%B8%B8%E6%88%8F%E6%B5%8B%E8%AF%95.png)

可以发现人物走到地图边缘时，摄像机不再显示空白区域，而是停留在Polygon Collider 2D设置的检测区域内

