---
layout:     post
title:      leetcode#4
subtitle:   "每日leetcode"
date:       2019-05-08 18:00:00
author:     "Cfeng"
header-img: "img/post-bg-alibaba.jpg"
catalog: true
tags:
    - 每日leetcode
    - 算法
---
# 寻找两个有序数组的中位数
## 题目描述
给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。   
请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 **O(log(m + n))**。    
你可以假设 nums1 和 nums2 不会同时为空。  
    
> 示例 1:
> 
> nums1 = [1, 3]
> nums2 = [2]
> 则中位数是 2.0
> 
> 示例 2:
> 
> nums1 = [1, 2]
> nums2 = [3, 4]
> 则中位数是 (2 + 3)/2 = 2.5
        
## 思路
题目要求O(log(m + n))，如果没有的话就是根据归并排序的思想直接找O((n+m)/2)   
但题目既然要求了就按照题目做。因为时间复杂度是log，所以能直接想到的就是二分和递归    
然后我就朝二分的思路想，后来发现题解的思路是递归。。真巧    
那已经确定要用二分了，就要思考二分啥？   
题目虽然是要求值，但我们可以把其变成求位置，二分大部分就是二分下标。确定了这点后，我就想到如果两个数组合并，那中位数的位置就是 pos = (len1 + len2 - 1) >> 1  （奇数）。也就代表其前面有pos个数，假设x个在nums1，pos-x个在nums2 。 OK，要二分的值确定了，就是x，确定x就能确定中位数。     
开始套二分模板，left = 0, right = len1;  当nums1[mid] > nums2[pos - mid - 1]改变右值，反之改变左值。
二分写法有好几种，只要确保找到答案能对就行
因为发现mid和pos - mid - 1可能会造成数组越界，所以优化左右初值。   
     
确定x即left后，开始分类讨论    
一：奇数   
    中位数是一个值，首先判断是否整个nums1都比中位数小，反之nums2也是，都是相对应的    
    之后判断nums1[left] 与 nums2[pos - left] 的大小，因为前pos个值已经确定，所以下一个值就是中位数，那么谁小谁就是下一个值    
              
二：偶数     
    中位数是均值，一样先判断两个极端情况      
    之后要判断两种特殊情况即nums1[left]是否比nums2[pos - left + 1]大,反之也要     
> 举例   
> nums1 = [1, 2, 8]
> nums2 = [3, 4, 5]
> 则中位数是 (3 + 4)/2 = 3.5  
     
    最后情况返回均值即可     

    注意要*0.5强制转换成double

## Java实现     
```   
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len1 = nums1.length;
        int len2 = nums2.length;
        int pos = (len1 + len2 - 1) >> 1;
        int left = Math.max(0, pos - len2), right = Math.min(pos, len1);
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums1[mid] > nums2[pos - mid - 1]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        if ((len1 + len2) % 2 == 1) {
            if (left >= len1) {
                return nums2[pos - left];
            } else if (pos - left >= len2) {
                return nums1[left];
            } else if (nums1[left] < nums2[pos - left]) {
                return nums1[left];
            } else {
                return nums2[pos - left];
            }
        } else {
            if (left >= len1) {
                return (nums2[pos - left] + nums2[pos - left + 1]) * 0.5;
            } else if (pos - left >= len2) {
                return (nums1[left] + nums1[left + 1]) * 0.5;
            } else if (pos - left + 1 < len2 && nums1[left] > nums2[pos - left + 1]) {
                return (nums2[pos - left] + nums2[pos - left + 1]) * 0.5;
            } else if (left + 1 < len1 && nums2[pos - left] > nums1[left + 1]) {
                return (nums1[left] + nums1[left + 1]) * 0.5;
            } else {
                return (nums1[left] + nums2[pos - left]) * 0.5;
            }
        }
    }
}
```    
执行用时 : 12 ms, 在Median of Two Sorted Arrays的Java提交中击败了92.69% 的用户
内存消耗 : 51.7 MB, 在Median of Two Sorted Arrays的Java提交中击败了73.05% 的用户  
