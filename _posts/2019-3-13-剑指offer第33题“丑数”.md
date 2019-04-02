---
layout:     post
title:      "剑指offer第33题“丑数”"
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
# 丑数
## 题目描述
把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。
***
## 思路
set有去重加排序功能，可以利用set存储所有丑数，每次当前丑数取出*2，*3，*5，存入set，作为丑数
之后找到相应丑数即可。
第0个返回0，坑到我了
## C++实现
```
class Solution {
public:
    int GetUglyNumber_Solution(int index) {
        if(index==0)
            return 0;
        set<long long> st;
        int a=index;
        st.insert(1);
        set<long long>::iterator it;
        it=st.begin();
        for(int i=1;i<=a;i++,it++){
            st.insert(*it*2);
            st.insert(*it*3);
            st.insert(*it*5);
        }
        it=st.begin();
        for(int i=1;i<index;i++){
            it++;
        }
        return *it;
    }
};
```
之后看了其他大佬的思路，秀秀秀
丑数的定义是1或者因子只有2 3 5，可推出丑数=丑数*丑数，假定丑数有序序列为：a1,a2,a3.......an 所以可以将以上序列（a1除外）可以分成3类，必定满足： 包含2的有序丑数序列：2*a1, 2*a2, 2*a3 ..... 包含3的有序丑数序列：3*a1, 3*a2, 3*a3 ..... 包含5的有序丑数序列：5*a1, 5*a2, 5*a3 ..... 以上3个序列的个数总数和为n个，而且已知a1 = 1了，将以上三个序列合并成一个有序序列即可 程序中t2,t3,t5实际就是合并过程中三个序列中带排序的字段索引 根据已知求到结果，再把这个结果当成已知，求下一个结果。。直到目标结果出现
# Java实现
```

public class Solution {
    public int GetUglyNumber_Solution(int index) {
        int []arr = new int [index];
        if(index==0)  return 0;
        arr[0]=1;int t2=0,t3=0,t5=0; //第一个丑数为1，放入数组第一个元素
      for(int i=1;i<index;i++)
      {
          arr[i]=Math.min(arr[t2]*2,Math.min(arr[t3]*3,arr[t5]*5));//通过1依次乘2，3，5找到后面的丑数，再通过找到的丑数依次乘2,3,5找到后面的，排序即可
         if(arr[t2]*2==arr[i]) t2++;
           if(arr[t3]*3==arr[i]) t3++;
           if(arr[t5]*5==arr[i]) t5++;
      }
        return arr[index-1];
    }
}

--------------------- 
作者：cquptpupplove 
来源：CSDN 
原文：https://blog.csdn.net/cquptpupplove/article/details/72123123 
版权声明：本文为博主原创文章，转载请附上博文链接！
```