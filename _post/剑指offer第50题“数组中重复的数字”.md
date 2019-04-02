---
layout:     post
title:      "剑指offer第50题“数组中重复的数字”"
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
# 数组中重复的数字
## 题目描述
在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。
***
## 思路
数组中数字的大小已经确定为0·n-1之间。所以如果numbers[i]出现了，我们就令numbers[numbers[i]]+=n，这样子发现的第一个大于n的数的下标就是我们要找的数。
但需注意有些numbers[i]值已经改变，所以需要额外进行减n操作获得原先的数
## C++实现
```
class Solution {
public:
    // Parameters:
    //        numbers:     an array of integers
    //        length:      the length of array numbers
    //        duplication: (Output) the duplicated number in the array number
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    bool duplicate(int numbers[], int length, int* duplication) {
        for(int i=0;i<length;i++){
            int a=numbers[i];
            if(numbers[i]>=length){
                a-=length;
            }
            if(numbers[a]>=length){
                *duplication=a;
                return true;
            }
            numbers[a]+=length;
        }
        return false;
    }
};
```
