---
layout:     post
title:      "剑指offer第43题“左旋转字符串”"
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
# 左旋转字符串
## 题目描述
汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！
***
## 思路
借用substr函数来截取两个字符串，然后调转顺序拼接返回即可。
需注意给定数字n可能大于字符串的长度，此时取余即可。
## C++实现
```
class Solution {
public:
    string LeftRotateString(string str, int n) {
        string resultStr = str;
        if (str.length() == 0) {
            return resultStr;
        }
        int len = str.length();
        int index = n % len;
        if (index != 0) {
            string clipStr = str.substr(0, index);
            string restStr = str.substr(index);
            resultStr = restStr + clipStr;
        }
        return resultStr;
    }
};
```
