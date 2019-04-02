---
layout:     post
title:      "剑指offer第40题“数组中只出现一次的数字”"
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
# 数组中只出现一次的数字
## 题目描述
一个整型数组里除了两个数字之外，其他的数字都出现了偶数次。请写程序找出这两个只出现一次的数字。
***
## 思路
异或运算的一个性质：任何一个数字异或它自己都等于0，也就是说，如果我们从头到尾依次异或数组中的每一个数字，那么最终的结果刚好是哪个出现一次的数字，因为那些成对出现的两次的数字都在异或中低消了。
所以将本题中所有数字异或一遍，就能得到两个单独的数的异或值。
由于这两个数字肯定不一样，那么异或的结果肯定不为0，也就是说在这个结果数字的二进制表示中至少有一位为1.我们在结果数字中找到第一个为1的位的位置，记为第n位。现在我们以第n位是不是1为标准把原数组中的数字分成两个子数组，第一个子数组中的每个数字的第n位都是1，而第二个子数组中每个数字的第n位都为0
两个子数组均为一个单独的数加其他成对的数，所以将两个子数组分别求异或和，最终的结果就是我们要找的数
## C++实现
```
class Solution {
public:
    int judgeindexbitis1(int num,int index)
    {
        num = num >> index;
        return(1 & num);
    }
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
        int sum = 0;
        for(int i=0;i<data.size();i++){
            sum^=data[i];
        }
        int index=0;
        int temp=sum;
        while((1&temp)==0){
            temp>>=1;
            index++;
        }
        for(int i=0;i<data.size();i++){
            if((judgeindexbitis1(data[i],index))){
                *num1^=data[i];
            }
            else{
                *num2^=data[i];
            }
        }
        return;
    }
};
```
