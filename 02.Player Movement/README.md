# Unity2D开发（二）人物移动

## 1.生成Player

在Hierarchy窗口中的Scene上点击右键，选择GameObject创建一个空的素材Create Empty，并命名为Player

![01.创建GameObject-Player](https://github.com/sfresurgam/unity-development-doc/tree/main/02.Player%20Movement/source%20image/01.%E5%88%9B%E5%BB%BAGameObject-Player.png)

点击生成好的Player，在Inspector窗口中点击Add Component，为人物添加渲染器Sprite Rendener、碰撞体Rigidbody 2D

![02.添加Component](https://github.com/sfresurgam/unity-development-doc/tree/main/02.Player%20Movement/source%20image/02.%E6%B7%BB%E5%8A%A0Component.png)

添加完成后的效果

![03.SpriteRenderer And Rigidbody2D](https://github.com/sfresurgam/unity-development-doc/tree/main/02.Player%20Movement/source%20image/03.SpriteRenderer%20And%20Rigidbody2D.png)

将上一个章节中切割好的第一张精灵图player_0拖拽到Sprite Renderer的Sprite上

将Rigidbody 2D中的Gravity Scale改为0，避免游戏启动后人物存在重力会坠落出屏幕

![04.拖拽图片](https://github.com/sfresurgam/unity-development-doc/tree/main/02.Player%20Movement/source%20image/04.%E6%8B%96%E6%8B%BD%E5%9B%BE%E7%89%87.png)

此时能看到中间的Scene窗口中出现了Player的图片，点击Game窗口也可以看到人物在窗口中的样子

## 2.脚本实现移动

在Assets文件夹中新建文件夹Scripts，用于存放所有的C#脚本，新建Player.cs

![05.创建脚本文件](https://github.com/sfresurgam/unity-development-doc/tree/main/02.Player%20Movement/source%20image/05.%E5%88%9B%E5%BB%BA%E8%84%9A%E6%9C%AC%E6%96%87%E4%BB%B6.png)

双击打开Player.cs，下方会提供完整的代码

删除掉自带的函数Start和Update，添加相关变量

```c#
// 映射GameObject上面带的Rigidbody 2D, 需要手动拖拽绑定
[SerializeField] private Rigidbody2D rb2;
// 人物移动的速度
[SerializeField] private float speed;
// 按住方向键后横轴移动的距离
private float _inputX;
// 按住方向键后纵轴移动的距离
private float _inputY;
// 记录人物移动路径的二维数组
private Vector2 _moveVector;
```

[关于[SerializeField\]]: Unity提供了SerializeField特性对非静态的非公有字段进行序列化的手段,只要在这一类字段上使用这个特性,那么这个字段就会出现在Inspector面板并被序列化,但依然是非公有的.并且Unity会对出现的Inspector面板上的字段做一些名称适配,比如去掉“_”,小写变大写,数字加空格等等

创建函数GetInput，用来获取用户的输入

```c#
private void GetInput()
{
    // 获取横轴上的位移
    _inputX = Input.GetAxisRaw("Horizontal");
    // 获取纵轴上的位移
    _inputY = Input.GetAxisRaw("Vertical");
    // 如果同时按下了横轴和纵轴的方向键，速度会叠加变快，需要乘以一个系数保证斜方向运动时的速度稳定
    if (_inputX != 0 && _inputY != 0)
    {
        _inputX *= 0.6f;
        _inputY *= 0.6f;
    }
    _moveVector = new Vector2(_inputX, _inputY);
}
```

创建函数Walk，实现人物Position的变化

```c#
private void Walk()
{
    rb2.MovePosition(speed * Time.deltaTime * _moveVector  + rb2.position);
}
```

在函数Update中执行GetInput方法，在函数FixedUpdate中执行Walk方法

```c#
public void Update()
{
    GetInput();
}

public void FixedUpdate()
{
    Walk();
}
```

[关于[Update\]和[FixedUpdate\]]: Update是在每次渲染新的一帧的时候才会调用，也就是说，这个函数的更新频率和设备的性能有关以及被渲染的物体（可以认为是三角形的数量）。在性能好的机器上可能fps30，差的可能小些。这会导致同一个游戏在不同的机器上效果不一致，有的快有的慢。因为Update的执行间隔不一样了。而FixedUpdate，是在固定的时间间隔执行，不受游戏帧率的影响，是真实时间，所以处理物理逻辑的时候要把代码放在FixedUpdate而不是Update。最好只在物理逻辑的处理中使用FixedUpdate，而不要滥用

Player.cs全部代码：

```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    [SerializeField] private Rigidbody2D rb2;
    [SerializeField] private float speed;
    private float _inputX;
    private float _inputY;
    private Vector2 _moveVector;
    
    public void Update()
    {
        GetInput();
    }
    public void FixedUpdate()
    {
        Walk();
    }
    private void GetInput()
    {
        _inputX = Input.GetAxisRaw("Horizontal");
        _inputY = Input.GetAxisRaw("Vertical");
        if (_inputX != 0 && _inputY != 0)
        {
            _inputX *= 0.6f;
            _inputY *= 0.6f;
        }
        _moveVector = new Vector2(_inputX, _inputY);
    }
    private void Walk()
    {
        rb2.MovePosition(speed * Time.deltaTime * _moveVector  + rb2.position);
    }
}
```

## 3.挂载脚本

将Player.cs脚本挂载到GameObject Player上

![06.挂载脚本](https://github.com/sfresurgam/unity-development-doc/tree/main/02.Player%20Movement/source%20image/06.%E6%8C%82%E8%BD%BD%E8%84%9A%E6%9C%AC.png)

将Rigidbody 2d也挂载到刚生成的Player Component，修改Speed为5

![07.挂载Rigidbody 2d](https://github.com/sfresurgam/unity-development-doc/tree/main/02.Player%20Movement/source%20image/07.%E6%8C%82%E8%BD%BDRigidbody%202d.png)

## 4.运行Unity控制人物移动

点击屏幕上方中间的第一个三角按钮，进入测试界面

![08.测试按钮](https://github.com/sfresurgam/unity-development-doc/tree/main/02.Player%20Movement/source%20image/08.%E6%B5%8B%E8%AF%95%E6%8C%89%E9%92%AE.png)

按下【W A S D】键或者方向键【上 下 左 右】，可以在窗口中看到人物成功进行了移动







































