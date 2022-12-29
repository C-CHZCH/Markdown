# C#需要关注的点

1. visual studio 在创建一个项目的时候会给你内嵌与你选择的项目类型相关的框架。例如：

在Console项目中这样写：

```csharp
namespace HelloWorld
{
    public class Program
    {  
        static void Main(string[] args)
        {
            Console.WriteLine("HelloWorld");
        }
    }
}
```

是没有问题的。(没有使用using System 就可以使用Console的相关方法)

这时如果我们查看其嵌入的框架可以发现其实visual studio是帮我们内嵌了System这个名称空间的。

![[1.png]]

[第一个C#程序](需要关注的点/第一个程序.md)

[类型存储与变量](需要关注的点/类型存储与变量.md)

[类的概述](需要关注的点\类的概述.md)

[方法](需要关注的点/方法.md)

[深入理解类（关键）](需要关注的点/深入理解类（关键）.md)

[数组](需要关注的点/数组.md)

[委托类型](需要关注的点/委托类型.md)

![[Pasted image 20220721094044.png]]