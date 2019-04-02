---
layout:     post
title:      "剑指offer第18题“二叉树的镜像”"
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
# 二叉树的镜像
## 题目描述
操作给定的二叉树，将其变换为源二叉树的镜像。
***
## 思路
既然是树。。。那我们继续递归呀。
先交换根的左右子树，然后递归左右儿子，如果节点是空的就返回。
## Java递归实现
```
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    public void Mirror(TreeNode root) {
        if(root==null)
            return;
        TreeNode left = null;
        TreeNode right = null;
        left = root.right;
        right = root.left;
        root.right=right;
        root.left=left;
        Mirror(left);
        Mirror(right);
    }
}
```

