# Unity2D开发03-人物动画

## 1.动画资源存储设计

Assets文件夹下创建Animators文件夹，用于存放所有的动画资源

Animators中再创建一层文件夹PlayerAnimation，用于存放Player单个的动画文件

Animators中点击右键，选择Create创建Animator Controller，命名为PlayerAnimatorController

创建完成后的资源图如下

![01.资源创建](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/01.%E8%B5%84%E6%BA%90%E5%88%9B%E5%BB%BA.png)

## 2.分析原素材

在第一章切割精灵图的时候，我们能发现切割出来的这么多图片，其实可以按照人物面朝的方向、动作去进行分类，一串方向、动作相同的图片可以认为是一个完整的帧动画，比如：

player_0到player_5是人物【面朝屏幕】【站在原地】的动画序列帧

player_6到player_11是人物【面朝右侧】【站在原地】的动画序列帧

player_12到player_17是人物【背对屏幕】【站在原地】的动画序列帧

![02.人物站在原地序列帧](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/02.%E4%BA%BA%E7%89%A9%E7%AB%99%E5%9C%A8%E5%8E%9F%E5%9C%B0%E5%BA%8F%E5%88%97%E5%B8%A7.png)

player_18到player_23是人物【面朝屏幕】【走动】的动画序列帧

player_24到player_29是人物【面朝右侧】【走动】的动画序列帧

player_30到player_35是人物【背对屏幕】【走动】的动画序列帧

![03.人物走动序列帧](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/03.%E4%BA%BA%E7%89%A9%E8%B5%B0%E5%8A%A8%E5%BA%8F%E5%88%97%E5%B8%A7.png)

注意素材里没有提供人物面朝左侧的图片，是因为我们可以在制作动画的过程中调整FlipX来实现镜面替换的效果

## 3.创建Animator与Animations

在Player的Inspector窗口中新增Component：Animator，用它来管理Player所有的Animation

绑定刚才创建的PlayerAnimatorController

![04.绑定AnimatorController](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/04.%E7%BB%91%E5%AE%9AAnimatorController.png)

在windows窗口中选择Animation，分别点选Animation和Animator，会打开两个窗口，按自己的喜好固定好窗口位置

![05.打开动画窗口](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/05.%E6%89%93%E5%BC%80%E5%8A%A8%E7%94%BB%E7%AA%97%E5%8F%A3.png)

选中Player，再点击Animation窗口的Create按钮，来创建第一个动画，我们命名为PlayerIdleDown.anim

![06.创建PlayerIdleDown动画文件](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/06.%E5%88%9B%E5%BB%BAPlayerIdleDown%E5%8A%A8%E7%94%BB%E6%96%87%E4%BB%B6.png)

将Player精灵图素材中的player_0到player_5拖拽到Animation窗口右侧的时间序列上

![07.拖拽player素材形成动画帧](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/07.%E6%8B%96%E6%8B%BDplayer%E7%B4%A0%E6%9D%90%E5%BD%A2%E6%88%90%E5%8A%A8%E7%94%BB%E5%B8%A7.png)

将时长调整到自己舒服的位置，我这里设置的是0:50

切换到Game窗口，点击Animation中的播放键，可以看到人物按照序列帧循环的方式动了起来

![08.查看效果](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/08.%E6%9F%A5%E7%9C%8B%E6%95%88%E6%9E%9C.png)

接下来我们按照同样的方式分别创造PlayerIdleUp、PlayerIdleRight、PlayerIdleLeft、PlayerWalkDown、PlayerWalkUp、PlayerWalkRight、PlayerWalkLeft

这里左侧的动画需要在Animation中点击AddProperty，选择Sprite Renderer中的Flip X，点击➕号添加到动画中，并勾选后面的对号，即可看到人物面朝左侧的动画效果

![09.FlipX实现镜面左侧](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/09.FlipX%E5%AE%9E%E7%8E%B0%E9%95%9C%E9%9D%A2%E5%B7%A6%E4%BE%A7.png)

全部制作完成后的动画列表

![10.Animations制作完成](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/10.Animations%E5%88%B6%E4%BD%9C%E5%AE%8C%E6%88%90.png)

## 4.绑定Idle Blend Tree

切换到Animator窗口，我们通过Blend Tree来实现错综复杂的动画切换效果，避免使用连线的方式去进行切换

首先将所以刚才与人物自动绑定的动画删除，只留下最初的三个状态Entry、AnyState、Exit

![11.Animator界面](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/11.Animator%E7%95%8C%E9%9D%A2.png)

点击右键选择Create State -> From New Blend Tree

![12.创建BlendTree](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/12.%E5%88%9B%E5%BB%BABlendTree.png)

选中新增的Blend Tree，在Inspector窗口中更名为PlayerIdle，双击进入

在左侧的Parameters中可以新建三个变量，分别是Float类型的InputX，Float类型的InputY和Bool类型的Moving，这三个变量我们通过代码的形式去进行赋值和操作，并将原来的Blend变量删除

在右侧，将Blend Type更改为2D Simple Directional，Parameters设置为InputX和InputY

![13.BlendTree设置](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/13.BlendTree%E8%AE%BE%E7%BD%AE.png)

下方的Motion中，可以通过Add Motion Field绑定动画，并设置面朝四个方向所对应的变量值

比如绑定了动画PlayerIdleDown，那么X轴移动的值为0，Y轴移动的值为-1，换为PlayerIdleUp，则Y轴移动的值为1

![14.BlendTree Motions](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/14.BlendTree%20Motions.png)

绑定好四个Idle状态的判定，即完成了人物在Idle状态下的动画切换

## 5.绑定Walk Blend Tree

在Animator窗口点击Base Layer回到上一层，再创建一个Blend Tree，命名为PlayerWalk，双击进入

因为变量是这个Animator公用的，所以只需要按照Idle的方式设置右边的属性

![15.Blend Tree Walk Motions](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/15.Blend%20Tree%20Walk%20Motions.png)

## 6.状态切换Idle与Walk

在Animator窗口中的PlayerIdle上右键选择Make Transition，连接到PlayerWalk上

选中这条连接线，在右边Inspector窗口中，将has exit time的对勾取消，Transition Duration修改为0，Conditions中添加Moving为true的条件

![16.连接线1](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/16.%E8%BF%9E%E6%8E%A5%E7%BA%BF1.png)

同样在PlayerWalk上选择Make Transition，连接到PlayerIdle，配置相同的属性，此处Conditions中添加Moving为false的条件，代表由Walk状态转为Idle状态时需要判断Moving这个变量是否变成了false

![17.连接线2](https://github.com/sfresurgam/unity-development-doc/blob/main/03.Player%20Animation/source%20image/17.%E8%BF%9E%E6%8E%A5%E7%BA%BF2.png)

进入Player.cs脚本，添加属性animator和_isMoving，实现走动与静止动画的切换函数SwitchAnimation，在GetInput中通过moveVector是否为0判断isMoving的状态

添加相关逻辑后的脚本：

```c#
using UnityEngine;

public class Player : MonoBehaviour
{
    [SerializeField] private Rigidbody2D rb2;

    [SerializeField] private float speed;

    private float _inputX;
    private float _inputY;
    
    private Vector2 _moveVector;
    
    // 动画部分
    [SerializeField] private Animator animator;
    private bool _isMoving;
    
    public void Update()
    {
        GetInput();
        SwitchAnimation();
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
        _isMoving = _moveVector != Vector2.zero;
    }
    
    private void Walk()
    {
        rb2.MovePosition(speed * Time.deltaTime * _moveVector  + rb2.position);
    }
    
    private void SwitchAnimation()
    {
        animator.SetBool("Moving", _isMoving);
        if (_isMoving)
        {
            animator.SetFloat("InputX", _inputX);
            animator.SetFloat("InputY", _inputY);
        }
    }
}
```

最后点击运行Unity的按钮，可以看到人物动画效果已经能够成功切换
