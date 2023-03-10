# STL标准模板库

## 容器与迭代器的定义

STL是C++库中高度成熟的已有实现。

STL容器类型：

1. 序列容器
2. 排序容器 
3. 哈希容器

STL迭代器：无论是序列容器还是关联容器，最常做的操作无疑是遍历容器中存储的元素，而实现此操作，多数情况会选用“迭代器（iterator）”

我们知道，尽管不同容器的内部结构各异，但它们本质上都是用来存储大量数据的，换句话说，都是一串能存储多个数据的存储单元。因此，诸如数据的排序、查找、求和等需要对数据进行遍历的操作方法应该是类似的。

既然类似，完全可以利用泛型技术，将它们设计成适用所有容器的通用算法，从而将容器和算法分离开。但实现此目的需要有一个类似中介的装置，它除了要具有对容器进行遍历读写数据的能力之外，还要能对外隐藏容器的内部差异，从而以统一的界面向算法传送数据。

这是泛型思维发展的必然结果，于是迭代器就产生了。简单来讲，迭代器和C++的指针

非常类似，它可以是需要的任意类型，通过迭代器可以指向容器中的某个元素，如果需要，还可以对该元素进行读/写操作。

**不同的容器具有不同的迭代器。**

C++11中不同容器指定使用的迭代器类型：

### 迭代器的定义方式

迭代器有着较为统一的定义方式。

[Untitled](STL标准模板库/Untitled.csv)

通过定义以上几种迭代器，就可以读取它指向的元素，

```
*迭代器名
```

就表示迭代器指向的元素。其中，常量迭代器和非常量迭代器的分别在于，通过非常量迭代器还能修改其指向的元素。另外，反向迭代器和正向迭代器的区别在于：

- 对正向迭代器进行 ++ 操作时，迭代器会指向容器中的后一个元素；
- 而对反向迭代器进行 ++ 操作时，迭代器会指向容器中的前一个元素。

## 迭代器本质是指针

迭代器实际上是对“遍历容器”这一操作进行了封装。

在编程中我们往往会用到各种各样的容器，但由于这些容器的底层实现各不相同，所以对他们进行遍历的方法也是不同的。例如，数组使用指针算数就可以遍历，但链表就要在不同节点直接进行跳转。

这是非常不利于代码重用的。例如你有一个简单的查找容器中最小值的函数findMin，如果没有迭代器，那么你就必须定义适用于数组版本的findMin和适用于链表版本的findMin，如果以后有更多容器需要使用findMin，那就只好继续添加重载……而如果每个容器又需要更多的函数例如findMax，sort，那简直就是重载地狱……

我们的救星就是迭代器啦！如果我们将这些遍历容器的操作都封装成迭代器，那么诸如findMin一类的算法就都可以针对迭代器编程而不是针对具体容器编程，工作量一下子就少了很多！

至于指针，由于指针也可以用来遍历容器(数组)，所以指针也可是算是迭代器的一种。但是指针还有其他功能，并不只局限于遍历数组。因为使用指针变量数组的操作太深入人心，c++stl中的迭代器就是刻意仿照指针来设计接口的

[序列式容器](STL标准模板库/序列式容器.md)

[关联容器](STL标准模板库/关联容器.md)

[iterator](STL标准模板库/iterator.md)