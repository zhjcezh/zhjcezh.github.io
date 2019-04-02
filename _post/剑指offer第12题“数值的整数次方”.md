---
layout:     post
title:      "剑指offer第12题“数值的整数次方”"
subtitle:   ""
date:       2019-03-11 14:00:00
author:     "Cfeng"
header-img: "img/post-bg-css.jpg"
catalog: true
tags:
    - 笔试
    - 剑指Offer
    - 算法
---

# 数值的整数次方
## 题目描述
给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。
***
## 思路

因为不知道exponent的大小，所以暂时放弃暴力循环，选择快速幂.
快速幂就是把指数进行一次log(N)级别的变换。 11 = 2^3+2^1+2^0 
那么我只需要算 3^1 和 3^2 还有 3^8 这样复杂度就降下来了。算 3^1 需要一次记为a,把a平方就是 3^2 记为b，把b平方就是 3^4 记为c,再平方就是 3^8 记为d，这样把a b d乘以之后就是结果了。这样速度就快起来了~

## C++实现
```
class Solution {
public:
    double Power(double base, int exponent) {
        double ans = 1,newbase = base;
        int n=abs(exponent);
        while(n!=0)
        {
            if(n&1)
                ans *= base;
            base *= base;
            n>>=1;
        }
        return exponent>=0?ans:1/ans;
    }
};
```
***

皮一波嘻嘻嘻,但还是尽量不要用库函数了
## C++
```
class Solution {
public:
    double Power(double base, int exponent) {
        return pow(base, exponent);
    }
};
```
Python
```
# -*- coding:utf-8 -*-
class Solution:
    def Power(self, base, exponent):
        # write code here
        return base**exponent
```
