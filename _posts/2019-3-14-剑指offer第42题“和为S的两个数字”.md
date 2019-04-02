---
layout:     post
title:      "剑指offer第42题“和为S的两个数字”"
subtitle:   ""
date:       2019-03-14 14:00:00
author:     "Cfeng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
    - 笔试
    - 剑指Offer
    - 算法
---
# 和为S的两个数字
## 题目描述
输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。
## 输出描述
对应每个测试案例，输出两个数，小的先输出。
***
## 思路
如果数组没有排序，可以先进行排序，可以用快速排序或者堆排序的方法进行排序，然后分别从有序数组头尾向中间遍历，相加，等于就输出，大于sum 就end--，小于sum就start++.
因为要乘积最小所以找到的第一个就是答案。
## C++实现
```
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        vector<int> vc;
        int left=0;
        int right=array.size()-1;
        while(left<right){
            if(array[left]+array[right]==sum){
                vc.push_back(array[left]);
                vc.push_back(array[right]);
                return vc;
            }
            else if(array[left]+array[right]>sum){
                right--;
            }
            else{
                left++;
            }
        }
        return vc;
    }
};
```
