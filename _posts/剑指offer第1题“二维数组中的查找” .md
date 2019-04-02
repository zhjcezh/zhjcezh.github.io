---
layout:     post
title:      "剑指offer第1题“二维数组中的查找"
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
# 二维数组中的查找
## 题目描述
在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
***
## 思路
题意是在有序二维数组中查找指定数
在这个二维数组中，最后一列是从小到大排序，所以当我们在最后一列找到第一个数值a>target时，可以确定如果有相同数值，那一定是与a同一行的，所以之后再遍历a所在行，判断是否有目标数值
## C++实现
```
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        if(array.size() == 0){
            return false;
        }
        int i = 0;
        int j = array[0].size() - 1;
        while(i < array.size() && j >= 0){
            if( array[i][j] == target ){
                return true;
            }
            if( array[i][j] < target){
                i++;
            }
            else{
                j--;
            }
        }
        return false;
    }
};
```
***
## Java实现
```
public class Solution {
    public boolean Find(int target, int [][] array) {
        if(array == null){
            return false;
        }
        int i = 0;
        int j = array[0].length - 1;
        while(i < array.length && j >= 0){
            if( array[i][j] == target ){
                return true;
            }
            if( array[i][j] < target){
                i++;
            }
            else{
                j--;
            }
        }
        return false;
    }
}
```