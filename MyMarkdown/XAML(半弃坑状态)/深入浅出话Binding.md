## Binding基础

一般来说 Binding的源是逻辑层的对象，Binding的目标是UI层的控件对象，这样数据就会源源不断地通过Binding送达UI层，被UI层展现，也就完成了数据驱动UI的过程。
在这里我们举一个例子，创建一个简单的数据源并通过Binding把它连接到UI元素上。
```C#
class Student
    {
        private String name;
        public String Name
        {
            get { return name; }
            set { name = value; }
        }
    }
```
那么我们如何将Student通过binding与UI元素进行连接的呢。
Binding是一种自动机制，当值变化后属性要有能力通知Binding，让Binding把变化传递给UI元素。我们可以再属性的set语句中激发一个PropertyChanged事件，这个事件不需要我们自己声明，我们要做是，让作为数据源的类实现System.ComponentModel名称空间中的INotifyPropertyChanged接口。
```C#
class Student:INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;
        private String name;
        public String Name
        {
            get { return name; }
            set { 
                name = value; 
                if(this.PropertyChanged != null)
                {
                    this.PropertyChanged.Invoke(this, new PropertyChangedEventArgs("Name"));
                }
            }
        }
    }   
```
当Name属性的值发生变化时ProoertyChanged事件就会被触发，Binding接受到这个事件后发现事件的信息告诉它是名为Name的属性发生了值的变化，于是就会通知Binding目标端的UI元素显示新的值。
在此我们可以在窗体上准备一个TextBox和一个Button。TextBox将作为Binding目标，我们还会在Button的Click事件发生时改变Student对象的Name属性值。
```C#
public partial class MainWindow : Window
    {
        private Student student;

        public MainWindow()
        {
            InitializeComponent();
            //数据源初始化
            student = new Student();
            //准备Binding
            Binding binding = new Binding();
            //为binding指定数据源
            binding.Source = student;
            //为binding指定访问路径
            binding.Path = new PropertyPath("Name");
            //使用Binding连接数据源与Binding目标，三个参数分别表示Binding目标，送达目标的哪个属性，指定哪个Binding实例
            BindingOperations.SetBinding(this.textBoxName, TextBox.TextProperty, binding);
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            student.Name += "Name";
        }
    }
```
实际的效果是，我们每按一下按钮，Name就会增加一个内容为"Name"的字符串，然后更新后的Name会显示在TextBox控件中。
当然也有一种三合一构造器的写法：
```C#
this.textBoxName.SetBinding(TextBox.TextProperty, new Binding("Name") { Source = student = new Student() });
```

Binding模型：
![[Pasted image 20220928153645.png]]

## Binding的源与路径
Binding的源也就是数据的源头。Binding对源的要求并不苛刻——只要是一个对象，并且通过属性公开自己的数据，它就能作为Binding的源。
当然Binding的源并不一定是一个逻辑层的对象，它也有很多种可能。比如：自己或自己的容器或子级元素作为源，用一个控件作为另一个控件的数据源，把集合作为ItemsControl的数据源，使用XML作为TreeView或Menu的数据源，把多个控件关联到一个数据制高点上，甚至干脆不给Binding指定数据源，让它自己去找。

### 控件作为Binding源与Binding标记拓展
目的：让UI元素产生一些联动效果，下面就是把一个TextBox的text属性关联在了Slider的Value属性上
```XAML
<StackPanel>
        <TextBox x:Name="textBox1" Text="{Binding Path = Value,ElementName=slider}" BorderBrush="Black" Margin="5" />
        <Slider x:Name="slider" Maximum="100" Minimum="0" Margin="5" />
    </StackPanel>
```

实际效果：
![[Pasted image 20220928154742.png]]
通过拉动下方的条，上方的TextBox的值会相应的进行变化。
我们来看看XAML中关于binding的声明。
与之对应的XAML代码为
```C#
		this.textBox.SetBinding(TextBox.TextProperty,new Binding("Value"){ElementName = "slider1"});
```

>[TIPS]
>因为在C#代码中我们可以直接访问控件对象，所以一般也不会使用Binding的ElementName属性，而是直接把对象赋值给Binding的Source属性。


### 控制Binding的方向及数据更新
Binding在源与目标之间架起了沟通的桥梁，默认情况下数据既能通过Binding送达目标，也能够从目标返回源。有时候数据只需要展示给用户、不允许用户修改，这时候可以把Binding模式更改为从源向目标的单向沟通。
控制Binding数据流向的属性是Mode，它的类型是BindingMode枚举。可取值为TwoWay、OneWay、OnTime、OneWayToSource和Default。
我们仍然使用上一节的例子，当我们拖动Slider的手柄时，TextBox里就会显示出Slider当前的值；如果我们在TextBox里输入一个恰当的值，然后按下Tab键，让焦点离开TextBox，则Slider的手柄就会跳到相应的值那里。![[Pasted image 20221009141117.png]]
很明显我们可以在XAML中修改Binding的Mode值。
为什么我们需要改变焦点，Slider的值才会变呢？这就引出了Binding的另一个属性UpdateSourceTrigger，它的属性时一个枚举，可以取值为PropertyChanged、LostFocus、Explicit和Default。我们只需要把这个属性更改为PropertyChanged，则Slider的手柄就会随着我们在TextBox里的输入而改变位置。

### Binding的路径
Binding关注的属性由Binding的Path属性来指定。例如在前面的例子中，我们是把Slider控件对象作为源、把它的Value属性作为路径。Path的实际类型是PropertyPath类型。
其中最简单的例子就是直接把Binding关联在Binding源上，前面的例子就是如此。
```XAML
 <TextBox x:Name="textBox1" Text="{Binding Path = Value,ElementName=slider}" BorderBrush="Black" Margin="5" />
``` 

等效的C#代码是
```C#
Binding binding = new Binding(){Path = new PropertyPath("Value"),Source = this.slider1};
this.textBox1.SetBinding(TextBox.TextProperty,binding);
```
或者使用Binding的构造器简写为：
```C#
Binding binding = new Binding(){Path = new PropertyPath("Value"),Source = this.slider};
this.textBox1.SetBinding(TextBox.TextProperty，binding);
```
Binding还支持多级路径。比如我们想让一个TextBox显示另外一个TextBox的文本长度，我们可以这样写：
![[Pasted image 20221009142945.png]]
等效的C#代码是![[Pasted image 20221009143344.png]]
运行的效果是
![[Pasted image 20221009143404.png]]
我们知道记录和类型的索引器又称为带参属性。既然是属性，索引器；也能作为Path来使用，比如我们想让一个TextBox显示另一个TextBox文本的第四个字符，我们可以这样写：
![[Pasted image 20221009143703.png]]

假设我们想要使用一个集合或者说DataView作为Binding的源，并且把它的默认元素当作Path使用，则需要这样的语法：
![[Pasted image 20221009143939.png]]
还有一个斜线的使用方法：
![[Pasted image 20221009144217.png]]
当然不指定List中的哪个元素的话，仍然是默认第一个元素。

### 没有Path的Binding
有的时候我们会在代码中看到一些Path是一个“.”，或者干脆没有Path的Binding，这其实是一种特殊情况——Binding源本身就是数据且不需要Path来证明。

### 为Binding指定源（Source）的几种方法

Binding的源是数据的来源，所以，只要一个对象包含数据并能通过属性把数据暴露出来，它就能当作Binding的源来使用。包含数据的元素比比皆是，但必须为Binding的Source指定合适的对象Binding才能正确工作。常见的方法有：
![[Pasted image 20221009145105.png]]

Binding源的设置看书P109


