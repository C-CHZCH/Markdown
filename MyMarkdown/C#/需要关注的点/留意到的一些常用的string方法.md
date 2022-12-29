# 常用的string方法
1. Trim()，删除字符串头部及尾部出现的空格，删除的过程为从外到内，直到碰到一个非空格的字符为止，所以不管前后有多少个连续的空格都会被删除掉。可以用来格式化一个字符串变为一个标准的英文语句。例如[leetcode58](https://leetcode.cn/problems/length-of-last-word/)
2. Insert()，用于在指定索引处的现有字符串中插入一个字符串并返回一个新字符串。格式为`  public string String.Insert(int index, string value);`