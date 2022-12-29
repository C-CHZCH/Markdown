# Lambda以及函数适配器

> C++ 11中引入了lambda表达式，可以方便的自定义生成匿名函数

## 语法格式

```cpp
[capture list] (params list) mutable exception-> return type { function body }
```

各部分的解释：

1. capture list：捕获外部变量列表
2. params list：形参列表
3. mutable指示符：用来说用是否可以修改捕获的变量
4. exception：异常设定
5. return type：返回类型
6. function body：函数体

此外我们呢也可以省略一些参数。

```cpp
[capture list] (params list) -> return type {function body}
[capture list] (params list) {function body}
[capture list] {function body}
```

这三种格式有各自的特点。

- 格式1声明了const类型的表达式，这种类型的表达式不能修改捕获列表中的值。
- 格式2省略了返回值类型，但编译器可以根据以下规则推断出Lambda表达式的返回类型： （1）：如果function body中存在return语句，则该Lambda表达式的返回类型由return语句的返回类型确定； （2）：如果function body中没有return语句，则返回值为void类型。
- 格式3中省略了参数列表，类似普通函数中的无参函数。

## 实例

### sort默认升序排序的过程

```cpp
using namespace std;

bool cmp(int a,int b){
    return a<b;
}//在sort中函数传递给cmp的a的是当前位置的下一个元素，而b则是当前元素
int main(){
   vector<int>test{0,1,3,2,10,9,50};
   sort(test.begin(),test.end(),cmp);
   for(int i : test){
       cout<<i<<endl;
   }
}
```

通过二元谓词函数cmp来制定规则。

但是这样非常的不方便，而且阅读性也较差。

采用lambda表达式可以轻松的解决这个问题。

```cpp
sort(test.begin(),test.end(),[](int a,int b)->bool {
       return a<b;
   });
```

sort的第三个参数中的lambda就实现了旧的二元谓词cmp的功能，而且表达上也直白很多。

## 外部变量的捕获 [ ]

Lambda表达式可以使用其可见范围内的外部变量，但必须明确声明（明确声明哪些外部变量可以被该Lambda表达式使用）。在STL中，函数可能会主动的往lambda生成的匿名函数中扔入变量，我们只需要设置对应的形参。

```cpp
#include <vector>
#include <iostream>
using namespace std;

int main()
{
    int a = 123;
    auto f = [a] { cout << a << endl; };
    f(); // 输出：123
    cout<<"type "<< typeid(f).name()<<endl;
    //或通过“函数体”后面的‘()’传入参数
    auto x = [](int a){cout << a << endl;}(123);
}
```

我们以这个例子进行讨论

在Lambda表达式中，外部变量的捕获方式也有值捕获、引用捕获、隐式捕获。

这一点和函数形参的传递非常类似。

### **值捕获**

值捕获和参数传递中的值传递类似，被捕获的变量的值在Lambda表达式创建时通过值拷贝的方式传入，因此随后对该变量的修改不会影响影响Lambda表达式中的值。

示例如下：

```cpp
int main()
{
    int a = 123;
    auto f = [a] { cout << a << endl; };
    a = 321;
    f(); // 输出：123
}
```

### **引用捕获**

使用引用捕获一个外部变量，只需要在捕获列表变量前面加上一个引用说明符&。如下：

```cpp
int main()
{
    int a = 123;
    auto f = [&a] { cout << a << endl; };
    a = 321;
    f(); // 输出：321
}
```

引用捕获的变量使用的实际上就是该引用所绑定的对象。

### **隐式捕获**

上面的值捕获和引用捕获都需要我们在捕获列表中显示列出Lambda表达式中使用的外部变量。除此之外，我们还可以让编译器根据函数体中的代码来推断需要捕获哪些变量，这种方式称之为隐式捕获。隐式捕获有两种方式，分别是[=]和[&]。[=]表示以值捕获的方式捕获外部变量，[&]表示以引用捕获的方式捕获外部变量。

隐式值捕获示例：

```cpp
int main()
{
    int a = 123;
    auto f = [=] { cout << a << endl; };    // 值捕获
    f(); // 输出：123
}
```

隐式引用捕获示例：

```cpp
int main()
{
    int a = 123;
    auto f = [&] { cout << a << endl; };    // 引用捕获
    a = 321;
    f(); // 输出：321
}
```

### 混合捕获

C++还支持混合捕获变量

如以下列表

[混合捕获](Lambda%E4%BB%A5%E5%8F%8A%E5%87%BD%E6%95%B0%E9%80%82%E9%85%8D%E5%99%A8%20a3e03c6121fc41a3a7b344bce72e880b/%E6%B7%B7%E5%90%88%E6%8D%95%E8%8E%B7%2074ad07265528444eb1f558afc1e8acc2.csv)

## 修改捕获的变量

正如上所讲，如果以传值方式捕获外部变量，则函数体中不能修改该外部变量。那么我们在值传递的时候有无办法修改外部变量？

### 关键字mutable

```cpp
int main()
{
    int a = 123;
    auto f = [a]()mutable { cout << ++a; }; // 不会报错
    cout << a << endl; // 输出：123
    f(); // 输出：124
}
```

## lambda表达式和常规的函数的差别

Lambda表达式中传递参数还有一些限制，主要有以下几点：

1. 参数列表中不能有默认参数
2. 不支持可变参数
3. 所有参数必须有参数名

# 函数适配器

如上我们不难发现，对于语句少的情况下，lambda表达式比函数更有优势，但是当我们需要很多地方复用相同的操作时，我们需要选择的是定义一个函数，而不是多次编写相同的lambda表达式。

如果lambda表达式的捕获列表为空时，我们很容易使用函数来替代。正如上方对于sort排序的cmp函数。但是对于捕获局部变量的lambda，使用函数来代替就不是一件容易的事情了。

此处我们已find_if函数作为一个例子。

find_if是根据我们给定的规则来判定是否返回当前找到的数据。

假如我们需要在一个string的vector中找到比给定的size更长的一个string对象

```cpp
bool check_size(const string &s,string::size_type sz){
     return s.size()>=sz;
}
```

我们很容易就可以编写出这样的一个函数来解决这个问题。

但是问题就出在find_if的第三个参数只接受一个一元谓词。

因此，我们这个函数是不可用的。我们需要解决如何传入sz参数的问题。

### 标准库bind函数

解决方法是使用一个名为bind的标准库函数，它定义在头文件functional中。

我们可以将bind看作一个通用的函数适配器，它接受一个可调用对象，生成一个新的可调用对象来适应原对象的参数列表。

```cpp
auto newCallable = bind(callable,arg_list);
```

newCallable本身是一个可调用对象，arg_list是一个逗号分隔的参数列表，对应给定callable的参数。调用newCallable时，其会调用callable，并且将arg_list作为形参传递给callable。arg_list中的参数占据了传递给newCallable的参数的位置。

### 绑定check_size的sz参数

我们为check_size绑定上size参数

```cpp
auto check6 = bind(check_size,_1,6);
```

此bind调用只有一个占位符，表示check6只接受单一的参数。占位符出现在arg_list的第一个位置，表示check6的此参数对应check_size的第一个参数。 此参数是一个const string&。因此，调用check6必须传递给它一个string类型的参数，check6会将此参数传递给check_size。

```cpp
string s = "hello";
bool b1 = check6(s);
```

使用bind，我们就可以将原来的基于lambda的find_if调用：

```cpp
bool check_size(const std::string &s, std::string::size_type sz) {
    return s.size()<=sz;
}

int main() {
    vector<string>words{"eee","ppp","i"};
    int sz = 1;
    auto wc = find_if(words.begin(),words.end(),[sz](const string &a){
        return a.size()<=sz;
    });
    words.insert(wc,"2");
    for(auto & word : words){
        cout<<word<<endl;
    }

}
```

替换为

```cpp
bool check_size(const std::string &s,std::string::size_type sz) {
    return s.size()<=sz;
}

int main() {
    vector<string>words{"eee","ppp","i"};
    int sz = 1;
    string p = "eee";
    auto wc = find_if(words.begin(),words.end(),bind(check_size,placeholders::_1,sz));
    words.insert(wc,"2");
    for(auto & word : words){
        cout<<word<<endl;
    }

}
```

在find_if运行时，其会调用经过bind绑定的函数对象，并且把当前遍历到的words[i]绑定到check_size函数中。

## placeholders名字

名字_n都定义在placeholders这一个命名空间里。为了使用这些名字。我们都需要写上std以及placeholders的命名空间。

## bind参数

如前文所言，我们可以通过bind修正参数的值。我们也可以用bind绑定可调用对象中的参数或者重新安排其顺序。

假定f是一个可调用对象，它有5个参数。

```cpp
auto g = bind(f,a,b,_2,c,_1);
```

我们来看看调用g进行参数绑定时的过程。

```cpp
g(_1,_2)对应着bind(f,a,b,_2,c,_1);
```

## *

对于函数对象与bind，lambda的解释：

[解释](Lambda%E4%BB%A5%E5%8F%8A%E5%87%BD%E6%95%B0%E9%80%82%E9%85%8D%E5%99%A8%20a3e03c6121fc41a3a7b344bce72e880b/%E8%A7%A3%E9%87%8A%20f177ccdc0b6343d997da4aa668cc5088.md)

## 例子
Lambda表达式的一个显著的使用例子：
[leetcode937](https://leetcode.cn/problems/reorder-data-in-log-files/)
```cpp
class Solution {
public:
    vector<string> reorderLogFiles(vector<string>& logs) {
        stable_sort(logs.begin(), logs.end(), [&](const string & log1, const string & log2) {
            int pos1 = log1.find_first_of(" ");
            int pos2 = log2.find_first_of(" ");
            bool isDigit1 = isdigit(log1[pos1 + 1]);
            bool isDigit2 = isdigit(log2[pos2 + 1]);
            if (isDigit1 && isDigit2) {
                return false;
            }
            if (!isDigit1 && !isDigit2) {
                string s1 = log1.substr(pos1);
                string s2 = log2.substr(pos2);
                if (s1 != s2) {
                    return s1 < s2;
                }
                return log1 < log2;
            }
            return isDigit1 ? false : true;
        });
        return logs;
    }
};
```