---
layout:     post
title:      "剑指offer第39题“平衡二叉树”"
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
# 平衡二叉树
## 题目描述
输入一棵二叉树，判断该二叉树是否是平衡二叉树。
***
## 思路
据二叉树的定义，我们可以递归遍历二叉树的每一个节点来，求出每个节点的左右子树的高度，如果每个节点的左右子树的高度相差不超过1，按照定义，它就是一颗平衡二叉树。
## C++实现
```
class Solution {
public:
    int TreeDepth(TreeNode* pRoot)
    {
        if(pRoot==nullptr)
            return 0;
        int left = TreeDepth(pRoot->left);
        int right = TreeDepth(pRoot->right);
        return 1+max(left,right);
    }
    bool IsBalanced_Solution(TreeNode* pRoot) {
        if(pRoot==nullptr)
            return true;
        int left = TreeDepth(pRoot->left);
        int right = TreeDepth(pRoot->right);
        int diff=left-right;
        if(diff>1||diff<-1)
            return false;
        return IsBalanced_Solution(pRoot->left)&&IsBalanced_Solution(pRoot->right);
    }
};
```
后来想了下，这个方法不太好，因为越靠近叶子的结点都被多次遍历，浪费时间。
所以我们可以采用后序遍历的方式遍历二叉树的每一个节点，在遍历到一个节点之前我们就已经遍历了它的左右字数。此时，记录每个节点为根节点的树的高度，就可以一边遍历一边判断每个节点是不是平衡的。
## C++实现
```
class Solution {
public:
    bool IsBalanced(TreeNode* pRoot, int* depth){  
        if(pRoot== nullptr){  
            *depth = 0;  
            return true;  
        }  
        int nLeftDepth,nRightDepth;  
        bool bLeft= IsBalanced(pRoot->left, &nLeftDepth);  
        bool bRight = IsBalanced(pRoot->right, &nRightDepth);  
        if (bLeft && bRight){  
            int diff = nRightDepth-nLeftDepth;  
            if (diff<=1 && diff>=-1)
            {  
                *depth = 1+max(nLeftDepth,nRightDepth);  
                return true;  
            }  
        }   
        return false;  
    } 
    bool IsBalanced_Solution(TreeNode* pRoot) {
        int depth = 0;  
        return IsBalanced(pRoot, &depth);  
    }
};
```
