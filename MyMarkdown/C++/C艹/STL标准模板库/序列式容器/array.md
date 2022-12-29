# array

array 容器是 C++11 标准中新增的序列容器，简单地理解，它就是在 C++ 普通数组的基础上，添加了一些成员函数和全局函数。在使用上，它比普通数组更安全，且效率并没有因此变差。

array的容器大小无法动态的扩展或收缩，它只允许访问或者替换存储的元素。

## 定义：

array以类模板的形式定义在<array>头文件，并位于命名空间std。

```cpp
#include <array>
using namespace std;
```

array容器有多种初始化方式：

在array<T,N>类模板中，T指名容器中的存储的具体数据类型，N用于指明容器的大小（必须是常量）。

```cpp
array<double,10>values;
```

由此就创建了一个名为values的array容器（aray容器不会默认初始化操作）

```cpp
array<double, 10> values {};//将所有的元素初始化为 0 
//或者和默认元素类型等效的值
```

当然array也可以像创建常规数组那样对元素进行初始化。

```cpp
array<double, 10> values {0.5,1.0,1.5,,2.0};
```

这里仅初始化了4个元素，剩余的元素都会被初始化为0.0

## 成员函数汇总

[Untitled](array/Untitled.csv)

PS：除此之外，C++ 11 标准库还新增加了 begin() 和 end() 这 2 个函数，和 array 容器包含的 begin() 和 end() 成员函数不同的是，标准库提供的这 2 个函数的操作对象，既可以是容器，还可以是普通数组。当操作对象是容器时，它和容器包含的 begin() 和 end() 成员函数的功能完全相同；如果操作对象是普通数组，则 begin() 函数返回的是指向数组第一个元素的指针，同样 end() 返回指向数组中最后一个元素之后一个位置的指针（注意不是最后一个元素）。

## 随机访问迭代器

查看上述常用成员函数可得知，许多函数都是返回一个迭代器。

### begin()/end() 和 cbegin()/cend()

array 容器模板类中的 begin() 和 end() 成员函数返回的都是正向迭代器，它们分别指向「首元素」和「尾元素+1」 的位置。在实际使用时，我们可以利用它们实现初始化容器或者遍历容器中元素的操作。

例如：循环中显式地使用迭代器来初始化容器地值

```cpp
#include <iostream>
#include <array>
using namespace std;
int main()
{
    array<int, 5>values;
    int h = 1;
    auto first = values.begin();
    auto last = values.end();
    //初始化 values 容器为{1,2,3,4,5}
    while (first != last)
    {
        *first = h;
        ++first;
        h++;
    }

    first = values.begin();
    while (first != last)
    {
        cout << *first << " ";
        ++first;
    }
    return 0;
}
```

得到1 2 3 4 5的输出。可以看到迭代器使用对象就像使用普通指针一样，可以通过加减来移动。

PS：无法通过迭代器来判断它指向的是array容器还是vector容器。

cbegin()和cend()与上相似，唯一不同的是它们返回的是一个const类型的正向迭代器。

### rbegin()/rend() 和 crbegin()/crend()

array 模板类中还提供了 rbegin()/rend() 和 crbegin()/crend() 成员函数，它们每对都可以分别得到指向最一个元素和第一个元素前一个位置的随机访问迭代器，又称它们为反向迭代器

```cpp
使用反向迭代器进行 ++ 或 -- 运算时，
++ 指的是迭代器向左移动一位，
-- 指的是迭代器向右移动一位，即这两个运算符的功能也“互换”了。
```

```cpp
#include <iostream>
//需要引入 array 头文件
#include <array>
using namespace std;
int main()
{
    array<int, 5>values;
    int h = 1;
    auto first = values.rbegin();
    auto last = values.rend(); 
    //初始化 values 容器为 {5,4,3,2,1}
    while (first != last)
    {
        *first = h;
        ++first;
        h++;
    }
    //重新遍历容器，并输入各个元素
    first = values.rbegin();
    while (first != last)
    {
        cout << *first << " ";
        ++first;
    }
    return 0;
}
```

得到输出为1 2 3 4 5.

## 访问元素

首先，可以通过容器名[]的方式直接访问和使用容器中的元素，这和

C++标准数组访问元素的方式相同，例如：

```cpp
values[4] = values[3] + 2.O*values[1];
```

此行代码中，第 5 个元素的值被赋值为右边表达式的值。需要注意的是，使用如上这样方式，由于没有做任何边界检查，所以即便使用越界的索引值去访问或存储元素，也不会被检测到。

为了能够有效地避免越界访问的情况，可以使用 array 容器提供的 at() 成员函数。

```cpp
values.at (4) = values.at(3) + 2.O*values.at(1);
```

这行代码和前一行语句实现的功能相同，其次当传给 at() 的索引是一个越界值时，程序会抛出 std::out_of_range 异常。

为什么array重载[]运算符时，没有边界检查功能：

答案很简单，因为性能。如果每次访问元素，都去检查索引值，无疑会产生很多开销。当不存在越界访问的可能时，就能避免这种开销。

除此之外，array 容器还提供了 get<n> 模板函数，它是一个辅助函数，能够获取到容器的第 n 个元素。需要注意的是，该模板函数中，参数的实参必须是一个在编译时可以确定的常量表达式，所以它不能是一个循环变量。也就是说，它只能访问模板参数指定的元素，编译器在编译时会对它进行检查。

```cpp
#include <iostream>
#include <array>
#include <string>
using namespace std;
int main()
{
    array<string, 5> words{ "one","two","three","four","five" };
    cout << get<3>(words) << endl; // Output words[3]
    //cout << get<6>(words) << std::endl; //越界，会发生编译错误
    return 0;
}
```

### 访问多个元素

array 容器提供的 size() 函数能够返回容器中元素的个数（函数返回值为 size_t 类型），所以能够像下面这样去逐个提取容器中的元素，并计算它们的和：

```cpp
double total = 0;
for(size_t i = 0 ; i < values.size() ; ++i)
{
    total += values[i];
}
```

除了借助 size() 外，对于任何可以使用迭代器的容器，都可以使用基于范围的循环，因此能够更加简便地计算容器中所有元素的和，比如：

```cpp
double total = 0;
for(auto&& value : values)
    total += value;
```

## array和常规的C 数组