---
layout:     post
title:      leetcode#5
subtitle:   "每日leetcode"
date:       2019-05-08 18:00:00
author:     "Cfeng"
header-img: "img/post-bg-alibaba.jpg"
catalog: true
tags:
    - 每日leetcode
    - 算法
---
# 最长回文子串
## 题目描述
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。   
    
> 示例 1：
> 
> 输入: "babad"
> 输出: "bab"
> 注意: "aba" 也是一个有效答案。
> 
> 示例 2：
> 
> 输入: "cbbd"
> 输出: "bb"
        
## 思路
很简单的老题目，dp秒解，后来发现时间有点慢。就用Manacher算法又做了一遍   
![Manacher算法](https://subetter.com/algorithm/manacher-algorithm.html)   
模板题，不多介绍
后来发现有个稍作优化的暴力求解。。时间还飞快，惊了
## Java实现dp     
```   
class Solution {
    String longestPalindrome(String s) {
        int len = s.length();
        if (len <= 1) return s;
        int[][] dp = new int[len][len];
        int resLeft = 0, resRight = 0;
        dp[0][0] = 1;
        for (int i = 1; i < len; i++) {
            dp[i][i] = 1;
            dp[i][i - 1] = 1;
        }
        for (int k = 2; k <= len; k++)//枚举子串长度
            for (int i = 0; i <= len - k; i++)//枚举子串起始位置
            {
                if (s.charAt(i) == s.charAt(i + k - 1) && dp[i + 1][i + k - 2] == 1) {
                    dp[i][i + k - 1] = 1;
                    if (resRight - resLeft + 1 < k) {
                        resLeft = i;
                        resRight = i + k - 1;
                    }
                }
            }
        return s.substring(resLeft, resLeft + resRight - resLeft + 1);
    }
}
```    
执行用时 : 106 ms, 在Longest Palindromic Substring的Java提交中击败了40.86% 的用户    
内存消耗 : 74.3 MB, 在Longest Palindromic Substring的Java提交中击败了15.90% 的用户    
## Java实现Manacher
```
public String longestPalindrome(String s) {
    int len = s.length();
    if (len <= 1) return s;
    char[] cc = s.toCharArray();
    char[] c = new char[len * 2 + 3];
    c[0] = '!';
    c[len * 2 + 2] = '@';
    c[1] = '#';
    for (int i = 0; i < len; i++) {
        c[i * 2 + 2] = cc[i];
        c[i * 2 + 3] = '#';
    }
    int n = c.length, id = 0, mx = 0;
    int p[] = new int[n + 1];
    for (int i = 1; i < n - 1; i++) {
        if (2 * id - i < 0) {
            p[i] = 1;
        } else {
            p[i] = mx > i ? Math.min(p[2 * id - i], mx - i) : 1;
        }
        while (c[i + p[i]] == c[i - p[i]]) p[i]++;

        if (i + p[i] > mx) {
            mx = i + p[i];
            id = i;
        }
    }
    int maxLen = 0, index = 0;
    for (int i = 1; i < n - 1; i++)
        if (p[i] > maxLen) {
            maxLen = p[i];
            index = i;
        }

    return s.substring((index - maxLen) / 2, (index - maxLen) / 2 + maxLen - 1);
}
```
执行用时 : 8 ms, 在Longest Palindromic Substring的Java提交中击败了98.88% 的用户    
内存消耗 : 35.8 MB, 在Longest Palindromic Substring的Java提交中击败了91.68% 的用户   
## Java实现大佬代码
```
class Solution {
     public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        int[] range = new int[2];
        char[] c = s.toCharArray();
        for (int i = 0; i < s.length(); i++) {
            i = find(c, i, range);
        }
        return s.substring(range[0], range[1] + 1);
    }

    private int find(char[] c, int low, int[] range) {
        int max = c.length - 1;
        int high = low;
        while (high < max && c[high + 1] == c[low]) {
            high++;
        }
        int result = high;
        while (low > 0 && high < max && c[low - 1] == c[high + 1]) {
            low--;
            high++;
        }
        if (high - low > range[1] - range[0]) {
            range[0] = low;
            range[1] = high;
        }
        return result;
    }
}
```

执行用时 : 7 ms, 在Longest Palindromic Substring的Java提交中击败了99.04% 的用户    
内存消耗 : 34.9 MB, 在Longest Palindromic Substring的Java提交中击败了95.32% 的用户    