---
layout:     post
title:      "剑指offer第41题“和为S的连续正数序列”"
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
# 和为S的连续正数序列
## 题目描述
题目描述
小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!
## 输出描述
输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序
***
## 思路
因为考虑到首位数字排序，可以设序列为x,x+1...x+n;
那么(2*x+n)*(n+1)/2=sum
通过移相可以得到：x=(sum-n*(n+1)/2)/(n+1)
之后只要遍历n的值，然后判断x是否为正整数即可。要注意代码中的式子要乘1.0，强制转换为浮点型。
## C++实现
```
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int> > ans;
        for(int i=sum/2;i>=1;i--){
            double x = 1.0*(sum-i*(i+1)/2)/(i+1);
            if(x==(int)x&&x>0){
                vector<int> vc;
                for(int j=0;j<=i;j++){
                    vc.push_back(x+j);
                }
                ans.push_back(vc);
            }
        }
        return ans;
    }
};
```
