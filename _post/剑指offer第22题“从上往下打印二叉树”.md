---
layout:     post
title:      "剑指offer第22题“从上往下打印二叉树”"
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
# 从上往下打印二叉树
## 题目描述
从上往下打印出二叉树的每个节点，同层节点从左至右打印。
***
## 思路
BFS广搜模板题。。没啥好说的吧
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
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> vc;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* temp=q.front();
            q.pop();
            if(!temp) 
                continue;
            vc.push_back(temp -> val);
            q.push(temp -> left);
            q.push(temp -> right);
        }
        return vc;
    }
};
```

