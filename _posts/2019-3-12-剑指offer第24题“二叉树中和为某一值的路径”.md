---
layout:     post
title:      "剑指offer第24题“二叉树中和为某一值的路径”"
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
# 二叉树中和为某一值的路径
## 题目描述
输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)
***
## 思路
这题做了我好久，结果发现是题目看错了，因为是固定根节点到叶子节点，所以递归遍历所有情况即可。节点和可以通过target不断减去root->val最后是否为0判断。
path.erase(path.end()-1);写成path.erase(path.begin()+path.size()-1);就会爆编译错误，看了我好久
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
    vector<vector<int> > pathList;
    vector<int> path;
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        if(root==nullptr)
            return pathList;
        path.push_back(root->val);
        if(root->left==nullptr&&root->right==nullptr&&root->val==expectNumber){
            pathList.push_back(path);
        }
        if(root->left!=nullptr&&root->val<=expectNumber){
            FindPath(root->left,expectNumber-root->val);
        }
        if(root->right!=nullptr&&root->val<=expectNumber){
            FindPath(root->right,expectNumber-root->val);
        }
        path.erase(path.end()-1);
        return pathList;
    }
};
```

