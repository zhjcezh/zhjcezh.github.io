---
layout:     post
title:      "剑指offer第17题“树的子结构”"
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
# 树的子结构
## 题目描述
输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）
***
## 思路
既然是树。。。那我们继续递归呀。
HasSubtree方法用来判断A树的节点的值是否和B树的根相等，不相等就递归判断节点的左儿子和右儿子是否和B树根相等
找到相等后，就开始判断是否是子树。用isSon
如果A为空了，B还有就返回false。
如果B空了就说明子树稳了。
如果AB节点不相等，也返回false。
然后递归判断左右子树是否分别相等。

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
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        boolean fg=false;
        if (root1!=null&&root2!=null) {
            if (root1.val==root2.val) {
                fg=isSon(root1,root2);
            }
            if (!fg){
                fg=HasSubtree(root1.left,root2);
            }
            if (!fg){
                fg=HasSubtree(root1.right,root2);
            }
        }
        return fg;
    }
    public boolean isSon(TreeNode root1, TreeNode root2) {
        if (root1==null&&root2!=null)
            return false;
        if (root2==null)
            return true;
        if (root1.val!=root2.val)
            return false;
        return isSon(root1.left,root2.left)&&isSon(root1.right,root2.right);
    }
}
```

