---
layout:     post
title:      leetcode#2
subtitle:   "每日leetcode"
date:       2019-05-07 15:00:00
author:     "Cfeng"
header-img: "img/post-bg-alibaba.jpg"
catalog: true
tags:
    - 每日leetcode
    - 算法
---
# 两数相加
## 题目描述
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。  
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。   
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。    
    
示例：   

> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
> 输出：7 -> 0 -> 8
> 原因：342 + 465 = 807 
      
## 思路     
要遍历链表的就要递归。于是要开始找递归规律。     
我为了方便起见，就新写了一个方法，多传递进位参数
在递归是要分类讨论，如果都为空，则要考虑是否有进位，有就返回该结点，没有就返回null
然后考虑l1为空和l2为空两种情况，为空就传递空值，操作没有什么复杂的地方
如果两者都不为空，则正常递归

## Java实现     
```   
/**
* Definition for singly-linked list.
* public class ListNode {
*     int val;
*     ListNode next;
*     ListNode(int x) { val = x; }
* }
*/
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode node = new ListNode(l1.val+l2.val);
        int num=node.val/10;
        node.val%=10;
        node.next=add(l1.next,l2.next,num);
        return node;
    }
    public ListNode add(ListNode l1, ListNode l2, int num) {
        ListNode node = new ListNode(0);
        if(l1==null&&l2==null){
            if(num>0){
                node.val=num;
            return node;
        }
        return null;
        }else if(l1==null){
            node.val=l2.val+num;
            num=node.val/10;
            node.val%=10;
            node.next=add(null,l2.next,num);
        }else if(l2==null){
            node.val=l1.val+num;
            num=node.val/10;
            node.val%=10;
            node.next=add(l1.next,null,num);
        }else{
            node.val=l1.val+l2.val+num;
            num=node.val/10;
            node.val%=10;
            node.next=add(l1.next,l2.next,num);
        }
        return node;
    }
}
```    
执行用时 : 9 ms, 在Add Two Numbers的Java提交中击败了96.31% 的用户
内存消耗 : 49.1 MB, 在Add Two Numbers的Java提交中击败了61.81% 的用户

