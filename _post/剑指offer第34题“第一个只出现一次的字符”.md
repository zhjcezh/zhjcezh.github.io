---
layout:     post
title:      "剑指offer第34题“第一个只出现一次的字符”"
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
# 第一个只出现一次的字符
## 题目描述
在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.
***
## 思路
用map存储每一个字符出现的次数，之后遍历字符串，找出第一个只出现一次的字符，直接返回下标。
## C++实现
```
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        map<char,int> mp;
        for(int i=0;i<str.length();i++){
            mp[str[i]]++;
        }
        for(int i=0;i<str.length();i++){
            if(mp[str[i]]==1){
                return i;
            }
        }
        return -1;
    }
};
```
