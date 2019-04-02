---
layout:     post
title:      "剑指offer第32题“把数组排成最小的数”"
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
# 把数组排成最小的数
## 题目描述
输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。
***
## 思路
和输出最大数的思路一样，将向量中的数全部排序即可。排序的规则不是比较a和b，而是比较ab与 ba。如果ab < ba，则a < b；如果ab > ba，则a > b；如果ab = ba，则a = b。
## C++实现
```
class Solution {
public:
    static bool cmp(int a,int b){
        int num1=1,num2=1;
        int aa=a,bb=b;
        while(a){
            a/=10;
            num1*=10;
        }
        while(b){
            b/=10;
            num2*=10;
        }
        return aa*num2+bb<aa+num1*bb;
    }
    string PrintMinNumber(vector<int> numbers) {
        sort(numbers.begin(),numbers.end(),cmp);
        string ans="";
        for(int i=0;i<numbers.size();i++){
            ans+=to_string(numbers[i]);
        }
        return ans;
    }
};
```