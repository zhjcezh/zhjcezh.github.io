---
layout:     post
title:      "剑指offer第19题“顺时针打印矩阵”"
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
# 顺时针打印矩阵
## 题目描述
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.
***
## 思路
设矩阵的行数为row，我们可以一圈一圈来，这样就只要循环row/2次即可
那每次的顺序是最上面一行从最左(left)到最右(right)，然后最右边一列，从第二行(top+1)到最后一行(low),然后最下面一行从倒数第二列(right-1)到第一列(left),然后最左边的倒数第二行(low-1)到第二行(top+1)，执行一次后将四个边界值跟新，就继续剥离操作。
## C++实现
```
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> vc;
        int row=matrix.size();
        int col=matrix[0].size();
        if(col==0||row==0)
            return vc;
        int left=0,right=col-1,top=0,low=row-1;
        while(left<=right&&top<=low){
            //最上，从左到右
            for(int i=left;i<=right;i++)
                vc.push_back(matrix[top][i]);
            //最右，从上到下
            for(int i=top+1;i<=low;i++)
                vc.push_back(matrix[i][right]);
            //最下，从右到左
            for(int i=right-1;i>=left&&top!=low;i--)
                vc.push_back(matrix[low][i]);
            //最左，从下到上
            for(int i=low-1;i>=top+1&&left!=right;i--)
                vc.push_back(matrix[i][left]);
            left++;
            right--;
            top++;
            low--;
        }
        return vc;
    }
};
```

