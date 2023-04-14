# Unity2D开发（09）场景切换

## 1.创建场景House

复制一份Template，改名为02.House，并将场景拖拽到Hierarchy窗口下

右键点击场景可以通过Load Scene和Unload Scene设置场景的加载

将01.Courtyard设置为Unload，将02.House设置为Load

![01.创建场景House](https://github.com/sfresurgam/unity-development-doc/blob/main/09.Scene%20Switch/source%20image/01.%E5%88%9B%E5%BB%BA%E5%9C%BA%E6%99%AFHouse.png)



绘制好的室内场景如下

![01.第二场景House](https://github.com/sfresurgam/unity-development-doc/blob/main/09.Scene%20Switch/source%20image/01.%E7%AC%AC%E4%BA%8C%E5%9C%BA%E6%99%AFHouse.png)

## 2.初始化脚本T Manager

点击【File】-【Build Setting】-【Add Open Scenes】将场景全部添加进来

![03.添加场景到Build Setting](https://github.com/sfresurgam/unity-development-doc/blob/main/09.Scene%20Switch/source%20image/03.%E6%B7%BB%E5%8A%A0%E5%9C%BA%E6%99%AF%E5%88%B0Build%20Setting.png)

在Persistent Scene中创建空的GameObject命名为Transition Manager

在【Scripts】文件夹内创建文件夹【Transition】，创建脚本Transition Manager.cs，并绑定到Hierarchy的Transition Manager上

```c#
using System.Collections;
using UnityEngine;
using UnityEngine.SceneManagement;

namespace Transition
{
    public class TransitionManager : MonoBehaviour
    {

        [Header("默认场景")] [SerializeField] private string startSceneName = string.Empty;

        private void Start()
        {
            StartCoroutine(LoadSceneSetActive(startSceneName));
        }

        private IEnumerator LoadSceneSetActive(string sceneName)
        {
            yield return SceneManager.LoadSceneAsync(sceneName, LoadSceneMode.Additive);

            var newScene = SceneManager.GetSceneAt(SceneManager.sceneCount - 1);

            SceneManager.SetActiveScene(newScene);
        }
    }
}
```

这里以协程的方式进行场景的加载，在游戏刚开始时加载定义好的场

比如我们将01.Courtyard和02.House都切换为Unload状态，然后设置startSceneName为01.Courtyard，点击Play运行游戏

![04.unload场景](https://github.com/sfresurgam/unity-development-doc/blob/main/09.Scene%20Switch/source%20image/04.unload%E5%9C%BA%E6%99%AF.png)

![05.测试加载场景](https://github.com/sfresurgam/unity-development-doc/blob/main/09.Scene%20Switch/source%20image/05.%E6%B5%8B%E8%AF%95%E5%8A%A0%E8%BD%BD%E5%9C%BA%E6%99%AF.png)

可以看到游戏启动后成功加载了场景01.Courtyard

## 3.场景切换

在场景House中添加【GameObject】并取名为PortalToCourtyard，意思是传送至场景Courtyard的通道，并为它添加碰撞器BoxCollider2D，放置在门口的位置，**注意要勾选Is Trigger**（图中忘记勾选了）

![06.创建PortalToCourtyard](https://github.com/sfresurgam/unity-development-doc/blob/main/09.Scene%20Switch/source%20image/06.%E5%88%9B%E5%BB%BAPortalToCourtyard.png)

创建脚本Portal.cs，挂载到PortalToCourtyard上

```c#
using UnityEngine;

namespace Transition
{
    public class Portal : MonoBehaviour
    {
        [Header("传送目的地")] [SerializeField] private string sceneToGo;
        [Header("传送目的地坐标")] [SerializeField] private Vector3 scenePositionToGo;

        private void OnTriggerEnter2D(Collider2D col)
        {
            // 注意要把Player的标签选择为"Player"
            if (col.CompareTag("Player"))
            {
                // 使用EventHandler
            }
        }
    }
}
```

这里我们需要用到事件委托中心，EventHandler。事件的发布和订阅等都需要用到委托.通过委托可以很好的实现类之间的解耦合

每次呼叫事件中心的函数，所有注册到事件中心的类都会执行相应的代码

在文件夹【Utils】创建脚本EventHandler.cs

```c#
using System;
using UnityEngine;

namespace Utils
{
    public static class EventHandler
    {
        // 传送门
        public static event Action<string, Vector3> TransitionEvent;

        public static void CallTransitionEvent(string sceneToGo, Vector3 scenePositionToGo)
        {
            TransitionEvent?.Invoke(sceneToGo, scenePositionToGo);
        }
    }
}
```

其中`?.`代表先判断TransitionEvent是否为空，不为空的话执行Invoke

然后在Portal.cs中调用CallTransitionEvent

```c#
using UnityEngine;
using Utils;

namespace Transition
{
    public class Portal : MonoBehaviour
    {
        [Header("传送目的地")] [SerializeField] private string sceneToGo;
        [Header("传送目的地坐标")] [SerializeField] private Vector3 scenePositionToGo;

        private void OnTriggerEnter2D(Collider2D col)
        {
            // 注意要把Player的标签选择为"Player"
            if (col.CompareTag("Player"))
            {
                // 使用EventHandler
                EventHandler.CallTransitionEvent(sceneToGo, scenePositionToGo);
            }
        }
    }
}
```

返回Unity，在PortalToCourtyard上修改传送地点和坐标（坐标设置为01.Courtyard的帐篷门口位置）

![07.修改传送地点和坐标](https://github.com/sfresurgam/unity-development-doc/blob/main/09.Scene%20Switch/source%20image/07.%E4%BF%AE%E6%94%B9%E4%BC%A0%E9%80%81%E5%9C%B0%E7%82%B9%E5%92%8C%E5%9D%90%E6%A0%87.png)

回到TransitionManager，添加传送事件的代码，注册到EventHandler上

```c#
private void OnEnable()
{
    EventHandler.TransitionEvent += OnTransitionEvent;
}

private void OnDisable()
{
    EventHandler.TransitionEvent -= OnTransitionEvent;
}

private void OnTransitionEvent(string sceneToGo, Vector3 scenePostionToGo)
{
    StartCoroutine(SwitchScene(sceneToGo, scenePostionToGo));
}

private IEnumerator SwitchScene(string sceneName, Vector3 position)
{
    // 卸载当前激活的场景
    yield return SceneManager.UnloadSceneAsync(SceneManager.GetActiveScene());
	// 加载新场景
    yield return LoadSceneSetActive(sceneName);
}
```

在点击运行之前，将两个场景都卸载，然后在TransitionManager上设置默认场景为02.House

点击Play，将人物走到门口的位置，可以看到成功传送到了01.Courtyard

但是现在出现了2个问题：1、位置还没有设置；2、无法绑定边界。接下来处理这两个问题

## 4.绑定边界

在EventHandler上新注册两个事件BeforeSceneUnloadEvent和AfterSceneloadEvent

```c#
public static event Action BeforeSceneUnloadEvent;
public static void CallBeforeSceneUnloadEvent()
{
    BeforeSceneUnloadEvent?.Invoke();
}

public static event Action AfterSceneloadEvent;
public static void CallAfterSceneloadEvent()
{
    AfterSceneloadEvent?.Invoke();
}
```

TransitionManager.cs代码中，在切换新旧场景的前后，需要执行边界的绑定

修改SwitchScene方法

```c#
private IEnumerator SwitchScene(string sceneName, Vector3 position)
{

    // 卸载旧场景前的准备
    EventHandler.CallBeforeSceneUnloadEvent();
    // 卸载当前激活的场景
    yield return SceneManager.UnloadSceneAsync(SceneManager.GetActiveScene());
    // 加载新场景
    yield return LoadSceneSetActive(sceneName);
    // 加载新场景后的准备
    EventHandler.CallAfterSceneloadEvent();
}
```

修改SwitchBoundsUtil中绑定边界的代码

```c#
using Cinemachine;
using UnityEngine;

namespace Utils
{
    public class SwitchBoundsUtil : MonoBehaviour
    {
        private void OnEnable()
        {
            EventHandler.AfterSceneloadEvent += SwitchBoundsConfiner;
        }

        private void OnDisable()
        {
            EventHandler.AfterSceneloadEvent -= SwitchBoundsConfiner;
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

再次运行游戏，切换场景时，控制台不再提示找不到边界的空指针异常

## 5.绑定位置

在EventHandler上新注册事件BeforeSceneUnloadEvent

```c#
public static event Action<Vector3> PortalToPositionEvent;

public static void CallPortalToPositionEvent(Vector3 pos)
{
    PortalToPositionEvent?.Invoke(pos);
}
```

在TransitionManager.cs的SwitchScene方法中执行这个事件

```c#
private IEnumerator SwitchScene(string sceneName, Vector3 position)
{
    // 卸载旧场景前的准备
    EventHandler.CallBeforeSceneUnloadEvent();
    // 卸载当前激活的场景
    yield return SceneManager.UnloadSceneAsync(SceneManager.GetActiveScene());
    // 加载新场景
    yield return LoadSceneSetActive(sceneName);
    // 传送人物坐标
    EventHandler.CallPortalToPositionEvent(position);
    // 加载新场景后的准备
    EventHandler.CallAfterSceneloadEvent();
}
```

回到PlayerController.cs，在场景切换时，应该禁用人物的输入，所以添加一个布尔值_inputDisable

在_inputDisable为false的情况下，才允许用户进行人物的操作

```c#
private bool _inputDisable;

private void Update()
{
    if (!_inputDisable)
    {
        GetInput();
    }
    SwitchAnimation();
}
```

添加三个事件BeforeSceneUnloadEvent、AfterSceneloadEvent和PortalToPositionEvent

```c#
private void OnEnable()
{
    EventHandler.BeforeSceneUnloadEvent += OnBeforeSceneUnloadEvent;
    EventHandler.AfterSceneloadEvent += OnAfterSceneloadEvent;
    EventHandler.PortalToPositionEvent += OnPortalToPositionEvent;
}

private void OnDisable()
{
    EventHandler.BeforeSceneUnloadEvent -= OnBeforeSceneUnloadEvent;
    EventHandler.AfterSceneloadEvent -= OnAfterSceneloadEvent;
    EventHandler.PortalToPositionEvent -= OnPortalToPositionEvent;
}
```

函数OnBeforeSceneUnloadEvent代表进行场景切换时，禁用人物的输入状态

```c#
private void OnBeforeSceneUnloadEvent()
{
    _inputDisable = true;
}
```

函数OnAfterSceneloadEvent代表进行场景切换时，恢复人物的输入状态

```c#
private void OnAfterSceneloadEvent()
{
    _inputDisable = false;
}
```

函数OnPortalToPositionEvent代表进行场景切换时，获取到目标场景的位置信息并赋值给人物

```c#
private void OnPortalToPositionEvent(Vector3 pos)
{
    transform.position = pos;
}
```

返回Unity进行测试，当人物走到门口后，能够成功切换到Courtyard的帐篷外，且坐标与设置的一致

传送前

![08.传送前](https://github.com/sfresurgam/unity-development-doc/blob/main/09.Scene%20Switch/source%20image/08.%E4%BC%A0%E9%80%81%E5%89%8D.png)

传送后

![09.传送后](https://github.com/sfresurgam/unity-development-doc/blob/main/09.Scene%20Switch/source%20image/09.%E4%BC%A0%E9%80%81%E5%90%8E.png)

从Courtyard到House也同样按照这个流程来即可

