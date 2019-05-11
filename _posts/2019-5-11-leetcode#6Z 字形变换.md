---
layout:     post
title:      leetcode#6
subtitle:   "每日leetcode"
date:       2019-05-11 13:00:00
author:     "Cfeng"
header-img: "img/post-bg-alibaba.jpg"
catalog: true
tags:
    - 每日leetcode
    - 算法
---
# Z 字形变换
## 题目描述
> 将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。     
> 比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：     
> L   C   I   R      
> E T O E S I I G        
> E   D   H   N      
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。     
请你实现这个将字符串进行指定行数变换的函数：      
string convert(string s, int numRows);       
> 示例 1:       
> 输入: s = "LEETCODEISHIRING", numRows = 3      
> 输出: "LCIRETOESIIGEDHN"      
> 示例 2:      
> 输入: s = "LEETCODEISHIRING", numRows = 4       
> 输出: "LDREOEIIECIHNTSG"      
> 解释:        
> L     D     R       
> E   O E   I I       
> E C   I H   N       
> T     S     G             
         
## 思路
发现每次到第一行和最后一行改变方向，所以设定一个sign判断方向，因为每次都是+1或-1，所以方向就用+1与-1表示，方便。然后就是遍历整个字符串，将字符放入相应的StringBuffer中。O(n)        
要注意的是，当行数等于1时要特殊j考虑，否则会k数组越界。      
## Java实现     
```   
public String convert(String s, int numRows) {
    if (numRows == 1) {
        return s;
    }
    StringBuffer[] strs = new StringBuffer[numRows];
        for (int i = 0; i < numRows; i++) {
        strs[i] = new StringBuffer();
    }
    int sign = -1;
    int step = 0;
    for (int i = 0; i < s.length(); i++) {
        strs[step].append(s.charAt(i));
        if (step == 0 || step == numRows - 1) {
            sign *= -1;
        }
        step += sign;
    }
    StringBuffer ans = new StringBuffer();
        for (int i = 0; i < numRows; i++) {
        ans.append(strs[i]);
    }
    return ans.toString();
}
```    

执行用时 : 24 ms, 在ZigZag Conversion的Java提交中击败了79.55% 的用户         
内存消耗 : 44.3 MB, 在ZigZag Conversion的Java提交中击败了71.30% 的用户
        
发现本方法时间效率有点低，就z又做了一遍，发现是有规律的，可以直接得到最终答案。     
第一行与最后一行，相邻两字符的下标永远相差2*(numRows-1)，所以可以直接得到答案的前端和后端。       
其他行分为两种，一种是在Z字斜线上的字母，另一种是竖线字母，斜线字母下标与竖线字母下标相差2*(numRows-i-1)；反之相差2i。      
        
## Java实现规律
```
public String convert(String s, int numRows) {
    if (numRows == 1) {
        return s;
    }
    StringBuffer ans = new StringBuffer();
    int j=0;
    for(int i=0;i<numRows;i++){
        int k=0;
        j=i;
        while(j<s.length()){
            ans.append(s.charAt(j));
            k++;
            if(i==numRows-1||i==0)
                j+=2*(numRows-1);
            else if((k&1)==0)
                j+=2*(i);
            else
                j+=2*(numRows-i-1);
        }
    }
    return ans.toString();
}
```
执行用时 : 10 ms, 在ZigZag Conversion的Java提交中击败了98.08% 的用户       
内存消耗 : 36.7 MB, 在ZigZag Conversion的Java提交中击败了99.54% 的用户
