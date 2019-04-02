---
layout:     post
title:      "剑指offer第14题“链表中倒数第k个结点”"
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

# 链表中倒数第k个结点
## 题目描述
输入一个链表，输出该链表中倒数第k个结点。
***
## 思路
链表题还是喜欢Java，省力
因为要找倒数第k个，所以可以定义两个指针，node1先移动k-1次，然后两个指针一起移动，直到node1到达末尾，此时node2就为目标。
要注意：链表可能输入为空，k也许为负，也有可能比链表长度大。都要考虑到。
（C++同一个思路写了半天，RE。。雾）

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
    public ListNode FindKthToTail(ListNode head,int k) {
        if(head==null||k<0)
            return null;
        ListNode node1=head;
        ListNode node2=head;
        int num=1;
        while(node1.next!=null){
            if(num==k)
                break;
            node1=node1.next;
            num++;
        }
        if(num!=k)
            return null;
        while(node1.next!=null){
            node1=node1.next;
            node2=node2.next;
        }
        return node2;
    }
}
```

