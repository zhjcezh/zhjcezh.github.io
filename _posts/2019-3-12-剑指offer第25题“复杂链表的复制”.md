---
layout:     post
title:      "剑指offer第25题“复杂链表的复制”"
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
# 复杂链表的复制
## 题目描述
输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）
***
## 思路
复杂链表的复制，还真复杂啊。
思路就是：假设原先链表为A->B，那我先复制一个A'，然后把A'插到AB中间，形成A->A'->B,这样把每一个结点都复制好后进入第二步，设置复制出来结点的random.假设原始结点的随机指向S，复制出来结点的random指向S后一个，第三步，抽离复制出来的链表，并把原先链表恢复原状
## JAva实现
```
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
public class Solution {
    public RandomListNode Clone(RandomListNode pHead){
        CloneNextNode(pHead);
        CloneRandomNode(pHead);
        return CloneHead(pHead);
    }
    public void CloneNextNode(RandomListNode pHead){
        RandomListNode node = pHead;
        while(node!=null){
            RandomListNode cloneNode = new RandomListNode(node.label);
            cloneNode.next=node.next;
            node.next=cloneNode;
            node=cloneNode.next;
        }
    }
    public void CloneRandomNode(RandomListNode pHead){
        RandomListNode node = pHead;
        while(node!=null){
            RandomListNode cloneNode = null;
            cloneNode=node.next;
            if(node.random!=null){
                cloneNode.random=node.random.next;
            }
            node=cloneNode.next;
        }
    }
    public RandomListNode CloneHead(RandomListNode pHead){
        RandomListNode cloneNode = null;
        RandomListNode head = null;
        RandomListNode node = pHead;
        if(node!=null){
            head = node.next;
            cloneNode = node.next;
            node.next = cloneNode.next;
			node = node.next;
        }
        while(node!=null){
            cloneNode.next = node.next;
            cloneNode = cloneNode.next;
            node.next = cloneNode.next;
			node = node.next;
        }
        return head;
    }
}
```

