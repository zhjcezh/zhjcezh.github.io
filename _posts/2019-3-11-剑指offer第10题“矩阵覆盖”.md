---
layout:     post
title:      "剑指offer第10题“矩阵覆盖”"
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
# 矩阵覆盖
## 题目描述
我们可以用2x1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2x1的小矩形无重叠地覆盖一个2xn的大矩形，总共有多少种方法？
***
## 思路
还是斐波那契
先考虑n=1的情况，就是竖着一种
再考虑n=2的情况，可以两个竖的和两个横的共两种
那n=3，就是在n=1的情况下多出两个位置，加上n=2的情乱搞下多出一个位置，又因为两竖情况重复，所以是1+2=3
f(n)=f(n+1)+f(n+2)
## C++实现
```
class Solution {
public:
    int rectCover(int number) {
        if(number<3)
            return number;
        int a=1,b=2;
        for(int i=3;i<=number;i++){
            b+=a;
            a=b-a;
        }
        return b;
    }
};
```
看到一个用Python写的
## Python
```
# -*- coding:utf-8 -*-
class Solution:
    def rectCover(self, number):
        if number == 0:
            return 0
        a = [1,2]
        if number > 2:
            for i in range(2,number):
                a.append(a[i-1]+a[i-2])
        return a[number-1]
--------------------- 
作者：Yeoman92 
来源：CSDN 
原文：https://blog.csdn.net/yeoman92/article/details/77882410 
版权声明：本文为博主原创文章，转载请附上博文链接！
```
