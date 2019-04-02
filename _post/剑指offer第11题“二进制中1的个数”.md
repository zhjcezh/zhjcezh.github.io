---
layout:     post
title:      "剑指offer第11题“二进制中1的个数”"
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

# 二进制中1的个数
## 题目描述
输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。
***
## 思路
正好知道Python有这样一个库函数，笑哭
## Python实现
```
# -*- coding:utf-8 -*-
class Solution:
    def NumberOf1(self, n):
        # write code here
        if n >= 0:
            return bin(n).count('1')
        else:
            return bin(n & 0xffffffff).count('1')
```
***
做完后在讨论版看到的，惊了
![图片](https://img-blog.csdn.net/20151106144820635)
## Java
```
public class Solution {
    public int NumberOf1(int n) {
        int count = 0;
        while(n!=0){
            count++;
            n=n & (n-1);
        }
        return count;
    }
}
```

