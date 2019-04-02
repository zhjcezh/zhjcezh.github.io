---
layout:     post
title:      "剑指offer第20题“包含min函数的栈”"
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

# 包含min函数的栈
## 题目描述
定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。
***
## 思路
因为是O(1)嘛，肯定不用想暴力了。所以我们可以再定有一个栈用来存储最小值。不能用int，因为如果最小值被pop了，是要回到之前的最小值的。
那辅助栈只要在每次push的时候判断，value值和栈顶的值谁大，value小就把value撞进来，反之就再装一次栈顶。要保证两个栈的大小是一样的。然后要一起pop。
## C++实现
```
class Solution {
private:
    stack<int> st1;
    stack<int> st2;
public:
    void push(int value) {
        st1.push(value);
        if(st2.empty())
            st2.push(value);
        else{
            int a=st2.top();
            st2.push(a<value?a:value);
        }
    }
    void pop() {
        st1.pop();
        st2.pop();
    }
    int top() {
        return st1.top();
    }
    int min() {
        return st2.top();
    }
};
```

