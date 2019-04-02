---
layout:     post
title:      "剑指offer第46题“孩子们的游戏"
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
# 孩子们的游戏(圆圈中最后剩下的数)
## 题目描述
每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)
***
## 思路
简单约瑟夫环问题
[约瑟夫环详细解释](https://blog.csdn.net/u011500062/article/details/72855826)
要注意出现错误输入返回-1.被坑了一次。。
## C++实现
```
class Solution {
public:
    int LastRemaining_Solution(int n, int m)
    {
         if(n < 1 || m < 1)
            return -1;
        int a = 0;
        for(int i = 2; i <=n; i++)
            a = (a + m) % i;
        return a;
    }
};
```
