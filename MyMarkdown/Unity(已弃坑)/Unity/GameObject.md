# GameObject

游戏对象类，Unity的所有游戏对象都是由游戏对象类实例化而来。

## 游戏对象的创建

- 通过GameObject构造函数创建游戏对象，生成一个空的游戏对象，如：`GameObject go = new GameObject(”GO”)；`

- 通过CreatePrimitive方法创建原始的游戏**模型**，如：
  
       `GameObject.CreatePrimitive(PrimitiveType.Cube)`

- 通过Instantiate方法实例化创建游戏对象
  
  - 克隆预制体
  
  - 克隆普通的游戏对象
    
    如：`Instantiate(Prefabs,transform.position,transform.rotation)`
    
    第三种方法是最常用的
    
    ```csharp
    public GameObject cubePrefab;
    Vector 3 pos = new Vector3(-1f,-2f,0f);
    GameObject go = Instantiate(cubePrefab,tpos,Quaternion.identity) as GameObject
    ```

这里我通过设置一个GameObject对象作为函数的参数，并且把位置设置位pos，然后设置一个自身的旋转(即不旋转)。

然后我们如果需要保存这样的一个游戏对象，则需要进行一个类型的转换，因为Instantiate函数返回的是一个Object，而不是GameObject，所以这里使用了as关键字来进行类型的转换。

## 游戏对象的销毁

- 现在销毁一个游戏对象`GameObject.Destroy(Object)`
- 延迟销毁一个游戏对象`GameObject.Destroy(Object,float)` 其中的float表示的是多少秒后销毁
- 销毁游戏对象时将销毁该物体所有的组件和子物体

## 游戏对象的查找

### Find方法

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class new_GameObject : MonoBehaviour
{
    public GameObject CudeObject;
    public GameObject sphereObject;
    public GameObject cylinderObject;
    // Start is called before the first frame update
    void Start()
    {
        CudeObject = GameObject.Find("Cube");
        sphereObject = GameObject.Find("Sphere");
        cylinderObject = GameObject.Find("Cylinder");
        Debug.Log(CudeObject.name);
        Debug.Log(sphereObject.name);
        Debug.Log(cylinderObject.name);
    }

    // Update is called once per frame
    void Update()
    {

    }
}
```

Find方法，查找**当前场景下**是否存在目标对象，并返回一个GameObject对象。

Cude对应着立方体

Sphere对应着球体

Cylinder对应着圆柱体

## FindWithTag

其实和Find大差不差

![Untitled](E:\MyMarkdown\Unity\Unity\GameObject\Untitled.png)

标签的添加方式看上图。

### 子物体中查找游戏对象

`transfrom.Find("G1/G11")` 在当前对象的子物体中查找，必须指明具体的查找路径，这里表示的G1/G11指的是当前对象的子物体G1中的子物体G11。

### 通过父物体进行查找

`GameObject.Find("Cube1").transform.GetChild(0)` 此处的0代表的是第一个子物体，返回的是一个transform类型

注意，当游戏物体未激活时，我们使用Find和FindWithTag是找不到的。但是通过父物体或者直接在子物体中查找是可以查找到的。

## 游戏物体的启用和禁用

(以按下D键启用游戏物体)

```csharp
if (Input.GetKey(KeyCode.D){
            cube.SetActive(true);
        }
```

当然，我们把true切换为false的话，那样就是禁用这个游戏物体。

## 访问游戏物体的激活状态

![Untitled](E:\MyMarkdown\Unity\Unity\GameObject\Untitled%201.png)

## 游戏对象组件的访问与控制（参数和启用以及禁用）

仍然是使用cube作为例子。

```csharp
//获取当前游戏物体上的一个组件
BoxCollider box = gameObject.GetComponent<BoxCollider>();
//组件上的size属性扩大三倍
box.size *= 3;
```

获取当前游戏物体上的脚本组件

仍然是使用gameObject上的GetCompnent方法

我们这里假设这个脚本的名字为test

```csharp
test c = gameObject.Getcomponent<test>();
```

如果我们要调用c里面的方法，或者属性的话，和调用对象里面的属性，方法是一样的。

```csharp
//如
c.Print();
```

调用其他游戏物体中的脚本组件

假设我们现在有另一个cube，名为cube1

他的脚本是这样的，DO方法用于将其材质的颜色修改为红色。

```csharp
public class cube1 : MonoBehaviour
{
    public void DO()
    {
       gameObject.GetComponent<MeshRenderer>().material.color = Color.red;
    }
}
```

现在我们回到cube游戏物体中来

```csharp
//定义一个另外的游戏对象
public GameObject other;
//获取名为cube1的脚本组件对象，命名为c2
cube1 c2 = other.GetComponent<cube1>();
//这时候我们就可以调用DO了
c2.DO();
```

## 游戏物体的移动

现在我们创建了一个这样的场景，我们想让cube移动。

1. 第一种方法是改变position信息，让它在某个方向上有一个增量

例如：在Update中 `transform.position += new Vector3(Time.deltaTime * 2, 0, 0);` 

1. 第二种方法是`transform.Translate` 方法
   
    例如：在Update中`transform.Translate(Vector3.right * Time.deltaTime*2f)` 这里其实是按着此物体的右方来移动的。如果我们想改为按着世界坐标系的右方来移动的话，那么我们就需要加入第二个参数`transform.Translate(Vector3.right * Time.deltaTime*2f,Space.World)` 

2. 第三种方法是使用Vector3.Lerp

这里Lerp方法中，第一个参数是起始位置，第二个参数是目标位置，第三个参数是插值。

`Vector3 target = new Vector3(3.0f, 0, -4f);`

`transform.position = Vector3.Lerp(transform.position, target, Time.deltaTime);`

1. 第四种方法是通过Vector3.MoveTowards进行一个匀速运动,这个方法会返回一个Vector3。

`Vector3.MoveTowards(Vector3 current,Vector3 target,float maxDistanceDelta)`

1. 我们可以为四个键设置一个Input，然后在四个if中插入以上三种的移动方法，在按下每个键时实现游戏物体的相关移动
2. 第六种方法其实是第五种方法的一个简介版本。我们通过GetAxis来获取当前物体的水平轴以及垂直轴的一个方向（虚拟轴），这个方法会返回一个0-1或0- -1之间的一个浮点数。这样我们就可以获得一个移动的方向。当然，我们也可以按下上下左右键

![Untitled](E:\MyMarkdown\Unity\Unity\GameObject\Untitled%202.png)

![Untitled](E:\MyMarkdown\Unity\Unity\GameObject\Untitled%203.png)

我们来看Unity官方对于Lerp的解释，Lerp返回的是一个Vector的插值，而这个值的计算为 a + (b - a) * t。而且不仅仅通过计算式，我们从Unity实际的运行效果来看，物体呈现一个先快后慢，然后不断逼近target（只会逼近，位置不会真的变成target）。

![                          **第六种方法的实现**](E:\MyMarkdown\Unity\Unity\GameObject\Untitled%204.png)

                          **第六种方法的实现**