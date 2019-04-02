---
layout:     post
title:      "剑指offer第36题“两个链表的第一个公共结点”"
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
# 两个链表的第一个公共结点
## 题目描述
输入两个链表，找出它们的第一个公共结点。
***
## 思路
如果两个链表有公共结点，那第一个公共结点后面的结点都是公共的。
所以两个链表最后一个结点都是公共的。所以我们要考虑如何操作之后，可以在遍历两链表时，能同时遍历到最后一个结点。
我们可以比较两个链表的长度，先将较长的链表，先遍历一点，使两链表等长，之后一个一个比过去就完事了。
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
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        ListNode h1 = pHead1;
        ListNode h2 = pHead2;
        int len1=0,len2=0;
        while(h1!=null){
            len1++;
            h1=h1.next;
        }
        while(h2!=null){
            len2++;
            h2=h2.next;
        }
        h1 = pHead1;
        h2 = pHead2;
        if(len1>=len2){
            int l=len1-len2;
            while(l>0){
                l--;
                h1=h1.next;
            }
            while(h1!=h2){
                h1=h1.next;
                h2=h2.next;
            }
            return h1;
        }
        else{
            int l=len2-len1;
            while(l>0){
                l--;
                h2=h2.next;
            }
            while(h1!=h2){
                h1=h1.next;
                h2=h2.next;
            }
            return h1;
        }
    }
}
```
