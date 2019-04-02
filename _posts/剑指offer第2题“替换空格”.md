---
layout:     post
title:      "剑指offer第2题“替换空格”"
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

# 替换空格
## 题目描述
请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
***
## 思路
刚开始想当然新定义了一个字符串，后来发现要在输入的str中进行修改。。。满头黑线
既然要在原先的字符串上改，那就要知道新字符串的长度newlen=spaceNum*2+length
然后从后往前修改，这样能确保需要的信息不被%20覆盖。
其他就是字符串边界搞清楚，不要RE了
## C++
```
class Solution {
public:
	void replaceSpace(char *str,int length) {
        if( length <= 0){
            return;
        }
        int spaceNum = 0;
        for ( int i = 0; i < length; i++){
            if ( str[i] == ' '){
                spaceNum++;
            }
        }
        int newlen = length + spaceNum * 2;
        for( int i = length - 1; i >= 0; i--){
            if( str[i] == ' ' ){
                str[--newlen] = '0';
                str[--newlen] = '2';
                str[--newlen] = '%';	
            }
            else{
                str[--newlen] = str[i];
            }
        }
    }
};
```
## Python
看了剑指offer的题解后，发现Python只要一行。。。。。真香
```
# -*- coding:utf-8 -*-
class Solution:
    # s 源字符串
    def replaceSpace(self, s):
        # write code here
        return s.replace(' ', '%20')
```
