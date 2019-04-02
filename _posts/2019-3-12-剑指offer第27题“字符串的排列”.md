---
layout:     post
title:      "剑指offer第27题“字符串的排列”"
subtitle:   ""
date:       2019-03-12 14:00:00
author:     "Cfeng"
header-img: "img/post-bg-halting.jpg"
catalog: true
tags:
    - 笔试
    - 剑指Offer
    - 算法
---
# 字符串的排列
## 题目描述
输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。
## 输入描述:
输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。
***
## 思路
全排列问题，dfs+递归，简单模板题
## Java实现
```
import java.util.ArrayList;
import java.util.Collections;
public class Solution {
    public ArrayList<String> Permutation(String str) {
		ArrayList<String> list = new ArrayList<String>();
		char[] ch = str.toCharArray();
		Permu(ch, 0, list);
		Collections.sort(list);
		return  list;
	}
 
	public void Permu(char[] str, int i, ArrayList<String> list) {
		if (str == null) {
			return;
		}
		if (i == str.length - 1) {
			if(list.contains(String.valueOf(str))){
				return;
			}
			list.add(String.valueOf(str));
		} else {
			boolean num=true;
			for (int j = i; j < str.length; j++) {
				char temp = str[j];
				str[j] = str[i];
				str[i] = temp;
				Permu(str, i + 1, list);
				temp = str[j];
				str[j] = str[i];
				str[i] = temp;
			}
		}
	}
}

```
