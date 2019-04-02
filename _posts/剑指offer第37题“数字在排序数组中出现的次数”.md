---
layout:     post
title:      "剑指offer第37题“数字在排序数组中出现的次数”"
subtitle:   ""
date:       2019-03-13 14:00:00
author:     "Cfeng"
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
    - 笔试
    - 剑指Offer
    - 算法
---
# 数字在排序数组中出现的次数
## 题目描述
统计一个数字在排序数组中出现的次数。
***
## 思路
使用二分查找位置，一个是找第一次出现的，一个是找最后一次出现的，最后两个减减加个一。
## C++实现
```
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        int lower = getLower(data,k);
        int upper = getUpper(data,k);
        return upper - lower + 1;
    }
    int getLower(vector<int> data,int k){
        int start = 0,end = data.size()-1;
        int mid = (start + end)/2;
         
        while(start <= end){
            if(data[mid] < k){
                start = mid + 1;
            }else{
                end = mid - 1;
            }
            mid = (start + end)/2;
        }
        return start;
    }
    int getUpper(vector<int> data,int k){
         int start = 0,end = data.size()-1;
        int mid = (start + end)/2;
         
        while(start <= end){
            if(data[mid] <= k){
                start = mid + 1;
            }else{
                end = mid - 1;
            }
            mid = (start + end)/2;
        }
        return end;
    }
};
```
后来看到有另一种做法，二分查找索k-0.5和k+0.5两个点的位置，这样就不用官第几次了。但是浮点数比较大小是不是时间上稍慢一点
```
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        return biSearch(data, k+0.5) - biSearch(data, k-0.5) ;
    }
private:
    int biSearch(const vector<int> & data, double num){
        int s = 0, e = data.size()-1;     
        while(s <= e){
            int mid = (e - s)/2 + s;
            if(data[mid] < num)
                s = mid + 1;
            else if(data[mid] > num)
                e = mid - 1;
        }
        return s;
    }
};
```

