# KMP(C)

## 算法目的

kmp算法用于文本串和模式串的匹配。(字符串匹配算法)

KMP算法的核心是利用匹配失败后的信息，尽量减少模式串与主串的匹配次数以达到快速匹配的目的。具体实现就是通过一个next()函数实现，函数本身包含了模式串的局部匹配信息。KMP算法的时间复杂度O(m+n)

## 理论部分

以下给出模式串和子串匹配的例子

```
文本串：aabaabaaf
模式串：aabaaf
```

通常，暴力解法为设置两个指针，一个指向文本串，另一个指向模式串。然后指针在模式串中不断指向字符，与文本串的字符相比较。当遇到不匹配的，模式串向前移动一位，然后再重复之前的操作。

那么在KMP中，我们需要先引入一个概念：前缀，后缀，最大相等前后缀

### 前缀与后缀(也可以理解为python的切片)后缀就是右切，前缀就是左切。

```
文本串：aabaabaaf
模式串：aabaaf
```

仍然以此模式串为例子

后缀不可包含头部,前缀不可包含尾部。

那么，这里模式串的后缀为

```
f
af
aaf
baaf
abaaf
```

前缀为

```
a
aa
aab 
aaba 
aabaa
```

### 最长相等前后缀

我们对模式串的各个子串进行最长相等前后缀的分析

```
子串     长度
a         0
aa        1
aab       0
aaba      1
aabaa     2
aabaaf    0
```

这里是对最长相等前后缀的长度进行了分析

那么，这里我们得到了一串序列

```
010120
```

那么，这就是模式串的前缀表

```
模式串：aabaaf
       010120
```

这时候问题来了，我们如何使用前缀表进行匹配呢

### 使用前缀表进行模式串与文本串的匹配

```
文本串: aabaabaaf
下标:   012345
模式串: aabaaf
前缀表: 010120
```

如上，我们在进行常规的字符串匹配时发现到模式串的f位置时，f与文本串中的b不匹配了

那么，这时候我们就要找f之前的子串的最长相等前后缀，就是f之前的第一个a对应的前缀表的数。

这个2意味着什么呢？

这个2代表着这里有一个长度为2的后缀，而也有一个长度为2的与之相等的前缀。

目的是让相等的后缀变成新的前缀。

所以，我们要做的事情就是把指针移动到下标为2的位置中重新进行字符串的匹配。(这里这句话可以这样理解:KMP算法的核心是利用匹配失败后的信息，尽量减少模式串与主串的匹配次数以达到快速匹配的目的,那么我们发现了有最长相等前后缀之后，我们为了减少文字串与模式串的匹配次数，那么，我们就要去掉相等的前缀的匹配。因此我们要把指针移动到最大相等前缀的后一位，如上:就是b。又因为计算机正好是0开始的，所以就是下标为2的位置。)

这么做之后,现在文本串和模式串的匹配位置应该是这样的:

```
文本串: aabaabaaf
模式串:    aabaaf
```

## 实现部分

### next数组的构建

**构造next数组其实就是计算模式串s，前缀表的过程。** 主要有如下三步：

1. 初始化
2. 处理前后缀不相同的情况
3. 处理前后缀相同的情况

首先是初始化部分：

```c
int j = 0;
next[0] = 0;
```

我们先创建一个j游标，并且初始化了next数组。j其实更像一个探路指针

以下是最关键的创建next数组部分:

```c
for (int i = 1; i < length; i++)
    {
        while (j > 0 && S[i] != S[j])
        {                    // j要保证大于0，因为下面有取j-1作为数组下标的操作
            j = next[j - 1]; // 注意这里，是要找前一位的对应的回退位置了
        }
        if (S[i] == S[j])
        {
            j++;
        }
        next[i] = j;
    }
```

这一段代码的关键是:我们对于i = 1，也就是第二个字符开始,我们对于前缀表的构建都是依据之前所构建的前缀表来判断的。

```
文本串: aabaabaaf
下标:   012345
模式串: aabaaf
前缀表: 010120
```

为什么这么说，看下面:

```
i = 1   j = 1
i = 2   j = next[0] = 0
i = 3   j = 0 + 1 = 1
```

在i = 1 时，我们所截取的子串是aa，此时符合的是前缀末尾 == 后缀末尾(说人话就是子串第一个和最后一个相等,这就意味着**这个子串的最大相等前后缀的长度至少为1)** 这时候我们所进入的是

```c
if (S[i] == S[j])
        {
            j++;
        }
```

这么一个语句，并且next[1] = 1。

在i = 2时，我们所截取的子串为aab

```c
while (j > 0 && S[i] != S[j])
        {                    // j要保证大于0，因为下面有取j-1作为数组下标的操作
            j = next[j - 1]; // 注意这里，是要找前一位的对应的回退位置了
        }
```

这时候我们要知道上一个子串aa对应的next为1，也就是j = 1。

那么这里j = next[j - 1] = next[ 0 ] = 0的意思是：

1. S[ i ] ≠ S[ j ]：这里所比较的是当前的末尾b和上一个所截取的子串的最大相等前后缀的下一位a
2. **通过这样比较，我们就可以知道现在的S[ i ]能否增加上一个子串的最大相等前后缀长度**。
3. 如果第一次比较不能那么我们就往后退一步，继续比较，看是否能够找到一个相等的前后缀。
4. 如果一直到S[ 0 ]之前都找不到相等的前后缀,我们就返回next[ 0 ]，也就是0，代表S[ i ]下的子串没有相等的前后缀。

这个例子其实用i = 5 aabaaf寻找相等前后缀来理解可能更加容易(TS:可以试想下如果把f换成b，那么这个while循环又会发生什么)

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\KMP(C)\Untitled.png)

### 字符串匹配

```c
#include <stdio.h>
typedef int Status;
typedef char *Star;
#define size 100; //字符串的最大长度
void getnext(Status *next, const Star S, Status length);
Status string_match(Status next[], Star string, Star Text_string, Status Text_length, Status length);
Status main(Status argc, Star argv[])
{
    printf("请依照此顺序输入:模式串长度，模式串，文本串长度，文本串\n");
    Status cin_length = 0;
    scanf("%d\n", &cin_length); //模式串长度
    char str[cin_length + 1];
    Star string = str;
    gets(str);//模式串的输入
    Status Text_length = 0;
    scanf("%d\n", &Text_length);//文本串长度
    char Text[Text_length + 1];
    Star Text_string = Text;
    gets(Text_string);//文本串的输入
    //printf("%s\n", str);
    //printf("%s\n",Text_string);
    Status next[cin_length];
    getnext(next, string, cin_length);//next数组的获取（不移动版本）
    printf("next数组为\n");
    for (Status site = 0; site < cin_length; site++)
    {
        printf("%d ", next[site]);
    }
    printf("\n");
    Status state = 0;
    state = string_match(next, string, Text_string, Text_length, cin_length);//（匹配函数）
    if (state == -1)
    {
        printf("未找到\n");
        return 0;
    }
    return 0;
}

void getnext(Status *next, const Star S, Status length)
{
    int j = 0;
    next[0] = 0;
    for (int i = 1; i < length; i++)
    {
        while (j > 0 && S[i] != S[j])
        {                    // j要保证大于0，因为下面有取j-1作为数组下标的操作
            j = next[j - 1]; // 注意这里，是要找前一位的对应的回退位置了
        }
        if (S[i] == S[j])
        {
            j++;
        }
        next[i] = j;
    }
}

Status string_match(Status next[], Star string, Star Text_string, Status Text_length, Status length)
{
    //printf("%s\n",string);
    //printf("%s\n",Text_string);
    Status cur = 0; //模式串位置
    for (Status T_cur = 0; T_cur < Text_length; T_cur++)
    {
        while (cur > 0 && Text_string[T_cur] != string[cur])
        {                        //不匹配的情况
            cur = next[cur - 1]; //到next中寻找回溯的位置
            //printf("k\n");
        }
        if (string[cur] == Text_string[T_cur])
        {          //匹配，二者同时向后移动
            cur++; //T_cur的增加在循环中
        }
        if (cur == length)
        {
            printf("文本串中有相应的子串\n");
            return 1;
        }
    }
    return -1;
}
```