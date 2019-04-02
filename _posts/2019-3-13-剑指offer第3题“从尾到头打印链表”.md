---
layout:     post
title:      "剑指offer第3题“从尾到头打印链表”"
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
# 从尾到头打印链表
## 题目描述
输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。
***
## 思路
emmmmC++是返回一个vector，vector可以使用函数reverse，直接将vector翻转。
## C++实现
```
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> vc;
        while(head!=nullptr){
            vc.push_back(head->val);
            head=head->next;
        }
        reverse(vc.begin(),vc.end());
        return vc;
    }
};
```
***
## reverse
```
template <class BidirectionalIterator> void reverse (BidirectionalIterator first, BidirectionalIterator last)
{
     while ((first!=last)&&(first!=--last))
     {
          std::iter_swap (first,last);
          ++first;
     }
}
```
看到别人是再定义一个stack，先将值存入栈中，再从栈把元素取出放入vector
也行吧。。