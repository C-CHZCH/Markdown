# string的一些操作
1. 自构C语言的split函数

```cpp
vector<string> split(string str, char del)
    {
        stringstream ss(str);
        string temp;
        vector<string> ret;
        while (getline(ss, temp, del))
        {
            ret.push_back(temp);
        }
        return ret;
    }
```

在这里，str需要传入的是需要切割的String对象，而del是分割的标志。
这里用到的是将str转化为字符串流（因为getline函数只能接受字符串流的放入），然后使用getline函数，第一个参数代表的是需要切割的字符串流，第二个则是用于保存每一次切割的string对象，最后一个参数则是切割的标志

2. 将string中的数字保留小数点后n位（四舍五入）

以下例子是保留小数点的后两位
```cpp
string doubleToString(const double &val)
    {
        char *chCode;
        chCode = new char[20];
        sprintf(chCode, "%.2lf", val);
        string str(chCode);
        delete[] chCode;
        return str;
    }
```
这其实是C语言风格的一个转换方式。但是其有比较大的弊端。
1. 需要保证缓冲区chCode足够的大，否则无法保证结果的正确性
2. 其中的格式化符号很容易写错。

C++风格的转换方式，则需要以下操作。
在介绍C++风格的转换之前，有必要先了解下[[stringstream]]。
我们来看看这题[leetcode2288](https://leetcode.cn/problems/apply-discount-to-prices/)
这题在对价格打折的时候是要求我们仅仅保留小数点的后两位。
我们来看看题解中使用sstream的解法
```cpp
class Solution {
public:
    string discountPrices(string sentence, int discount) {
        string res;
        istringstream iss(sentence);
        string t;
        while (getline(iss, t, ' ')) { // while (iss >> t)
            bool isPrice = (t.size() > 1 && t[0] == '$');
            if (isPrice) {
                for (int i = 1; i < t.size(); ++i) {
                    if (t[i] == '$' || t[i] < '0' || t[i] > '9') {
                        isPrice = 0; break;
                    }
                }
            }
            if (isPrice) {
                string p = t.substr(1);
                double price = stod(p);
                price *= (100 - discount) * 0.01;
                
                ostringstream oss;
                oss << fixed << setprecision(2) << price;
                
                res += '$' + oss.str() + ' ';
            } else {
                res += t + ' ';
            }
        }
        res.pop_back();
        return res;
    }
};
```
关键在于看判断出来￥后面的是数字之后（也就是`if (isPrice)`），创建了ostringstream对象，然后将price推到oss中。在使用`setprecisionsetprecision`的时候，我们先引入iomanip头文件。
```text

原数字 使用了此函数 最后输出的结果
28.92786  setprecision(3) 28.9

21.40 setprecision(5) 21.4

109.50 setprecision(4) 109.5

34.78596 setprecision(2) 35
```
而fixed关键字和此函数连用的时候，它将指定浮点数字的小数点后要显示的位数，而不是要显示的总有效数位数。