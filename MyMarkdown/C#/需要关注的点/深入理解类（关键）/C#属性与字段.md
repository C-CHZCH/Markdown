# C#属性与字段

## 属性

属性是一种成员，它提供灵活的机制来读取、写入或计算私有字段的值。属性可用作公共数据成员，但它们实际上是称为*访问器*的特殊方法。这使得可以轻松访问数据，还有助于提高方法的安全性和灵活性。

## 字段

字段是在[类](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/class)或[结构](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/struct)中直接声明的任意类型的变量。字段是其包含类型的成员。

**字段**就是类和结构中声明的变量。

**属性**这是一种成员，可以设置它的读取、写入或计算的机制。

举个例子：

描述一个人的信息

【生日】抽象为字段比较合适，因为不需要额外的计算。

【年龄】抽象为属性比较合适，因为需要【生日】和【当前时间】进行计算，而且具有只读特性

最直观的区别是，属性可以单独设置读/写的权限，而字段不能。比如

```csharp
public int Prop { get; private set; } 
 // public get, private set
```

复杂一些，属性的读写可以是一段逻辑。比如

```csharp
public int Prop 
{
get {  do something; return member; } 
set { do something; member = value; } 
}
```

## 属性示例

1. 属性本身并没有任何的存储。取而代之，访问器决定如何处理发送过来的数据，以及应将什么数据发送出去。在这种情况下，属性会使用一个字段作为存储
2. set访问器接受输入的参数value，并把它的值赋给字段
3. get访问器只是返回字段的值

## 属性与关联字段

属性常和字段关联，一种方式是将字段声明为private以封装字段，并声明一个public属性来控制从类的外部对该字段的访问。和属性关联的字段常被称为后备字段或后备存储

```csharp
class C1{
private int example;
public int Example{
get{return example;}
set{example = this.value}
}
```

属性与关联字段存在着一种命名约定：

![Untitled](E:\MyMarkdown\C%23需要关注的点\C%23需要关注的点\深入理解类（关键）\C%23属性与字段\Untitled.png)

关联字段为小驼峰命名法，第一个单词以小写开头，第二个单词开始以大写开头，且单词之间不使用空格，连接线，下划线等分割。

属性则使用大驼峰命名法。

## 属性与公有字段Public

1. 属性是函数成员而不是数据成员，允许你处理输入和输出，而公有字段不行。
2. 属性可以只读或只写
3. 编译后的变量与编译后的属性语义不同

## 自动实现属性

![Untitled](E:\MyMarkdown\C%23需要关注的点\C%23需要关注的点\深入理解类（关键）\C%23属性与字段\Untitled%201.png)