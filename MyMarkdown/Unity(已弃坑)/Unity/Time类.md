# Time类

Time类中有着许多的静态变量

- Time.time 表示从游戏开发到现在的时间，会随着游戏的暂停而停止计算。
- Time.timeSinceLevelLoad 表示从当前Scene开始到目前为止的时间，也会随着暂停操作而停止。
- **Time.deltaTime** 表示从上一帧到当前帧时间，以秒为单位。又称增量时间
- Time.fixedTime 表示以秒计游戏开始的时间，固定时间以定期间隔更新（相当于fixedDeltaTime）直到达到time属性。
- Time.fixedDeltaTime 表示以秒计间隔，在物理和其他固定帧率进行更新，在Edit->ProjectSettings->Time的Fixed Timestep可以自行设置。（物理引擎和FixedUpdate的更新时间间隔）
- Time.SmoothDeltaTime 表示一个平稳的deltaTime，根据前 N帧的时间加权平均的值。
- Time.timeScale 时间缩放，默认值为1，若设置<1，表示时间减慢，若设置>1,表示时间加快，可以用来加速和减速游戏，非常有用。
- Time.frameCount 总帧数
- Time.realtimeSinceStartup 表示自游戏开始后的总时间，即使暂停也会不断的增加。（不受timeScale的影响）
- Time.captureFramerate 表示设置每秒的帧率，然后不考虑真实时间。
- Time.unscaledDeltaTime 不考虑timescale时候与deltaTime相同，若timescale被设置，则无效。
- Time.unscaledTime 不考虑timescale时候与time相同，若timescale被设置，则无效。

根据描述可以发现有些是支持读写，有些是只能进行读取。

这些变量或者方法可以看此图理解

![Untitled](E:\MyMarkdown\Unity\Unity\Time类\Untitled.png)

## Time.timeScale

游戏时间规模

timeScale与游戏里所有和时间挂钩的事件有关。

当timeScale被设置为0时，将会暂停所有与帧率无关的事件。这些主要是指所有的物理事件和依赖时间的函数、刚体力和速度等，而且 FixedUpdate 会被暂停（不是Update），因为FixedUpdate函数是根据时间来进行更新的。

但是timeScale并不会影响Update的调用。Update的调用仅取决于机器的渲染能力。也就是说，当timeScale = 0时，所有与时间相关的都会暂停（比如与之挂钩的动画与特效），但是Update的调用仍未停止，也就是说机器仍然是在渲染画面的。

也就是说timeScale影响的时游戏里的时间缩放比例。

例如：

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        this.transform.Rotate(0, 10*Time.deltaTime, 0);
        Debug.Log(Time.deltaTime);
    }
    void OnGUI()
    {
        if (GUILayout.Button("Slow", GUILayout.Width(100)))
        {
            Time.timeScale = 0.5f;
        }
        if (GUILayout.Button("Fast", GUILayout.Width(100)))
        {
            Time.timeScale = 2.0f;
        }
    }
}
```

这里有一个脚本，设置了两个GUI按钮，按下Slow就会让时间规模修改为0.5f

按下Fast就会让时间规模修改为2.0f。

## Time.deltaTime

上一帧耗费的时间(只读)。

其实可以用作一个计数器：实现每隔多少秒做什么事情

- 控制物体均匀旋转
- 每秒移动多少米
- 每秒旋转多少度

此处以一个Cube物体的显示和隐藏来作为一个例子

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class cube_show : MonoBehaviour
{
    private float speed = 1.0f;
    private float timer = 3.0f;//作为一个计时时间
    // Start is called before the first frame update
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        timer -= Time.deltaTime;
        if (timer <= 0)
        {
            timer = 3.0f;
            //此处对MeshRenderer组件进行enable的切换
            GetComponent<MeshRenderer>().enabled = !GetComponent<MeshRenderer>().enabled;
        }
        //实现每秒向右走speed
        transform.Translate(Vector3.right * Time.deltaTime * speed);
    }
}
```