# input类

## 相关方法

- Input.GetKey()
  
    按下某个键之后，持续的返回true

- Input.GetKeyDown()
  
    按下某键的一瞬间返回true

- Input.GetKeyUp()
  
    抬起某键的一瞬间返回true

这三个方法的参数都是KeyCode这一个枚举，KeyCode键码保存了物理键盘按键索引码。

```csharp
void Update()
    {
        if (Input.GetKey(KeyCode.A))
        {
            Debug.Log("GetKey");
        }
        else if (Input.GetKeyDown(KeyCode.S))
        {
            Debug.Log("GetKey");
        }
        else if (Input.GetKeyUp(KeyCode.D))
        {
            Debug.Log("GetKey");
        }
    }
```

![                       对于space键的判定图](E:\MyMarkdown\Unity%20C%23脚本基础\Unity%20C%23脚本基础\input类\Untitled.png)

                       对于space键的判定图

其实这个和Mouse的检测（鼠标）是类似的。

不同的是Mouse对于左右键的判断是按照0，1，2

具体可看以下方法。

```csharp
if (Input.GetMouseButtonDown(0))
        {
            Debug.Log("left Down");
        }
        else if (Input.GetMouseButtonDown(1))
        {
            Debug.Log("Right Down");
        }
        else if (Input.GetMouseButton(0))
        {
            Debug.Log("continous Left Mouse");
        }
        else if (Input.GetMouseButton(1))
        {
            Debug.Log("continous Right Mouse");
        }
```

当然还有Up的方法，这里没有写出来。

其中的参数0，1，2，三者分别对应的是左键，右键，中键。