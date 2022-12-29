# stringstream
C++引入了ostringstream、istringstream、stringstream这三个类，要使用他们创建对象就必须包含**sstream.h**头文件。

istringstream类用于执行C++风格的串流输入操作。

ostringstream类用于执行C++风格的串流输出操作。

stringstream类同时可以支持C++风格的串流的输入输出操作。

istringstream类是从istream和stringstreambase派生而来，ostringstream是从ostream和 stringstreambase派生而来， stringstream则是从iostream类和stringstreambase派生而来。

他们的继承关系如下图所示：

![](https://images2017.cnblogs.com/blog/763943/201710/763943-20171011163237699-1265211356.png)

上一段测试例子
```cpp
  #include <iostream>
  #include <sstream>
  using namespace std; 4 
  int main() 6 {
      istringstream istr;
      istr.str("1 56.7");
      //上述两个过程可以简单写成 istringstream istr("1 56.7");
     cout << istr.str() << endl;
     int a;
     float b;
     istr >> a;
     cout << a << endl;
     istr >> b;
     cout << b << endl;
     system("pause");
     return 0;
 }
```

在构造字符串流的时候，空格会成为字符串参数的内部分界，对a，b的赋值就可以说明这个。