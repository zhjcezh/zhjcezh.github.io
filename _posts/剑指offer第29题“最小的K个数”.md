---
layout:     post
title:      "剑指offer第29题“最小的K个数”"
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
# 最小的K个数
## 题目描述
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。
***
## 思路
用冒泡排序，排序k次，就能得到结果，瞬间反应的方法
## Java实现（冒泡）
```
import java.util.ArrayList;
public class Solution {
public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> al = new ArrayList<Integer>();
        if (k > input.length) {
            return al;
        }
        for (int i = 0; i < k; i++) {
            for (int j = 0; j < input.length - i - 1; j++) {
                if (input[j] < input[j + 1]) {
                    int temp = input[j];
                    input[j] = input[j + 1];
                    input[j + 1] = temp;
                }
            }
            al.add(input[input.length - i - 1]);
        }
        return al;
    }
}
```
第二种方法，是参考上一题，使用Partition函数，如果选到的数正好是第k个，那么左边的数就是已经排序的了
```
import java.util.ArrayList;
public class Solution {
    public ArrayList GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList aList = new ArrayList();
        if(input.length == 0 || k > input.length || k <= 0)
            return aList;
        int low = 0;
        int high = input.length-1;
        int index = Partition(input,k,low,high);
        while(index != k-1){
            if (index > k-1) {
                high = index-1;
                index = Partition(input,k,low,high);
            }else{
                low = index+1;
                index = Partition(input,k,low,high);
            }
        }
        for (int i = 0; i < k; i++)
            aList.add(input[i]);
        return aList;
    }
     
    int Partition(int[] input,int k,int low,int high){
        int pivotkey = input[k-1];
        swap(input,k-1,low);
        while(low < high){
            while(low < high && input[high] >= pivotkey)
                high--;
            swap(input,low,high);
            while(low < high && input[low] <= pivotkey)
                low++;
            swap(input,low,high);
        }
        return low;
    }
 
 
    private void swap(int[] input, int low, int high) {
        int temp = input[high];
        input[high] = input[low];
        input[low] = temp;
    }
}
```
