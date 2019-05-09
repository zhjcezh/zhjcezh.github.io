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
请你来实现一个 atoi 函数，使其能将字符串转换成整数。    
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。    

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。    

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。    

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。    

在任何情况下，若函数不能进行有效的转换时，请返回 0。    

说明：    

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2^31,  2^31 − 1]。如果数值超过这个范围，qing返回  INT_MAX 或 INT_MIN。   

> 示例 1:    
> 输入: "42"    
>  输出: 42     
>       
> 示例 2:     
> 输入: "   -42"     
> 输出: -42     
> 解释: 第一个非空白字符为 '-', 它是一个负号。     
> 我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。           
        
## 思路
把正负号先标记去除，然后依次取出每一个字符拼接成数字，进行转换，在转换之前对数字进行判断，如果溢出就返回对应的最大或者最小值.    

## Java实现     
```   
class Solution {
    public int myAtoi(String str) {
        str = str.trim();
        if (str.isEmpty()) return 0;
        int sign = 1;
        long ans = 0;
        int i = 0;
        if (str.charAt(i) == '-'){
            sign = -1;
            i++;
        }else if(str.charAt(i) == '+'){
            i++;
        }
        while (i < str.length() && str.charAt(i) >= '0' && str.charAt(i) <= '9') { 
            ans = 10 * ans + (str.charAt(i++) - '0');
            if(ans>Integer.MAX_VALUE){
                if(sign==1){
                    return Integer.MAX_VALUE;
                }else{
                    return Integer.MIN_VALUE;
                }
            }
        }
        int a = (int)ans *sign;
        return a;
    }
}
```    

执行用时 : 8 ms, 在String to Integer (atoi)的Java提交中击败了90.25% 的用户    
内存消耗 : 34.7 MB, 在String to Integer (atoi)的Java提交中击败了97.85% 的用户     

后来我同学提醒我。。题目有规定只能存储 32 位大小的有符号整数，所以在判断溢出除修改ans>Integer.MAX_VALUE/10 或者 ans==Integer.MAX_VALUE / 10 && str.charAt(i)>'7'两者满足任意一者即返回
