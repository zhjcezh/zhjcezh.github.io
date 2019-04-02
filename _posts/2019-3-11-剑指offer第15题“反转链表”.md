---
layout:     post
title:      "剑指offer第15题“反转链表”"
subtitle:   ""
date:       2019-03-11 14:00:00
author:     "Cfeng"
header-img: "img/post-bg-css.jpg"
catalog: true
tags:
    - 笔试
    - 剑指Offer
    - 算法
---
# 反转链表
## 题目描述
输入一个链表，反转链表后，输出新链表的表头。。
***
## 思路
继续用Java做链表题，本题就是顺序交换嘛
那可以定义三个指针，left指向刚更新好的点，最后会指向原来的表尾，所以最后是return left;
第二个指针就是mid，它指向正要被转换的点，转换完后往后移动一位即可。
第三个指针是right，它指向下一个要被转换的点。
## Java实现
```
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode left=null;
        ListNode mid=null;
        ListNode right=null;
        mid=head;
        while(mid!=null){
            right = mid.next;
            mid.next = left;
            left = mid;
            mid = right;
        }
        return left;
    }
}
```

