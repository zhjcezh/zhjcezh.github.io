---
layout:     post
title:      "剑指offer第5题“用两个栈实现队列”"
subtitle:   ""
date:       2019-03-15 15:00:00
author:     "Cfeng"
header-img: "img/post-bg-universe.jpg.jpg"
catalog: true
tags:
    - 笔试
    - 剑指Offer
    - 算法
---
# 用两个栈实现队列
## 题目描述
用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
***
## 思路
栈是先进后出，队列是先进先出。用两个栈s1,s2来模拟队列，可以现将所有数据存入s1，再将s1中数据存入s2。从s2中提取的数据就是先进的数据。
因为题意是模拟队列的push和pop，不是一次性的操作。push可以就是将数据存入s1，而pop就要判断s2是否为空了，如果空就说明我们要将数据从s1传入s2，如果非空，就说明之前传过，但s2顶部的数据就是最先进入的数据
## C++
```
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        int a;
        if(stack2.empty()){
            while(!stack1.empty()){
                a=stack1.top();
                stack1.pop();
                stack2.push(a);
            }
        }
        a=stack2.top();
        stack2.pop();
        return a;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```
因为较简单，就不用其他东西来做了