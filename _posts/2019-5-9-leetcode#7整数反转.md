---
layout:     post
title:      leetcode#7
subtitle:   "每日leetcode"
date:       2019-05-09 13:00:00
author:     "Cfeng"
header-img: "img/post-bg-alibaba.jpg"
catalog: true
tags:
    - 每日leetcode
    - 算法
---
# 整数反转
## 题目描述
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。     

> 示例 1:   
> 输入: 123    
> 输出: 321  
>          
> 示例 2:    
> 输入: -123    
> 输出: -321    
     

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。     
        
## 思路
解法主要分为两步，一步如何颠倒一个数字，一步如何判断数字范围。    
第一步： 不断将一个数字%10和/10，可以得到该数字各位分离的数字。    
第二部： 因为都是用int型的，如果超出了范围，其除以10的结果就不会跟之前的结果一致，通过这点也可以进行区分。    

## Java实现     
```   
class Solution {
    public int reverse(int x) {
        int result = 0;
        while (x != 0) {
            int tmp = result * 10 + x % 10;
            if (tmp / 10 != result) {
                return 0;
            }
            result = tmp;
            x = x / 10;
        }
        return result;
    }
}
```    

执行用时 : 5 ms, 在Reverse Integer的Java提交中击败了99.57% 的用户
内存消耗 : 33.4 MB, 在Reverse Integer的Java提交中击败了82.51% 的用户