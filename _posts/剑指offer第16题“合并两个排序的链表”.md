---
layout:     post
title:      "剑指offer第16题“合并两个排序的链表”"
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
# 合并两个排序的链表
## 题目描述
输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。
***
## 思路
同学问我这题怎么做。。思路竟然是新建一个链表来模拟题目。
我看到这题就想到了递归。因为很简单啊。每次提取一个点head，剩下的就是(list1.next,list2)或者(list1,list2.next)两种情况。head.next不就是再一次调用这个函数得到的点吗？
所以直接使用递归吧，模拟太烦了。
## Java递归实现
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
    public ListNode Merge(ListNode list1,ListNode list2) {
        if(list1==null)
            return list2;
        if(list2==null)
            return list1;
        ListNode head=null;
        if(list1.val<=list2.val){
            head=list1;
            head.next=Merge(list1.next,list2);
        }
        else{
            head=list2;
            head.next=Merge(list1,list2.next);
        }
        return head;
    }
}
```
## 非递归
```
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        ListNode head = new ListNode(0);
        ListNode p = head;
        boolean isFirst = true;
        while(list1 != null && list2 != null){
            if(list1.val < list2.val){
                p.next = list1;
                list1 = list1.next;
            }else{
                p.next = list2;
                list2 = list2.next;
            }
            p = p.next;
        }
        if(list1 != null){
            p.next = list1;
        }
        if(list2 != null){
            p.next = list2;
        }
        return head.next;
    }
    
}
```

