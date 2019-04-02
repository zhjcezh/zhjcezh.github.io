---
layout:     post
title:      "剑指offer第45题“扑克牌顺子”"
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
# 扑克牌顺子
## 题目描述
LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。
***
## 思路
先将5个数排个序。然后找出0的个数，与它们之间的间隔数，例如1和3间隔1个数。
然后比较间隔数和0的个数即可。
## C++实现
```
class Solution {
public:
    bool IsContinuous( vector<int> numbers ) {
        if(numbers.size()<5){
            return false;
        }
        sort(numbers.begin(),numbers.end());
        int num1=0;
        int num2=0;
        for(int i=0;i<numbers.size()-1;i++){
            if(numbers[i]==0){
                num1++;
            }
            else{
                if(numbers[i]==numbers[i+1])
                    return false;
                num2+=(numbers[i+1]-numbers[i]-1);
            }
        }
        return num2<=num1;
    }
};
```
