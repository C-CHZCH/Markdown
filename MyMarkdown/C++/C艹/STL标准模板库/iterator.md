# iterator

除了为每个容器定义的迭代器外，标准库在头文件iterator中还定义了额外的迭代器

- 插入迭代器：这些迭代器被绑定在一个容器上，可用来向容器中插入元素
- 流迭代器：这些迭代器被绑定在输入或者输出流中，可用来遍历所关联的IO流。
- 反向迭代器：正如其名，这和我们容器绑定的普通迭代器的操作是相反的。
- 移动迭代器：这些专用的迭代器不是拷贝其中的元素，而是移动他们。


iterator在C++风格代码中非常常见，并且运用很广泛，特别是在STL容器中，代替了指针。
下面补充一些个人遇见的iterator中的妙用

1. 在与algorithm中的merge函数搭配使用
	首先先介绍写merge函数。这是一个STL已实现的归并排序的方法，其可用于将两个有序的序列合并为一个新的有序序列。
	C++ STL 标准库的开发人员考虑到用户可能需要自定义排序规则，因此为 merge() 函数设计了以下 2 种语法格式：（要求两个已知的数组的排序规则是相同的）
	
```
以默认的升序排序作为排序规则
OutputIterator merge (InputIterator1 first1, InputIterator1 last1,
                      InputIterator2 first2, InputIterator2 last2,
                      OutputIterator result);
以自定义的 comp 规则作为排序规则
OutputIterator merge (InputIterator1 first1, InputIterator1 last1,
                      InputIterator2 first2, InputIterator2 last2,
                      OutputIterator result, Compare comp);
```

可以看到，first1、last1、first2 以及 last2 都为输入迭代器，[first1, last1) 和 [first2, last2) 各用来指定一个有序序列；result 为输出迭代器，用于为最终生成的新有序序列指定存储位置；comp 用于自定义排序规则。同时，该函数会返回一个输出迭代器，其指向的是新有序序列中最后一个元素之后的位置。

> 注意，当采用第一种语法格式时，[first1, last1) 和 [first2, last2) 指定区域内的元素必须支持 < 小于运算符；同样当采用第二种语法格式时，[first1, last1) 和 [first2, last2) 指定区域内的元素必须支持 comp 排序规则内的比较运算符。

因此在result这个最终的存储序列的大小是固定的时候我们可以直接传入result的begin iterator来让方法保存最终的结果。但是如果我们并不确定result的大小时，可以使用back_insert()方法。
`back_inserter`用于在末尾插入元素。  
实现方法是构造一个迭代器，这个迭代器可以在容器末尾添加元素。  
这个迭代器是以安插（insert）方式而非覆写（overwrite）方式运作的。
可以使用`back_inserter`的容器是有`push_back`成员函数的容器，比如`vector, deque and list`等
在[leetcode1305](https://leetcode.cn/problems/all-elements-in-two-binary-search-trees/)中我们就使用到了这个技巧。在这题中我们并不知道最终的res的数组的大小，因此我们不可以仅仅的放入一个res的begin iterator，那么我们可以改写为这样`merge(m1.begin(),m1.end(),m2.begin(),m2.end(),back_inserter(ans));` 使用back_insert来构造一个尾部的迭代器，然后每次merge往ans中写入时都会使用这个尾部迭代器。

