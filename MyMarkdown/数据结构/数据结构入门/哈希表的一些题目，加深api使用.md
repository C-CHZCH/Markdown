# 哈希表的一些题目，加深api使用

## 简单

### 字母出现的频率是否相等（等长的情况下）

出自[leetcode242](https://leetcode-cn.com/problems/valid-anagram/)

> 给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

示例：

```
输入: s = "anagram", t = "nagaram"
输出: true
```

写法：

```cpp
//
// Created by CHZCH on 2022/4/18.
//

#include <string>

using namespace std;

class Solution {
public:
    bool isAnagram(string s, string t) {
        int number_s[26];
        for (int &number_: number_s) {
            number_ = 0;
        }
        int number_t[26];
        for (int &number_: number_t) {
            number_ = 0;
        }
        int length_s = (int) s.size();
        int lengt_t = (int) t.size();
        for (int i = 0; i < length_s; ++i) {
            number_s[s[i] - 97]++;
        }
        for (int i = 0; i < length_s; ++i) {
            number_t[t[i] - 97]++;
        }
        for (int i = 0; i < 26; i++) {
            if (number_s[i] != number_t[i]) {
                return false;
            }
        }
        return true;
    }
};
```

自建一个数组来作为哈希表，根据ASCII中小写字母的十进制数，来作为key值。

### 两个数组的交集

来自[leetcode349](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

> 给定两个数组 `nums1`和 `nums2` ，返回 *它们的交集*。输出结果中的每个元素一定是 **唯一**
>  的。我们可以 **不考虑输出结果的顺序**。

示例：

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

很简单的一个set问题

```cpp
//
// Created by CHZCH on 2022/4/18.
//

#include <vector>
#include <unordered_set>

using namespace std;

class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set; // 存放结果
        unordered_set<int> nums_set(nums1.begin(), nums1.end());
        for (int num : nums2) {
            // 发现nums2的元素 在nums_set里又出现过
            if (nums_set.find(num) != nums_set.end()) {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
```

### 快乐数

来自[leetcode202](https://leetcode-cn.com/problems/happy-number/)

> 编写一个算法来判断一个数 n 是不是快乐数。「快乐数」 定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
> 然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
> 如果这个过程 结果为 1，那么这个数就是快乐数。
> 如果 n 是 快乐数 就返回 true ；不是，则返回 false 。

示例：

```
输入：n = 19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

关键其实是在题目中的无限循环。如果出现了无线循环的话，那么就可以肯定这绝对不是快乐数。

```cpp
//
// Created by CHZCH on 2022/4/18.
//
#include <unordered_set>

using namespace std;

class Solution {
public:
    // 取数值各个位上的单数之和
    static int getSum(int n) {
        int sum = 0;
        while (n) {
            sum += (n % 10) * (n % 10);
            n /= 10;
        }
        return sum;
    }
    static bool isHappy(int n) {
        unordered_set<int> set;
        while(true) {
            int sum = getSum(n);
            if (sum == 1) {
                return true;
            }
            // 如果这个sum曾经出现过，说明已经陷入了无限循环了，立刻return false
            if (set.find(sum) != set.end()) {
                return false;
            } else {
                set.insert(sum);
            }
            n = sum;
        }
    }
};
```

## 四数相加问题

来自[leetcode454](https://leetcode-cn.com/problems/4sum-ii/)

> 给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：0 <= i, j, k, l < n
> nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

示例：

```cpp
输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

其实这题相较于leetcode中的双数和以及三数和是比较简单的，因为没有要求去重。

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int, int> umap; //key:a+b的数值，value:a+b数值出现的次数
        // 遍历大A和大B数组，统计两个数组元素之和，和出现的次数，放到map中
        for (int a : A) {
            for (int b : B) {
                umap[a + b]++;
            }
        }
        int count = 0; // 统计a+b+c+d = 0 出现的次数
        // 在遍历大C和大D数组，找到如果 0-(c+d) 在map中出现过的话，就把map中key对应的value也就是出现次数统计出来。
        for (int c : C) {
            for (int d : D) {
                if (umap.find(0 - (c + d)) != umap.end()) {
                    count += umap[0 - (c + d)];
                }
            }
        }
        return count;
    }
};
```