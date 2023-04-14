# Unity2D开发（07）景观树

## 1.导入素材

导入【mystric_wood】【Objects】中的树木，按照Slice 16切割，将锚点设置为Bottom，如果树木有阴影的话可以修改Pivot的Custom，将Y值稍微调整

![01.导入与切割素材](https://github.com/sfresurgam/unity-development-doc/blob/main/07.Landscape%20Tree/source%20image/01.%E5%AF%BC%E5%85%A5%E4%B8%8E%E5%88%87%E5%89%B2%E7%B4%A0%E6%9D%90.png)

## 2.树木导入场景

将带有Fruit的树木拖拽到Hierarchy窗口的01.Courtyard场景中，并命名为Tree Fruits

修改层级关系为Instance，和Player一致，这样可以在Scene窗口中看到它

![02.修改Pivot和SortingLayer](https://github.com/sfresurgam/unity-development-doc/blob/main/07.Landscape%20Tree/source%20image/02.%E4%BF%AE%E6%94%B9Pivot%E5%92%8CSortingLayer.png)

增加Box Colldier 2D，用于制作与人物的碰撞关系

![03.添加BoxCollider](https://github.com/sfresurgam/unity-development-doc/blob/main/07.Landscape%20Tree/source%20image/03.%E6%B7%BB%E5%8A%A0BoxCollider.png)

增加Polygon Collider 2D，用于制作物体遮挡半透明的效果

![04.添加PolygonCollider](https://github.com/sfresurgam/unity-development-doc/blob/main/07.Landscape%20Tree/source%20image/04.%E6%B7%BB%E5%8A%A0PolygonCollider.png)

## 3.实现遮挡半透明

使用扩展工具包DoTween来实现人物走进Polygon Collider所在的物体区域时，遮挡部分逐渐变成半透明的效果

在https://assetstore.unity.com里搜索找到Dotween，添加到My Assets中

返回编辑器，可以在【Windows】-【Package Manager】-【My Assets】中找到Dotween并Import到项目内

![05.添加Dotween](https://github.com/sfresurgam/unity-development-doc/blob/main/07.Landscape%20Tree/source%20image/05.%E6%B7%BB%E5%8A%A0Dotween.png)

导入后点击SetUp Dotween，等待一段时间点击Apply，关闭Dotween的按钮即可完成导入

![06.设置Dotween](https://github.com/sfresurgam/unity-development-doc/blob/main/07.Landscape%20Tree/source%20image/06.%E8%AE%BE%E7%BD%AEDotween.png)

## 4.编写脚本

在【Scripts】-【Utils】文件夹中新建脚本ObjectFaderUtil.cs，将脚本挂载到Tree Fruits上

```c#
using DG.Tweening;
using UnityEngine;

namespace Utils
{
    [RequireComponent(typeof(SpriteRenderer))]
    public class ObjectFaderUtil : MonoBehaviour
    {
        [Header("精灵渲染器")] [SerializeField] private SpriteRenderer spriteRenderer;
        [Header("淡入淡出时间")] [SerializeField] private float fadeDuration = 0.35f;
        [Header("透明比例")] [SerializeField] private float targetAlpha = 0.45f;

        // 淡入
        public void FadeIn()
        {
            var targetColor = new Color(1, 1, 1, targetAlpha);
            spriteRenderer.DOColor(targetColor, fadeDuration);
        }

        //淡出
        public void FadeOut()
        {
            var targetColor = new Color(1, 1, 1, 1);
            spriteRenderer.DOColor(targetColor, fadeDuration);
        }
    }
}
```

在【Scripts】-【Player】文件夹中新建脚本PlayerTriggerObjectFader.cs，将脚本挂载到Player上

```c#
using UnityEngine;
using Utils;

namespace Player
{
    public class PlayerTriggerObjectFader : MonoBehaviour
    {
        private void OnTriggerEnter2D(Collider2D col)
        {
            var fader = col.GetComponent<ObjectFaderUtil>();
            if (fader != null)
            {
                fader.FadeIn();
            }
        }

        private void OnTriggerExit2D(Collider2D col)
        {
            var fader = col.GetComponent<ObjectFaderUtil>();
            if (fader != null)
            {
                fader.FadeOut();
            }
        }
    }
}
```

运行游戏，Player走到树木后面时，可以发现树木变成了半透明效果，走出树木遮挡范围，树木恢复到原来的不透明度

![07.测试效果](https://github.com/sfresurgam/unity-development-doc/blob/main/07.Landscape%20Tree/source%20image/07.%E6%B5%8B%E8%AF%95%E6%95%88%E6%9E%9C.png)

## 5.保存为预制体

在【Assets】文件夹下新建Prefabs文件夹，将Tree Fruits拖拽到Prefabs文件夹内即可

![08.保存为预制体](https://github.com/sfresurgam/unity-development-doc/blob/main/07.Landscape%20Tree/source%20image/08.%E4%BF%9D%E5%AD%98%E4%B8%BA%E9%A2%84%E5%88%B6%E4%BD%93.png)
