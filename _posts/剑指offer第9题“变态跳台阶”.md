---
layout:     post
title:      "剑指offer第9题“变态跳台阶”"
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
# 变态跳台阶
## 题目描述
一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
***
## 思路
参考上一题可知
f(n)=f(n-1)+f(n-2)+...+f(1)
f(n-1)=f(n-2)+...+f(1)
得f(n)=2*f(n-1)结束
## C++实现
```
class Solution {
public:
    int jumpFloorII(int number) {
        if(number==0||number==1)
            return 1;
        int a=1;
        for(int i=2;i<=number;i++){
            a*=2;
        }
        return a;
    }
};
```

