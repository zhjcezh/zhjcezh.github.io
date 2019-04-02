---
layout:     post
title:      "剑指offer第49题“把字符串转换成整数”"
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
# 把字符串转换成整数
## 题目描述
将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。
## 输入描述:
输入一个字符串,包括数字字母符号,可以为空
## 输出描述:
如果是合法的数值表达则返回该数字，否则返回0
***
## 思路
先判断首位是否可能有+-号。
然后把每一位数字加到num上。如果出现非数字即返回0
## C++实现
```
class Solution {
public:
    int StrToInt(string str) {
        int num = 0;
        bool fg=0;
        for(int i=0;i<str.length();i++){
            if(i==0){
                if(str[i]=='+')
                    continue;
                if(str[i]=='-'){
                    fg=1;
                    continue;
                }
            }
            if(str[i]>='0'&&str[i]<='9'){
                num=num*10+str[i]-'0';
            }
            else{
                return 0;
            }
        }
        if(fg==1){
            num*=-1;
        }
        return num;
    }
};
```
