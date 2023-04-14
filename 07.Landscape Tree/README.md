# Unity2D开发（07）景观树

## 1.导入素材

导入【mystric_wood】【Objects】中的树木，按照Slice 16切割，将锚点设置为Bottom，如果树木有阴影的话可以修改Pivot的Custom，将Y值稍微调整

![01.导入与切割素材](F:\Unity\Unity Markdown File\Unity2D开发7\Picture\01.导入与切割素材.png)

## 2.树木导入场景

将带有Fruit的树木拖拽到Hierarchy窗口的01.Courtyard场景中，并命名为Tree Fruits

修改层级关系为Instance，和Player一致，这样可以在Scene窗口中看到它

![02.修改Pivot和SortingLayer](F:\Unity\Unity Markdown File\Unity2D开发7\Picture\02.修改Pivot和SortingLayer.png)

增加Box Colldier 2D，用于制作与人物的碰撞关系

![03.添加BoxCollider](F:\Unity\Unity Markdown File\Unity2D开发7\Picture\03.添加BoxCollider.png)

增加Polygon Collider 2D，用于制作物体遮挡半透明的效果

![04.添加PolygonCollider](F:\Unity\Unity Markdown File\Unity2D开发7\Picture\04.添加PolygonCollider.png)

## 3.实现遮挡半透明

使用扩展工具包DoTween来实现人物走进Polygon Collider所在的物体区域时，遮挡部分逐渐变成半透明的效果

在https://assetstore.unity.com里搜索找到Dotween，添加到My Assets中

返回编辑器，可以在【Windows】-【Package Manager】-【My Assets】中找到Dotween并Import到项目内

![05.添加Dotween](F:\Unity\Unity Markdown File\Unity2D开发7\Picture\05.添加Dotween.png)

导入后点击SetUp Dotween，等待一段时间点击Apply，关闭Dotween的按钮即可完成导入

![06.设置Dotween](F:\Unity\Unity Markdown File\Unity2D开发7\Picture\06.设置Dotween.png)

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

![07.测试效果](F:\Unity\Unity Markdown File\Unity2D开发7\Picture\07.测试效果.png)

## 5.保存为预制体

在【Assets】文件夹下新建Prefabs文件夹，将Tree Fruits拖拽到Prefabs文件夹内即可

![08.保存为预制体](F:\Unity\Unity Markdown File\Unity2D开发7\Picture\08.保存为预制体.png)