---
layout:     post
title:      "剑指offer第44题“翻转单词顺序列”"
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
# 翻转单词顺序列
## 题目描述
牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？
***
## 思路
使用了Java中split来分割字符串为字符串数组。
然后通过StringBuilder来拼接字符串，要注意不能多空格。
## Java实现
```
class Solution {
    public String ReverseSentence(String str) {
    	if(str==null||str.trim().length()==0)
    		return str;
        String[] s = str.split("\\s");
        StringBuilder sb = new StringBuilder();
        for(int i=s.length-1;i>=1;i--){
        	sb.append(s[i]);
        	sb.append(" ");
        }
        sb.append(s[0]);
        return sb.toString();
    }
}
```
