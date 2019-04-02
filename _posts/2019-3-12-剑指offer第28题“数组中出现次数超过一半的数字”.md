---
layout:     post
title:      "剑指offer第27题“数组中出现次数超过一半的数字”"
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
数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。
***
## 思路
第一个想到的思路就是map存储，O(n)吧，一边存一边记录出现次数，超过一半就直接返回
## C++实现（Map）
```
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        int n = numbers.size();
        map<int, int> m;
        int count;
        for (int i = 0; i < n; i++) {
            count = ++m[numbers[i]];
            if (count > n/2) 
                return numbers[i];
        }
        return 0;
    }
};
```
第二种方法，使用Partition函数，一个O(n)算法，具体目的是选取一个数，使该数的左边都比它小，右边都比它大，就是快排的一次排序的目的。下面代码中直接选取了第一个数，其实任一一个范围内的数都可以。
如果选取的数正好是中位数的话，那就是可能的正确答案，判断其是否满足要求，如果不是中位数，就向左或向有递归，直至找到中位数。
```
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        int length=numbers.size();
        if(numbers.empty()||length<0)
            return 0;
        int middle=length>>1;
        int start=0;
        int end=length-1;
        //int index=partition(numbers,length,start,end);      
        int index=partition(numbers,start,end);              
        while(index!=middle){
            if(index>middle){
               end=index-1;
               index=partition(numbers,start,end); 
            }
            else{
               start=index+1;
               index=partition(numbers,start,end);               
            }
        }
        int result=numbers[middle];
        if(!CheckMoreThanHAlf(numbers,length,result))
            return 0;
        return result;
    }
    int partition(vector<int> arr, int start, int end)
    {
        int pivotIndex=start;
        int pivot = arr[pivotIndex];
        swap(arr[pivotIndex], arr[end]);
        int storeIndex = start;
        for(int i = start; i < end; ++i) {
            if(arr[i] < pivot) {
                swap(arr[i], arr[storeIndex]);
                ++storeIndex;
            }
        }
        swap(arr[storeIndex], arr[end]);
        return storeIndex;
    }
    bool CheckMoreThanHAlf(vector<int> numbers,int length,int result){
        int times=0;
        for(int i=0;i<length;++i){
            if(numbers[i]==result)
                times++;
        }
        bool isMoreThanHalf=true;
        if(times*2<=length)
            isMoreThanHalf=false;
        return isMoreThanHalf;
    }
};
```
之后看到另一种解法链接：
如果有符合条件的数字，则它出现的次数比其他所有数字出现的次数和还要多。
在遍历数组时保存两个值：一是数组中一个数字，一是次数。遍历下一个数字时，若它与之前保存的数字相同，则次数加1，否则次数减1；若次数为0，则保存下一个数字，并将次数置为1。遍历结束后，所保存的数字即为所求。然后再判断它是否符合条件即可。
```
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers)
    {
        if(numbers.empty()) return 0;
         
        // 遍历每个元素，并记录次数；若与前一个元素相同，则次数加1，否则次数减1
        int result = numbers[0];
        int times = 1; // 次数
         
        for(int i=1;i<numbers.size();++i)
        {
            if(times == 0)
            {
                // 更新result的值为当前元素，并置次数为1
                result = numbers[i];
                times = 1;
            }
            else if(numbers[i] == result)
            {
                ++times; // 相同则加1
            }
            else
            {
                --times; // 不同则减1               
            }
        }
         
        // 判断result是否符合条件，即出现次数大于数组长度的一半
        times = 0;
        for(int i=0;i<numbers.size();++i)
        {
            if(numbers[i] == result) ++times;
        }
         
        return (times > numbers.size()/2) ? result : 0;
    }
};
```
