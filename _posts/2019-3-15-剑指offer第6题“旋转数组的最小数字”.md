---
layout:     post
title:      "剑指offer第6题“旋转数组的最小数字”"
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
# 旋转数组的最小数字
## 题目描述
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。
***
## 思路
一：暴力O(n)
二：二分，首先判断第一个值与最后一个值的大小关系，如果第一个值小则说明已经排好序，直接范围第一个值即可。反之，只要找到第一个降序的值，二分查找，如果中间值比第一个值大，则说明目标值在偏后位置，left=mid，反之right=mid。如果left与right相差一，输出小值即可，根据判断顺序，可以控制为一定为right。最后的返回随便写一个即可，不写会编译错误。
## C++实现
```
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        if(rotateArray.size()==0)
            return 0;
        int minn=INT_MAX;
        for(int i=0;i<rotateArray.size();i++){
            if(minn>rotateArray[i]){
                minn=rotateArray[i];
            }
        }
        return minn;
    }
};
```
***
## 二分查找
```
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        if(rotateArray.size()==0)
            return 0;
        int left=0,right=rotateArray.size()-1,mid=0;
        if(rotateArray[left]<rotateArray[right])
            return rotateArray[left];
        while(left<right){
            mid=(left+right)/2;
            if(rotateArray[left]<=rotateArray[mid]){
                left=mid;
            }
            else{
                right=mid;
            }
            if(left+1==right){
                return rotateArray[right];
            }
        }
        return rotateArray[mid];//写返回1，0，都对，数据问题
    }
};
```
