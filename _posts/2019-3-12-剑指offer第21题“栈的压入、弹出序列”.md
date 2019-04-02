---
layout:     post
title:      "剑指offer第21题“栈的压入、弹出序列”"
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
# 栈的压入、弹出序列
## 题目描述
输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）
***
## 思路
之前有做到过一题一样，就是模拟栈的压入和弹出。在循环按照顺序把pushV中的数压入栈，在压入栈的时候判断是否和popV的序列相等，如果相等就把栈pop了，和弹出序列一样。最后如果栈能全部弹出则为成功。
## C++实现
```
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        vector<int> st;
        if(pushV.size() == 0) 
            return false;
        for(int i=0,j=0;i<pushV.size();i++){
            st.push_back(pushV[i]);
            while(j<popV.size()&&st.back()==popV[j]){
                st.pop_back();
                j++;
            }
        }
        return st.empty();
    }
};
```

