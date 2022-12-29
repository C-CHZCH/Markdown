# Java程序基本设计框架

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("hello,world");
    }
}
```

来看这一段代码。

public代表的是访问修饰符，类似于C++中类的访问限制。

同理这里的class也是代表Java类，类是构建所有Java程序和applet的构建块。

Java程序的所有内容都必须放置于类中。

这里的Helloworld代表着类名。(类名不可用Java关键字，这点于C++一致)。标准的命名规范为：类名是大写字母开头的名词，如果类名由多个单词组成，则每个单词的首字母都需要大写，例如此处的HelloWorld。（骆驼命名法）

源代码的文件名必须与公共类的名字相同，并且以.Java作为拓展名。（大小写非常重要）

**每一个源文件中都需要有一个main方法，而main方法必须声明为public**

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled.png)

目光转向

```java
 System.out.println("hello,world");
```

这里调用了System.out类中的println方法。与C++，Python相同，Java调用方法的通用方法如下

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%201.png)

Java函数参数方面与C保持一致。

### Java注释

与C++/C保持一致（//）

### Java数据类型

大致与C/C++相同，下方仅仅说明不同点

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%202.png)

### Java变量与常量

声明一个变量：

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%203.png)

（分号是必须的）

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%204.png)

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%205.png)

在声明一个变量之后必须对变量进行显性的初始化。

Java种不可使用未进行初始化的变量

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%206.png)

常量：

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%207.png)

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%208.png)

### 枚举

枚举与C++保持一致，不再阐述。

### 数学函数的使用

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%209.png)

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2010.png)

PS:

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2011.png)

### Java关系运算符，以及结合赋值，自加自减，强制类型转换，类型转换

这部分与C++保持一致

### Java字符串部分与C++部分类似，在有需求使用字符串函数时，自行查找

## 标准输出与输入

### 读取输入

首先构建一个Scanner对象

首先需要引入java.util包。因为Scanner类定义在java.lang包中

```java
import java.util.Scanner;
public class scan {
    public static void main(){
        Scanner in = new Scanner(System.in);
    }
}
```

数据的读取

```java
**String name = in.nextLine();**
```

nextline使用方法详解：[https://www.geeksforgeeks.org/scanner-nextline-method-in-java-with-examples/](https://www.geeksforgeeks.org/scanner-nextline-method-in-java-with-examples/)   （TIZI）

在这里使用nextline是因为输入可能涉及到空格（nextline目前可粗略地理解为C地gets()）。在这里，如果我们想要输入一个单词，就可以使用到next()方法。

```java
String name = in.next();
System.out.print(name);
```

如果我们需要输入一个整数，则可以使用nextInt()。

```java
int number = 0;
number = in.nextInt();
System.out.print(number);
```

与此类似，想要读取下一个浮点数，我们就要调用nextDouble()方法。

小例子：

读取用户输入的姓名，年龄，并且输出一个相关的格式化输出。

```java
import java.util.Scanner;
public class scan {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.print("What is your name?\n");
        String name = in.nextLine();
        System.out.print("How old are you\n");
        int age;
        age = in.nextInt();
        System.out.println("Hello " +name+ " Next year you will be " +(age + 1));
    }
}
```

小声bb:这输出充满了Python味。

核心卷对于Scanner类的一个注释：

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2012.png)

对于部分输入方法的注释：

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2013.png)

### 格式化输入

我们可以使用System.out.print(x)来将数值x输出到控制台。这条命令以x的类型所允许的最大非0数位个数打印输出x

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2014.png)

但是这也带来了部分显示问题(显示美分，美元存在问题)。

Java 5 沿用了C函数库中的printf()。例如

```java
public class helloWorld {
    public static void main(String[] args){
     double x = 3.145674;
     System.out.printf("%4.3f",x);
    }
}//输出：3.146
```

了解转换符：

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2015.png)

也可以使用静态的String.format方法创建出一个格式化字符串，而不去打印输出。

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2016.png)

printf方法也有关羽日期与时间的格式化选项。java.time包中定义了相关的方法。

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2017.png)

### 文件输入与输出自查

### 控制流程

1. 块作用域 

Java中的块作用域与C中的基本一致，都是使用大括号来划定语句和变量的作用域。

1. 条件语句

Java中的条件语句形式为：

```java
if(condition) statement
```

剩余的与C语言保持一致，此处不再赘述。

while语句也是如此，此处不再赘述。（包括do while）

1. 循环语句

for/switch与C保持一致，此处不再赘述

## 大数

使用java.math包中的相关方法来进行大数的处理

看核心卷基础知识P95

## 数组

### 数组的声明

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2018.png)

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2019.png)

### 数组元素的访问

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2020.png)

对于数组元素的特殊for循环结构：for each结构

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2021.png)

### 数组拷贝

Java允许将一个数组变量拷贝到另一个数组变量，这时两个变量将引用同一个数组。

可以理解为这是一种**浅拷贝。**

```java
public class helloWorld {
    public static void main(String[] args){
       int []arraya = {1,2,3,4,5};
       int []arrayA = arraya;
       System.out.print(arrayA[1]);
    }
}
```

图表：

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2022.png)

若想实现深拷贝，则需要依靠Arrays类中的copy0f()方法。

```java
import java.util.Arrays;
public class helloWorld {
    public static void main(String[] args){
    int []arraya = {1,2,3,4,5};
    int []CopyArray = Arrays.copyOf(arraya,arraya.length);
    System.out.println(CopyArray[1]);
    }
}
```

### 关于数组的特别注释

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2023.png)

### 数组排序

若想对数值型数组进行排序，可以使用Arrays类中的sort方法：

```java
int []arraya = {1,2,3,4,5};
Arrays.sort(arraya);
```

此方法依据于优化了的快速排序(QuickSort)算法

获得随机数：

使用Math.random方法获得随机数

### 多维数组与C类似

### 不规则数组

我们可以使用一个循环来为存储了行的数组变量分配一个新的数组

```java
int [][]arraya = new int[5][];
       for(int number = 0;number<arraya.length;number++){
           arraya[number] = new int [number+1];
       }
```

C++注释：

![Untitled](E:\MyMarkdown\Java\Java\基础知识\Java程序基本设计框架\Untitled%2024.png)