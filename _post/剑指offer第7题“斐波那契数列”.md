---
layout:     post
title:      "剑指offer第7题“斐波那契数列”"
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
# 斐波那契数列
## 题目描述
大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。
n<=39
***
## 思路
递归肯定超时不用想，那就for循环的O(n)，两个数相加再相减，就能每次保存临近两个值
一开始还在想39会不会爆int，后来发现模板返回的是int，那就不管了
如果n特别大，可以用python写，大数贼舒服
## C++实现
```
class Solution {
public:
    int Fibonacci(int n) {
        if(n==0)
            return 0;
        if(n==1)
            return 1;
        int a=0,b=1;
        for(int i=2;i<=n;i++){
            b+=a;
            a=b-a;
        }
        return b;
    }
};
```

