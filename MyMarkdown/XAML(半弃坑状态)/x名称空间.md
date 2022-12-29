2 ## 4.1x名称空间里都有什么
x名称空间映射的是http://schemas.microsoft.com/winfx/2006/xaml,它包含的类均与解析XAML语言相关，因此也可以称之为XAML名称空间。
x名称空间中包含的工具入下：
| 名称            | 种类（在XAML中的形式） |
| --------------- | ---------------------- |
| x:Array         | 标签拓展               |
| x:Class         | 属性                   |
| x:ClassModifier | 属性                   |
| x:Code          | XAML指令元素           |
| x:FieldModifier | 属性                   |
| x:Key           | 属性                   |
| x:Name          | 属性                   |
| x:Null          | 标签拓展               |
| x:Shared        | 属性                   |
| x:Static        | 标签拓展               |
| x:Subclass      | 属性                   |
| x:Type          | 标签拓展               |
| x:TypeArguments | 属性                   |
| x:Uid           | 属性                   |
| x:XData         | XAML指令元素           |

## 4.2x名称空间中的属性
1.x:Class
	这个属性的作用是告诉XAML编译器将XAML标签的编译结果与后台代码中指定的类合并。
>[!tips]
>1. 这个属性只能用于根结点
>2. 使用这个属性的根节点的类型要与x:Class的值所指示的类型保持一致
>3. x:Class的值所指示的类型在声明时必须使用partial关键字

2.x:ClassModifier
	这个属性的作用时告诉XAML编译由标签编译生成的类具有怎样的访问控制级别
>[!Tips]
>1. 使用时标签必须具有x:Class Attribute
>2. x:ClassModifier的值必须与x:Class所指示类的访问控制级别一致
>3.  x:ClassModifier的值随后台代码的编译语言不同而有所不同。

3.x:Name
	我们知道XAML是一种声明式语言。XAML标签声明的是对象，一个XAML标签对应着一个对象，这个对象一般是一个控件类的实例。而在.NET上类是引用类型。如果我们需要为对象准备一个引用对象以便在C#代码中直接访问就必须显式地告诉XAML编辑器——为这个对象声明引用变量，这时候x:Name就派上用场了。
这里我们使用一段XAML代码作为例子
```XAML
<StackPanel>
	<TextBox Margin = "5"/>
	<Button Content = "OK" Margin = "5" Click = "Button_Click"/>
	</StackPanel>
```
对应的事件处理器：
```C#
private void Button_Click(object sender,RoutedEventArgs e)P{
	StackPanel stackPanel = this.Content as StackPanel;
	TextBox textBox = stackPanel.Children[0] as TextBox;
	if(string.IsNullOrEmpty(textBox.Name)){
	textBox.Text = "No name!";
	}
	else{
	textBox.Text = textBox.Name;
	}
}
```
可以看到C#中需要映射到XAML中的过程是非常繁琐的。那么这时候x:Name属性就派上用场了。
>[!tips]
>x:Name的作用有两个
>1. 告诉XAML编译器，当有一个标签带有这个属性时除了为这个标签生成对应实例外还要为这个实例声明一个引用变量，变量名就是x:Name的值
>2. 将XAML标签所对应对象的Name属性(如果有)也设为x:Name的值，并把这个值注册到UI树上，以方便寻找

这里我们把上面的XAML代码修改一下：
```XAML
<StackPanel>
	<TextBox x:Name="textBox" Margin = "5"/>
	<Button Content = "OK" Margin = "5" Click = "Button_Click"/>
	</StackPanel>
```
这样，在事件处理器中：
```C#
private void Button_Click(object sender,RoutedEventArgs e)P{
	
	//StackPanel stackPanel = this.Content as StackPanel;
	//TextBox textBox = stackPanel.Children[0] as TextBox;
	if(string.IsNullOrEmpty(textBox.Name)){
	textBox.Text = "No name!";
	}
	else{
	textBox.Text = textBox.Name;
	}
}
```

4.x:FieldModifier
	  用于限定引用变量的访问级别。在默认情况下，这些类的字段的访问级别按照面向对象的封装原则，都是internal，在编程的时候我们需要把被访问控件的引用变量改为public级别。
>[!tips]
>因为x:FieldModifier是用来改变引用变量访问级别的，所以使用x:FieldModifier的前提是这个标签同时也使用x:Name。

5.x:Key
	在XAML中我们可以把很多需要多次使用的内容提取出来放在资源字典里，需要使用这个资源的时候我们就把它的Key检索出来。
	x:Key的作用就是为资源贴上用于检索的索引。在WPF中几乎每个元素都有自己的Resource属性，这个属性是Key-Value的集合，只要把这个元素放进这个集合，这个元素就成为资源字典中的一个条目。![[Pasted image 20220831151529.png]]
	key不但在XAML中可以使用，在C#中的相关函数也可以使用。

## 4.3 x名称空间中的标记拓展
	标记拓展实际上就是一些MarkupExtension类的直接或间接派生类。x名称空间中就包含有一些这样的类

1. x:Type
x:Type的值是一个数据类型的名称。常常用来给其他数据类型的对象赋值。
2. x:Null
显式地对一个属性赋一个空值。
### 4.3.3标记扩展实例的另外一种声明语法。
我们除了可以这么写标记拓展：
```XAML
<Button Content="OK" Style="{x:Null}"/>
```

也可以这样写：
```XAML
<Button Content="OK" >
	<Button.Style>
		<x:Null/>
	</Button.Style>
</Button>
```

但是第二种写法因为比较的繁琐，因此很少使用，但是在x:Array中我们必须使用标签式声明。

3. x:Array
x:Array的作用是通过它的Items属性向使用者暴露一个类型已知的ArrayList实例，ArrayList内成员的类型由x:Array的Type指明。
![[Pasted image 20220901085812.png]]
![[Pasted image 20220901085834.png]]

4. x:Static
x:Static是一个很常用的标记拓展，它的功能是在XAML文档中使用数据类型的static成员。![[Pasted image 20220901090017.png]]
![[Pasted image 20220901090033.png]]

## 4.4XAML指令元素
XAML指令元素只有两个
* x:Code
* x:XData
