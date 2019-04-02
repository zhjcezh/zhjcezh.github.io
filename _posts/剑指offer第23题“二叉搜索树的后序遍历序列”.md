---
layout:     post
title:      "剑指offer第23题“二叉搜索树的后序遍历序列”"
subtitle:   ""
date:       2019-03-12 14:00:00
author:     "Cfeng"
header-img: "img/post-bg-halting.jpg"
catalog: true
tags:
    - 笔试
    - 剑指Offer
    - 算法
---
# 二叉搜索树的后序遍历序列
## 题目描述
输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。
***
## 思路
后序遍历序列，最主要的就是根节点在末尾，那二叉搜索树的根的左儿子一定比根小，所以从右往左找到的第一个比根小的结点，就是左子树的根，这样左右就被区分为，左右子树的后续遍历序列。之后又是相同的方法区分，既然是相同的方法，就可以写个递归函数。
## C++实现
```
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        if(sequence.size()==0)
            return false;
        return judge(sequence, 0, sequence.size() - 1);
    }
    bool judge(vector<int>& a, int l, int r){
        if(l >= r) 
            return true;
        int i = r;
        while(i > l && a[i - 1] > a[r]) 
            i++;
        for(int j = i - 1; j >= l; j--) 
            if(a[j] > a[r]) 
                return false;
        return judge(a, l, i - 1) && (judge(a, i, r - 1));
    }
};
```

