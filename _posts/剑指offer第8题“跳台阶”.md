---
layout:     post
title:      "剑指offer第8题“跳台阶”"
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
# 跳台阶
## 题目描述
一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）
***
## 思路
为什么要把这题放在斐波那契的后面。。代码拿过来改下初值和n就可以了
## C++实现
```
class Solution {
public:
    int jumpFloor(int number) {
        if(number==0||number==1)
            return 1;
        int a=1,b=1;
        for(int i=2;i<=number;i++){
            b+=a;
            a=b-a;
        }
        return b;
    }
};
```

