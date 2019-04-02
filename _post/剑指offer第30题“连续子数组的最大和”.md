---
layout:     post
title:      "剑指offer第30题“连续子数组的最大和”"
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
# 连续子数组的最大和
## 题目描述
HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)
***
## 思路
一开始默认求的是正数，然后错了一次。。难受
一开始定义两个数，一个表示当前的和，一个表示最大值，初值都为array[0]，之后如果加上后一个数是大于0的则继续往上加，反之则等于后一个数。然后每次循环比较一次最大值。
## Java实现
```
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        int num=array[0];
        int maxx=array[0];
        for(int i=1;i<array.length;i++){
            if(num>=0){
                num+=array[i];
            }
            else{
                num=array[i];
            }
            if(maxx<num)
                maxx=num;
        }
        return maxx;
    }
}
```