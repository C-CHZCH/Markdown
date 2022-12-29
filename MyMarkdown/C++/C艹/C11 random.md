# C11 random

# 前言

对于伪随机函数，C语言提供了rand函数，如：`cout<<rand()<<endl;` 但是C语言提供的rand无法设定返回的类型以及随机生成的范围。

## C11

C11提供了random库，里面有许多模板类供我们使用，且我们也可以设置生成伪随机数的引擎。

简单查看：[https://www.jianshu.com/p/05863a00af8d](https://www.jianshu.com/p/05863a00af8d)