# vector

vector 容器是STL中最常用的容器之一，它和 array 容器非常类似。不同之处在于，array 实现的是静态数组（容量固定的数组），而 vector 实现的是一个动态数组，即可以进行元素的插入和删除，在此过程中，vector 会动态调整所占用的内存空间，整个过程无需人工干预。

vector 常被称为**向量容器**，因为该容器擅长在尾部插入或删除元素，在常量时间内就可以完成，时间复杂度为`O(1)`；而对于在容器头部或者中部插入或删除元素，则花费时间要长一些（移动元素需要耗费时间），时间复杂度为线性阶`O(n)`。

## 定义

同样的，vector 容器以类模板 vector<T>（ T 表示存储元素的类型）的形式定义在 <vector> 头文件中，并位于 std 命名空间中。

创建vector：

```cpp
#include <vector>
using namespace std;
vector<double> values;
```

因为容器中没有元素，所以没有为其分配空间。当添加第一个元素时，vector会自动分配内存。

在创建好**空容器**的基础上，还可以像下面这样通过调用 reserve() 成员函数来增加容器的容量：

```cpp
values.reserve(20);
```

这样就设置了容器的内存分配，即至少可以容纳 20 个元素。注意，如果 vector 的容量在执行此语句之前，已经大于或等于 20 个元素，那么这条语句什么也不做；另外，调用 reserve() 不会影响已存储的元素，也不会生成任何元素，即 values 容器内此时仍然没有任何元素。
二维的vector是先定义行的数量，再去定义列。如：
```cpp
	int main(){
    vector<int>arr = {8,5,10,1,3,11,20,21,3,2,0};
    vector<vector<int>>a(9,arr);
   for(auto num : a){
       for(auto j :num){
           cout<<j<<endl;
       }
    }
}
```

### reserve和resize的区别

vector 的reserve增加了vector的capacity，但是它的size没有改变！而resize改变了vector的capacity同时也增加了它的size！

原因如下：

reserve是容器预留空间，但在空间内不真正创建元素对象，所以在没有添加新的对象之前，不能引用容器内的元素。加入新的元素时，要调用push_back()/insert()函数。

resize是改变容器的大小，且在创建对象，因此，调用这个函数之后，就可以引用容器内的对象了，因此当加入新的元素时，用operator[]操作符，或者用迭代器来引用元素对象。此时再调用push_back()函数，是加在这个新的空间后面的。

两个函数的参数形式也有区别的，reserve函数之后一个参数，即需要预留的容器的空间；resize函数可以有两个参数，第一个参数是容器新的大小， 第二个参数是要加入容器中的新元素，如果这个参数被省略，那么就调用元素对象的默认构造函数。

除此之外，vector容器也可像array那样进行类数组的初始化

```cpp
std::vector<int> primes {2, 3, 5, 7, 11, 13, 17, 19};
```

创建时也可指定元素个数以及初始值（默认是0）

```cpp
std::vector<double> values(20, 1.0);
```

PS:括号内的两个参数可以是变量，也可以是常量

通过存储类型相同的vector，也可以创建出新的vector

```cpp
std::vector<char>value1(5, 'c');
std::vector<char>value2(value1);
```

如果不想复制其它容器中所有的元素，可以用一对[指针](http://c.biancheng.net/c/80/)或者迭代器来指定初始值的范围，例如：

```cpp
int array[]={1,2,3};
std::vector<int>values(array, array+2);//values 将保存{1,2}
std::vector<int>value1{1,2,3,4,5};
std::vector<int>value2(std::begin(value1),std::begin(value1)+3);//value2保存{1,2,3}
```

### 常用成员函数

相比 array 容器，vector 提供了更多了成员函数供我们使用，它们各自的功能如表 1 所示。

[Untitled](vector%203b0631be23c64f12aa9481432288bf72/Untitled%20Database%20d8e9f06f34fc497cac58bf0399aee7ac.csv)

## vector容量capacity和大小size的区别

vector 容器的容量（用 capacity 表示），指的是在不分配更多内存的情况下，容器可以保存的最多元素个数；而 vector 容器的大小（用 size 表示），指的是它实际所包含的元素个数。

对于一个 vector 对象来说，通过该模板类提供的 capacity() 成员函数，可以获得当前容器的容量；通过 size() 成员函数，可以获得容器当前的大小。

### 添加元素

向vector中添加元素的唯一方式就是使用它的成员函数。

1. push_back()

向尾部添加一个元素(可以是变量)

```cpp
#include <iostream>
#include <vector>
using namespace std;
int main()
{
    vector<int> values{};
    values.push_back(1);
    values.push_back(2);
    for (int i = 0; i < values.size(); i++) {
        cout << values[i] << " ";
    }
    return 0;
}
```

1. emplace_back()

同样也是往尾部添加一个元素。

```cpp
#include <iostream>
#include <vector>
using namespace std;
int main()
{
    vector<int> values{};
    values.emplace_back(1);
    values.emplace_back(2);
    for (int i = 0; i < values.size(); i++) {
        cout << values[i] << " ";
    }
    return 0;
}
```

二者的差别：

emplace_back() 和 push_back() 的区别，就在于底层实现的机制不同。push_back() 向容器尾部添加元素时，首先会创建这个元素，然后再将这个元素拷贝或者移动到容器中（如果是拷贝的话，事后会自行销毁先前创建的这个元素）；而 emplace_back() 在实现时，则是直接在容器尾部创建这个元素，省去了拷贝或移动元素的过程。

因此emplace_back()有着更高的效率，优先使用。

1. insert()

insert() 函数的功能是在 vector 容器的指定位置插入一个或多个元素。

[Untitled](vector%203b0631be23c64f12aa9481432288bf72/Untitled%20Database%200b819f4ce9a64a599e1e098ae18eadb9.csv)

```cpp
#include <iostream> 
#include <vector> 
#include <array> 
using namespace std;
int main()
{
    std::vector<int> demo{1,2};
    //第一种格式用法
    demo.insert(demo.begin() + 1, 3);//{1,3,2}
    //第二种格式用法
    demo.insert(demo.end(), 2, 5);//{1,3,2,5,5}
    //第三种格式用法
    std::array<int,3>test{ 7,8,9 };
    demo.insert(demo.end(), test.begin(), test.end());//{1,3,2,5,5,7,8,9}
    //第四种格式用法
    demo.insert(demo.end(), { 10,11 });//{1,3,2,5,5,7,8,9,10,11}
    for (int i = 0; i < demo.size(); i++) {
        cout << demo[i] << " ";
    }
    return 0;
}
```

1. emplace()

emplace() 是C++11 标准新增加的成员函数，用于在 vector 容器指定位置之前插入一个新的元素。emplace() 每次只能插入一个元素，而不是多个。

语法格式如下：

```cpp
//iterator emplace (const_iterator pos, args...);
#include <vector>
#include <iostream>
using namespace std;
int main()
{
    std::vector<int> demo1{1,2};
    //emplace() 每次只能插入一个 int 类型元素
    demo1.emplace(demo1.begin(), 3);
    for (int i = 0; i < demo1.size(); i++) {
        cout << demo1[i] << " ";
    }
    return 0;
}
```

其中，pos 为指定插入位置的迭代器；args... 表示与新插入元素的构造函数相对应的多个参数；该函数会返回表示新插入元素位置的迭代器。

相比较于insert()，它的效率更高。

### 删除元素

[Untitled](vector%203b0631be23c64f12aa9481432288bf72/Untitled%20Database%2026a6edea5e634a06ba0b28df713e1cde.csv)

### vector迭代器独特之处

和 array 容器不同，vector 容器可以随着存储元素的增加，自行申请更多的存储空间。因此，在创建 vector 对象时，我们可以直接创建一个空的 vector 容器，并不会影响后续使用该容器。

但这会产生一个问题，即在初始化空的 vector 容器时，不能使用迭代器。

除此之外，vector 容器在申请更多内存的同时，容器中的所有元素可能会被复制或移动到新的内存地址，这会导致之前创建的迭代器失效。因此在改变容量之后，迭代器需要重新初始化。

### vector扩容问题