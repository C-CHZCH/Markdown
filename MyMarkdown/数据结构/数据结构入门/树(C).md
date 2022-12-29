# 树(C)

## 理论定义

**树是一种以层次化方式组织和存放数据的特定数据结构**，它看上去像一棵 "圣诞树"，它的根在上，叶朝下。

**树是n个(n≥0)个结点的有限集。它也是以边相连的结点的集合。**

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled.png)

n≠0的树具有以下**特性**:

1. 每个节点可以有多个**子节点**(children)，而该节点是相应子节点的**父节点**(parent) - 比如说，1,2 是 0 的子节点，3 是 7,8 的父节点

2. 树有一个没有父节点的节点，称为**根节点**(root) - 比如图中的 0 节点
   
       没有子节点的节点称为**叶节点**(leaf) - 比如图中的 7，8，9，10 (又称为叶子)

3. 节点两个具有相同父节点的节点称为**兄弟节点**(sibling) - 比如图中 4，5 节点互为兄弟节点

4. 一个节点的子节点以及子节点的后代称为该节点的**子树** (subtree) - 比如 1 和 1 的子节点构成了节点 0 的一棵子树

**树高：**是由根结点出发，到子结点的最长路径长度。

**结点深度：**是指对应结点到根结点路径长度。

**度：结点拥有的子树数称为结点的度。(例如上图根结点的度就是3)**

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled%201.png)

多棵互不相交的树并排在一起就是**森林**

### 树的基础性质

1. K叉树的第i层上至多有k^(i - 1)个结点(i≥1)
2. 深度为i的k叉树至多有(k^i - 1)/（k - 1）个结点（等比数列求和）
3. 对任何一课二叉树T，如果其终端结点数为n0,度为2的结点数为n2，则n0 = n2 + 1

## 二叉树

**二叉树(binary)是一种特殊的树，它是每个节点最多有两个子树的树结构，通常子树被称作是 "左子树" 和 "右子树。如下图:**

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled%202.png)

### 完全二叉树 (Complete Binary Tree)

若设二叉树的深度为 h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第 h 层所有的结点都**连续集中**在最左边，这就是完全二叉树。(从右到左**连续缺少**若干个结点)

例如:.

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled%203.png)

**即除了最后一层外，每一层上的节点数均达到最大值；在最后一层上只缺少右边的若干结点。**

而像这样就不是完全二叉树, 例如下图：(# 代表有元素)

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled%204.png)

### 满二叉树 (Full Binary Tree)

**除最后一层无任何子节点外，每一层上的所有结点都有两个子结点的二叉树被称之为满二叉树**。

满二叉树是特殊的完全二叉树

### 完全二叉树的性质(重要)

1. 完全二叉树有且仅有两种状态,一种是存在结点只有左孩子而没有右孩子。如下

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled%205.png)

另一种是不存在结点仅有左孩子。如满二叉树。

而这两种形态下，分支结点(度不为0的结点）与叶子结点存在数量上的关系。

第一种情况下分支节点 = 叶子结点数

第二种情况下分支结点 = 叶子结点数 - 1

1. 具有n个结点的完全二叉树的深度为[log2 n] + 1([]指的是向下取整）
2. 对一棵有n个结点的完全二叉树，则对其任意结点i（1≤i≤n）有

若i = 1，则i为根；如果 i>1 ,则其双亲是结点i

如果2i>n，则结点i无左孩子，否则左孩子是结点2i

如果2i+1>n，则结点无右孩子，否则右孩子是结点2i+1

### **完美二叉树(Perfect Binary Tree)**

`A Perfect Binary Tree(PBT) is a tree with all leaf nodes at the same depth. All internal nodes have degree 2.`

一个深度为k(>=-**1**)且有2^(k+1) - 1个结点的二叉树称为**完美二叉树**。 (注： 国内的数据结构教材大多翻译为"满二叉树")

### **完满二叉树(Full Binary Tree)**

`A Full Binary Tree (FBT) is a tree in which every node other than the leaves has two children.`

换句话说，**所有非叶子结点的度都是2**。（**只要你有孩子，你就必然是有两个孩子。**）

**注：**Full Binary Tree又叫做Strictly Binary Tree。

例如：

![https://images2015.cnblogs.com/blog/1094457/201702/1094457-20170225183305616-1864401342.png](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled%203.png)

### 二叉树的存储结构

1. 顺序存储结构

使用一维数组来进行顺序存储

1. 链式存储结构

```c
typedef struct Node{
    Elemtype data;
    struct Node *lchild,*rchild;
}
```

二叉树还有一些特殊的表示方法，在后面再论述。

### 创建和遍历二叉树

首先我们需要了解几个特殊的遍历顺序排法。

先序排列，中序排列，后序排列。

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled%207.png)

如果你按照 根节点 -> 左孩子 -> 右孩子 的方式遍历，即「先序遍历」，每次先遍历根节点，遍历结果为 1 2 4 5 3 6 7；

同理，如果你按照 左孩子 -> 根节点 -> 右孩子 的方式遍历，即「中序序遍历」，遍历结果为 4 2 5 1 6 3 7；

如果你按照 左孩子 -> 右孩子 -> 根节点 的方式遍历，即「后序序遍历」，遍历结果为 4 5 2 6 7 3 1；

最后，层次遍历就是按照每一层从左向右的方式进行遍历，遍历结果为 1 2 3 4 5 6 7

现在我们需要考虑的就是代码的实现部分。

`**创建二叉树部分**`

**前序遍历**创建二叉树。

```c
#include <stdio.h>
#include <stdlib.h>
typedef int Elemtype;
typedef struct Node{
    Elemtype data;
    struct Node *lchild,*rchild;
}Node;
Node* create_tree();
int main(char argc,char *argv[]){
    Node *root = create_tree();
    //printf("%d",root->lchild->lchild->lchild->data);
}

Node* create_tree(){
    Node *root;
    Elemtype cin_data;
    scanf("%d",&cin_data);
    if(cin_data == 0){//创建递归退出的条件
        return NULL;    
    }
    else{
        root = (Node*)malloc(sizeof(Node));
        root->data = cin_data;//前序创建
        root->lchild = create_tree();
        root->rchild = create_tree();
        return root;
    }

}
```

可以用一个栈来理解这个二重递归

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled%206.png)

图中的0即时没有结点。在我们输入1时，我们将此项压入栈底，接着我们进入了

```c
root->lchild = create_tree();
```

此条语句所创建的递归中，接着我们输入2，又进入了一层递归。

这时我们输入一个0，那么在2这一层递归中，我们执行了

```c
root->rchild = create_tree();
```

这一句(没有进入2所继续创建的的root->lchild = create_tree()，意味着2的左孩子为空)，由于我们继续输入了一个0，所以2的右孩子也为空。我们返回到data = 1这一层，顺便把data = 2的地址传给了1的左指针。最后我们又输入了一个0，代表data = 1的右孩子为空，程序结束。

![Untitled](E:\MyMarkdown\数据结构入门\数据结构入门\树(C)\Untitled%207.png)

谨记这是一个按照**先(前)序顺序**的创建方法。

[二叉树的遍历](二叉树的遍历.md)

[哈夫曼树](哈夫曼树.md)

### 线索二叉树

结构：

若结点有左子树，则其lchild域指示其左孩子，否则指示其前驱。

若结点有右子树，则其rchild域指示其右孩子，否则指示其后继。

[二叉查找树](二叉查找树.md)

[红黑树](红黑树.md)

[AVL树](AVL树.md)

