# get与set

# **C#中类的属性访问器--get和set分析**

（加强对属性读取与写入的控制）

**解释一：**

属性的访问器包含与获取（读取或计算）或设置（写）属性有关的可执行语句。访问器声明可以包含 get 访问器或 set 访问器，或者两者均包含。声明采用下列形式之一：

```csharp
get
{
    ...
}
set
{
    .
}
```

### get 访问器

### get 访问器体与方法体相似。它必须返回属性类型的值。执行 get 访问器相当于读取字段的值。以下是返回私有字段 name 的值的 get 访问器：

```csharp
class Employee
{
    private string name;//私有成员变量，the name fieldpublic string Name//成员属性(即对外公开的成员变量)，the Name property
    {
       get
       {
          return name;
       }
    }
    ...
}
```

当引用属性时，除非该属性为赋值目标，否则将调用 get 访问器读取该属性的值。例如：

```csharp
Employee e1 = new Employee();
...
Console.Write(e1.Name);// The get accessor is invoked here
```

get 访问器必须在 return 或 throw 语句中终止，并且控制不能超出访问器体。

### set 访问器

### set 访问器与返回 void 的方法类似。它使用一个标识名称为value 的隐式参数，此参数的类型与属性的类型一致。在下例中，set 访问器被添加到 Name 属性：

```csharp
class Employee
{
    private string name;//私有成员变量，the name fieldpublic string Name//成员属性(即对外公开的成员变量)，the Name property
    {
       get
       {
           return name;
       }
       set
       {
           name = value;
       }
    }
}
```

当对属性赋值时，用提供新值的参数调用 set 访问器。例如：

```csharp
e1.Name = "Joe";// The set accessor is invoked here
```

在 set 访问器中不能再声明或者定义一个名为value的局部变量，因为隐式参数名称已经使用了value这一标识名称。

备注：属性按如下方式，根据所使用的访问器进行分类：只带有 get 访问器的属性称为只读属性。无法对只读属性赋值。 只带有 set 访问器的属性称为只写属性。只写属性除作为赋值的目标外，无法对其进行引用。 同时带有 get 和 set 访问器的属性为读写属性。 在属性声明中，get 和 set 访问器都必须在属性体的内部声明。

使用 get 访问器更改对象的状态是一种错误的编程样式。例如，以下访问器在每次访问 number 字段时都产生更改对象状态的副作用。

```csharp
public int Number
{
   get
   {
      return number++;//错误，在get访问器中不能修改number的值
   }
}
```

可以将 get 访问器用于返回字段值，或用于计算字段值并将其返回。例如：

```csharp
public string Name
{
   get
   {
      return name != null ? name : "NA";
   }
}
```

在上述代码段中，如果没有对 Name 属性赋值，它将返回值 NA。

如何访问基类中被派生类中具有同一名称的另一个属性隐藏的属性，举例说明：

```csharp
// property_hiding.cs// Property hidingusing System;
public class BaseClass
{
   private string name;
   public string Name
   {
      get
      {
         return name;
      }
      set
      {
         name = value;//value为隐形参数
      }
   }
}

public class DerivedClass : BaseClass
{
   private string name;
   public new string Name// 派生类中定义一个与基类中同名的属性，注意使用了new修饰符
   {
      get
      {
         return name;
      }
      set
      {
         name = value;
      }
   }
}

public class MainClass
{
   public static void Main()
   {
      DerivedClass d1 = new DerivedClass();
      d1.Name = "John";// Derived class property
      Console.WriteLine("Name in the derived class is: {0}",d1.Name);
      ((BaseClass)d1).Name = "Mary";// Base class property
      Console.WriteLine("Name in the base class is: {0}",((BaseClass)d1).Name);
   }
}
```

输出结果为：

```csharp
Name in the derived class is: John
Name in the base class is: Mary
```

以下是上例中显示的重点： 派生类中的属性 Name 隐藏基类中的属性 Name。在这种情况下，派生类的该属性声明使用 new 修饰符：

```csharp
   public new string Name
   {
      ...
//转换 (BaseClass) 用于访问基类中的隐藏属性：
      ((BaseClass)d1).Name = "Mary";
      ...
   }
```

### **解释二：**

代码如下：

```csharp
public class Car
{
  private string color;
  public string Color
  {
    get
    {
      return color;
    }
    set
    {
      color=value;
    }
  }
}
```

理解：通过get和set对公有变量即属性Color进行读写操作，实际就是间接更改color私有变量的值。那既然如此，为何不设color为public，让实例进接对color进行读写操作呢？如果有一天，老板让你把这个类改成：当汽车的颜色改变时，同时计算一下汽车的《价格》属性，那么如果直接对Color操作，你不是死定了? “属性”是.net的特色之一。其实就相当于方法，尤其是java中经常会用到get、set方法（.net的有些思想就是java的）。 属性的真实作用不只是为了更改某个成员变量的值，还可以对私有变量进行条件判断、安全性检查、权限控制等操作。比如form的size属性在set的同时要重画form，如果你不想让用户对color修改，就不要提供set方法。

set and get是面向对象具有的，它的用途一般是对类里面的变量进行操作，而不是直接对类的变量进行操作。它有一个很大的作用就是：便于维护。因为如果一个类的一个变量int a，在其它包或命名空间类中使用了1000次,但是过了许久，你想把a改为b，如果直接对变量a操作的话，就得需求修改整个程序的1000处。  如果用属性了就不会了，属性名称可以不变只须修改get和set访问器代码即可。

```csharp
public int A
{
 set
 {
   a = value;
 }
 get
 {
   return a;
 }
}
修改为:
public int A
{
 set
 {
   b = value;
 }
 get
 {
   return b;
 }
}
```

除去这个属性之外的地方根本不需要改变。 通过上面的讲解要明白一点，get和set访问器就是用来改变类中的私有变量，而不能让实例直接操作，像上面的代码保证了color属性的安全性。既然如此可不可以写成

```csharp
set
{
    color=value*20;//value是不是相当于Color的值
}
```

我当初和你有一样的想法．但是现在改变了。举个例子说明一下：

```csharp
public class Car
{
  public string Color
  {
    get
    {
      if（this.viewstate["color"]!= null)
      {
        return this.viewstate["color"];
      }
      return "":
    }
    set
    {
      this.viewstate["color"]=value;
    }
  }
}
```

在asp.net中通常这么使用，如果用变量的话就不好使用了。而且get，set中可以写多个语句，如上的get。