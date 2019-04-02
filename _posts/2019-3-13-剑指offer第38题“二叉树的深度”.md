---
layout:     post
title:      "剑指offer第38题“二叉树的深度”"
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
# 二叉树的深度
## 题目描述
输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。
***
## 思路
当前结点的深度就是1+左结点与右结点的最大深度。所以递归一下子，如果是空指针就返回0.
## C++实现
```
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
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
};
```


