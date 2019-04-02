---
layout:     post
title:      "剑指offer第51题“构建乘积数组”"
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
# 构建乘积数组
## 题目描述
给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。
***
## 思路
类似于求数组的前缀和与后缀和一样，求出前缀积与后缀积
然后选择相应的头尾想乘得到最终结果
## C++实现
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        vector<int> a,b,ans;
        a.push_back(1);
        b.push_back(1);
        for(int i=0;i<A.size();i++){
            a.push_back(a[i]*A[i]);
            b.push_back(b[i]*A[A.size()-i-1]);
        }
        for(int i=0;i<A.size();i++){
            ans.push_back(a[i]*b[A.size()-i-1]);
        }
        return ans;
    }
};
